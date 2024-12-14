---
layout: post
title: Unity Custom Editor Tool
date: 2024-12-12 16:40:50
author: dochi486
categories: Unity
published: true
short_description: 모든 매테리얼의 쉐이더를 한 번에 바꿔주는 커스텀 에디터 툴
---

### 유니티 커스텀 에디터

- 지원되지 않는 쉐이더를 한 번에 바꾸는 커스텀 에디터 툴

```cs
using System.Collections.Generic;
using UnityEngine;
using UnityEditor;
using System.Linq;


public class MaterialChange : EditorWindow
{
    public static Shader oldShader;
    public static Shader newShader;

    // 모든 매태리얼 중에서 Stylized Lit으로 설정된 메테리얼 쉐이더를
    // URP의 Lit으로 바꿔준다.
    [MenuItem("Window/3. Change Broken Shader")]
    static void Init()
    {
        MaterialChange window = (MaterialChange)EditorWindow.
          GetWindow(typeof(MaterialChange));
        window.Show();
    }

    private void OnGUI()
    {
        GUILayout.Label("Change Broken Material into Working Material",
         EditorStyles.boldLabel);
        oldShader = (Shader)EditorGUILayout.ObjectField("Broken Shader" ,
         oldShader, typeof(Shader) , true);
        newShader = (Shader)EditorGUILayout.ObjectField("New Shader",
         newShader, typeof(Shader), true);

        if(GUILayout.Button("Change Shader"))
            ChangeShader();
    }

    public static void ChangeShader()
    {
        List<GameObject> currentSceneObjects = new List<GameObject>();

        currentSceneObjects = FindObjectsByType<GameObject>(FindObjectsSortMode.None)
        .ToList();

        foreach(var item in currentSceneObjects)
        {
            Renderer renderer = item.GetComponent<Renderer>();

            if(renderer != null && renderer.sharedMaterial != null)
            {
                renderer.sharedMaterial.shader = newShader;
            }
        }
    }
}
```
