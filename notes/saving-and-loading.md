# Saving and Loading

This approach involves creating a serializable vanilla class, that will be serialized into binary format, and then read/written to/from the disc. The file dir will vary depending on the build platform (See: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html)), but on windows it typically points to `%userprofile%\AppData\LocalLow\<companyname>\<productname>`

You'll need to either store one large serializable class, or a separate serializable class for each context.

Note: Some var types (Vector3, Quaternion, Color) cannot be serialized. In those cases, you'll need to create an array of floats or something. 

Source: [Brackeys: save & load system in Unity](https://youtu.be/XOjd_qU2Ido)

## The Object to Save/Load To/From the Filesystem

```cs
[System.Serializable]
public class PlayerData {
	public int level;
	public int currentHealth;
	public float[] position;
	
	public PlayerData(Player player) {
		// ...
		position = new float[3];
		position.x = player.transform.position.x;
		position.y = player.transform.position.y;
		position.z = player.transform.position.z;
	}
}
```

## The Class to Save And Load

```cs
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

// No monobehaviour needed.
public static class SaveSystem {

	public static void SavePlayer(Player player) {
		// The filename and extension doesn't matter
		string filename = "player.data";
		string path = Application.persistentDataPath + "/" + filename;

		BinaryFormatter formatter = new BinaryFormatter();
		FileStream stream = new FileStream(path, FileMode.Create);

		// Populate the serializable class (PlayerData) here
		PlayerData = new PlayerData(player);

		// Write the data
		formatter.serialize(stream, data);
 
		// Always close the file
		stream.Close();
	}

	public static PlayerData LoadPlayer() {
		string filename = "player.data";
		string path = Application.persistentDataPath + "/" + filename;

		// Check if file exists
		if(! File.Exists(path)) {
				Debug.LogError("File does not exist in " + path);
				return null;
		}

		BinaryFormatter formatter = new BinaryFormatter();
		FileStream stream = new FileStream(path, FileMode.Read);

		// Read from the file, cast as some object
		PlayerData = formatter.Deserialize(stream) as PlayerData;

		// Always close streams
		stream.Close();

		return PlayerData;
	}
}
```
