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
