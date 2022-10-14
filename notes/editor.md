# Editor


## About Using The Assets/Editor Folder

Editor scripts use the `UnityEditor` namespace, which doesn't get used when compiling your build. What this means, is that if you have UnityEditor code in your regular scripts, your project will throw errors on build. There are two ways to get around this:

Place your scripts that use editor code in `Assets/Editor`, or wrap your UnityEditor code in `#IF` `#ENDIF` preprocessor defines:

```cs
#if UNITY_EDITOR
	Debug.Log("This will only run in editor, not in build");
#endif
```


See also: [Conditional Compilation](https://docs.unity3d.com/Manual/PlatformDependentCompilation.htmlB)


## A Function That Runs Before A Build Is Started

```cs
using UnityEditor.Build;
using UnityEditor.Build.Reporting;

public class OnBuildTest : IPreprocessBuildWithReport {
    public int callbackOrder { get { return 0; } }

	public void OnPreprocessBuild(BuildReport report) {
		// Some code to run 
	}
}

```

See also: [Unity Docs: IPreprocessBuildWithReport.OnPreprocessBuild](https://docs.unity3d.com/ScriptReference/Build.IPreprocessBuildWithReport.OnPreprocessBuild.html)


## How to Render A Position Handle in Editor

This script will render a position handle in the scene. If the user drags it around, the new position will be saved in the source component.

```cs
public class ExampleScript : MonoBehaviour {
	public Transform someTransform;
}

[CustomEditor(typeof(ExampleScript)), CanEditMultipleObjects]
public class ExampleScriptEditor : Editor {
    protected virtual void OnSceneGUI() {
		// Fetch a reference to the component
        ExampleScript example = (DotProductVisualizationExampleScript;

        EditorGUI.BeginChangeCheck();

		// Draw a position handle at a given world space position
        Vector3 pos = Handles.PositionHandle(example.someTransform.position, Quaternion.identity);

		// Tip: You can tweak/limit the position here if it fits your needs.
		// For example, you could normalize the position here, or restrict the length of the vector so that it's at most x meters from the parent position.

		// If the user made a change, then record it, and update the position in the original component
        if (EditorGUI.EndChangeCheck()) {
            Undo.RecordObject(example, "Handles Change");
            example.someTransform.position = pos;
        }
    }
}
```

See also: [Unity Docs: Handles.PositionHandle](https://docs.unity3d.com/ScriptReference/Handles.PositionHandle.html)

## How to Render A Rotation Handle in Editor

This script will render a rotation handle in the scene. If the user drags it around, the new rotation will be saved in the source component.

```cs
public class ExampleScript : MonoBehaviour {
	public Transform someTransform;
}

[CustomEditor(typeof(ExampleScript)), CanEditMultipleObjects]
public class ExampleScriptEditor : Editor {
    protected virtual void OnSceneGUI() {
		// Fetch a reference to the component
        ExampleScript example = (DotProductVisualizationExampleScript;

        EditorGUI.BeginChangeCheck();

		// Draw a rotation handle at a given world space position
        Quaternion rot = Handles.RotationHandle(example.someTransform.rotation, example.someTransform.position);

		// If the user made a change, then record it, and update the position in the original component
        if (EditorGUI.EndChangeCheck()) {
            Undo.RecordObject(example, "Handles Change");
            example.someTransform.rotation = rot;
        }
    }
}
```

See also: [Unity Docs: Handles.RotationHandle](https://docs.unity3d.com/ScriptReference/Handles.RotationHandle.html)
