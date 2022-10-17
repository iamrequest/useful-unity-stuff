# General Unity

## Debug Logging Over Time, Using Animation Curves

An easy way to show the rate of change over time, is to use an animation curve to plot values on a graph:

```cs
public AnimationCurve logGraph;

public void Update() {
	logGraph.AddKey(Time.time, transform.position.y);
}
```

<p align="center">
	<img src="/images/animation-curve-logging.png">
</p>


## How to Run Some Code Every X Frames (Instead of Every Frame)

Sometimes you want some code to run in Update(), but only every other frame:

```cs
public const int FRAME_INTERVAL;

private void Update() {
	if(Time.frameCount % FRAME_INTERVAL == 0) {
		// ...
	}
}
```

## How to Raycast

I have to look up the syntax every time I raycast


```cs
RaycastHit hit;

Transform originTranform; // The transform from where we'll raycast from, in the direction of originTransform.forward
float maxDistance = 100; // The distance to raycast

if (Physics.Raycast(originTransform.position, originTransform.forward, out hit, maxDistance, layerMask)) { 
}
```


## How to Compare Layermasks

```cs
// Check if layers overlap
if ((myLayerMask.value & (1 << collision.collider.gameObject.layer)) > 0) { }

// Check if layers do not overlap 
if ((myLayerMask.value & (1 << collision.collider.gameObject.layer)) == 0) { }
```

## About Coroutines

Coroutines can be used to run some function across multiple frames.

```cs
Coroutine someCoroutine;

public void DoSomething() {
	// Optional: Stop the coroutine if it's already running.
	//	You can remove this if you want to allow multiple instances of the coroutine to run at the same time
	if (someCoroutine != null) {
		StopCoroutine(someCoroutine);
		someCoroutine = null;
	}

	// Call the coroutine. Caching a reference to the coroutine is optional
	someCoroutine = StartCoroutine(_DoSomething());
}

public IEnumerator _DoSomething() {
	// Wait for 1 second
	yield return new WaitForSeconds(1f);
	
	// Wait for the next frame
	yield return null;
	
	// Return from the coroutine
	someCoroutine = null;
	yield break;
}
```


See also: [Unity Docs: Coroutines](https://docs.unity3d.com/Manual/Coroutines.html)


## How to Rename a Unity Project

  - In your file explorer, change the unity project's folder name
  - Remove all files with a `.sln` or `.csproj` file extension. Unity will re-create them when you open the project again.
  - Open the unity project

## Singleton Pattern

Note: Singletons are powerful and convinient, but they are shouldn't be overused ([stackoverflow: are singletons really that bad?](https://stackoverflow.com/a/1020384)). Singletons lead to tight coupling and spaghetti code, so use them with caution.

```cs
public class SingletonClass : MonoBehaviour {
	public static SingletonClass Instance { get; private set; }

	private void Awake() {
		if (Instance == null) {
			Instance = this;
		} else {
			Debug.LogError($"Multiple {GetType()} components detected. This is probably a bug.");
			Destroy(this);
		}
	}
}
```
