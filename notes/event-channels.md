# Event Channels

## About 

Event Channels are a simple way to send events across your components, without needing an explicit reference to one-another.

We'll create a ScriptableObject that will contain your UnityActions. Downstream components will use a reference to this ScriptableObject to subscribe to, and to invoke, the UnityAction.


## Barebones Event Channel

```cs
[CreateAssetMenu(menuName = "Events/Simple Event Channel")]
public class GameStateEventChannel : ScriptableObject { 
	public UnityAction onEventRaised;

	public void RaiseEvent() {
		if(onEventRaised != null) {
			onEventRaised.Invoke();
		}
	}
}


public class EventChannelListener : MonoBehaviour {
	public GameStateEventChannel someEventChannel;

	// Subscribe and unsubscribe to events
	private void OnEnable() {
		eventChannel.onEventRaised += SomeFunction;
	}
	private void OnDisable() {
		eventChannel.onEventRaised -= SomeFunction;
	}


	private void SomeFunction() {
		// This gets called once the event channel is raised
	}
}

public class EventChannelRaiser : MonoBehaviour {
	public GameStateEventChannel someEventChannel;

	public void InvokeEventChannel() {
		// Invoke the UnityAction in the event channel
		someEventChannel.RaiseEvent();
	}
}

```

## Event Channels with Parameters

You can also pass values through event channels:

```cs
[CreateAssetMenu(menuName = "Events/Simple Event Channel")]
public class GameStateEventChannel : ScriptableObject { 
	public UnityAction<int> onEventRaised;

	public void RaiseEvent(int someValue) {
		if(onEventRaised != null) {
			onEventRaised.Invoke(someValue);
		}
	}
}


public class EventChannelListener : MonoBehaviour {
	public GameStateEventChannel someEventChannel;

	// Subscribe and unsubscribe to events
	private void OnEnable() {
		eventChannel.onEventRaised += SomeFunction;
	}
	private void OnDisable() {
		eventChannel.onEventRaised -= SomeFunction;
	}


	private void SomeFunction(int someValue) {
		// This gets called once the event channel is raised
	}
}

public class EventChannelRaiser : MonoBehaviour {
	public GameStateEventChannel someEventChannel;
	int exampleEventChannelParameter;

	public void InvokeEventChannel() {
		// Invoke the UnityAction in the event channel
		someEventChannel.RaiseEvent(exampleEventChannelParameter);
	}
}

```


See also:

* [Unity Youtube, Open Projects Devlog: Game Architecture with ScriptableObjects](https://www.youtube.com/watch?v=WLDgtRNK2VE)
* An example from one of my games, Disco Queen: Dancing FeVR, that heavily used event channels:
	* [Event channel definition](https://github.com/iamrequest/disco-queen-dancing-fever/blob/master/Assets/EventChannels/gameState/GameStateEventChannel.cs)
	* [Event channel usage](https://github.com/iamrequest/disco-queen-dancing-fever/blob/master/Assets/Scripts/gameState/GameManager.cs#L7)
