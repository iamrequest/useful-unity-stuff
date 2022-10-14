# Editor

## About Using The Assets/Editor Folder

Editor scripts use the `UnityEditor` namespace, which doesn't get used when compiling your build. What this means, is that if you have UnityEditor code in your regular scripts, your project will throw errors on build. There are two ways to get around this:

Place your scripts that use editor code in `Assets/Editor`, or wrap your UnityEditor code in #IF #ENDIF preprocessor defines:

```cs
#if UNITY_EDITOR
	Debug.Log("This will only run in editor, not in build");
#endif
```


See also:
	* [Conditional Compilation](https://docs.unity3d.com/Manual/PlatformDependentCompilation.htmlB)

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

See also:
	* [Unity Docs: IPreprocessBuildWithReport.OnPreprocessBuild](https://docs.unity3d.com/ScriptReference/Build.IPreprocessBuildWithReport.OnPreprocessBuild.html)
