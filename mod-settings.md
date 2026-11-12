## What are Mod Settings?

Mod Settings are specific parameters and options you let players edit to change how your mod (for example a module) behaves.

They usually are edited through the tablet from [Mod Selector](<https://steamcommunity.com/sharedfiles/filedetails/?id=801400247>), by using [Tweaks](<https://steamcommunity.com/sharedfiles/filedetails/?id=1366808675>).

Otherwise, they can be edited through the files at those locations:
- Windows: `%APPDATA%\..\LocalLow\Steel Crate Games\Keep Talking and Nobody Explodes\Modsettings`
- Mac: `~/Library/Application Support/com.steelcrategames.keeptalkingandnobodyexplodes/Modsettings`
- Linux: `~/.config/unity3d/Steel Crate Games/Keep Talking and Nobody Explodes/Modsettings`
- Linux (flatpak): `~/.var/app/com.valvesoftware.Steam/.config/unity3d/Steel Crate Games/Keep Talking and Nobody Explodes/Modsettings`
- Steam Deck (Proton): `~/.local/share/Steam/steamapps/compatdata/341800/pfx/drive_c/users/steamuser/AppData/LocalLow/Steel Crate Games/Keep Talking and Nobody Explodes/Modsettings`

Some examples of ModSettings include:
- [Fast Math](<https://github.com/Sirawit7205/ktaneFastMath/tree/master/Assets>): Edit how much time you have to submit an answer.
- [Forget Me Not](<https://github.com/Hexicube/KTANE-Hexi-s-Advanced-Modules/tree/master/Assets>): Enable or disable Keyboard inputs.
- [Morse-A-Maze](<https://github.com/CaitSith2/KTANE-mods/tree/master/MorsaAMaze>): Change which colours the LED uses to transmit morse.
- [Font Select](<https://github.com/rocket0634/Niko-Modules/tree/m/FontSelectModule/Assets>): Remove two similar fonts from the possibilities

Those settings only are created after launching the module once. Before that, they won't be in the game's files and won't appear in the tablet.

## How do I add Mod Settings to my mod?

Mod Settings for each mod are stored as a JSON file in the locations mentionned above.

There are two different ways of adding Mod Settings: The Ktane Modkit contains the KMModSettings Component which can read the settings and provide them as a raw string. There is also the ModConfig community plugin which replaces it, makes reading settings easier, and gives you more control over its content, such as having descriptions or a custom file name.

In all cases, you'll need to declare a Class that contains all settings you will want to be available to the player. Here is an example that will be used for the remainder of this file:

```c#
private class CustomModuleSettings
{
	// Set the default value in the case of errors
	public int Timer = 10;
}
```

The variables in this class have to be public.

### Using the KMModSettings component

Attach to your mod prefab the [KMModSettings](<https://github.com/Qkrisi/ktanemodkit/wiki/KMModSettings>) component, and leave both of its parameters `Settings` and `SettingsPath` empty. They'll get set by the game and you should only read them.

You should get a reference through it like you'd do for any component such as KMBombInfo or KMBombModule. In this case, the rest of this file will use `modSettings`.

If you are using a Service, you can use `modSettings.RefreshSettings()` to refresh the settings before reading them; but modules generally can just read them directly upon loading.

Reading the settings is done through reading `modSettings.Settings`. It gives you a string you must Deserialize (convert from JSON to Class), using `JsonConvert.DeserializeObject<CustomModuleSettings>(modSettings.Settings);`. Use the `using Newtonsoft.Json` assembly for it to work.\
It is recommended to do this within a try/catch statement to avoid errors due to parsing failures. You can check [Fast Math](<https://github.com/Sirawit7205/ktaneFastMath/blob/master/Assets/FastMathModule.cs#L391>) for an example of how to do this.

For your settings to be placed in the correct locations mentionned above and read by KTANE, you need to either place a json file manually with the correct naming scheme ("[ModId]-settings.json", where [ModId] is the ID entered in Assets/Editor/Resources/ModConfig); or to just create a file named "modSettings.json" with the correct settings in your mod bundle, which will be placed into the correct location automatically. 

For example, our file for the CustomModuleSettings would have this as its content:

```json
{
	"Timer": 10
}
```

### Using the ModConfig plug-in

ModConfig is part of the KTaNE Utils community plug-in, available directly in the [Community Modkit](<https://github.com/Qkrisi/ktanemodkit>) though the Plugins window (Keep Talking ModKit > Plugins > KtaNE Utilities (By samfundev).

It has multiple advantages, such as reading the settings just by calling a single method instead of handling the try/catch yourself, choosing the name of the JSON file that's created, and adding custom descriptions to each setting if wanted.

Using the same Class declared above, you can construct a new ModConfig using some code like `var config = new ModConfig<CustomModuleSettings>("customModuleSettings");`. The string you input is the name you want the final settings JSON file to have if none exist.

Getting the settings is as simple as calling `CustomModuleSettings customSettings = config.Read();`, and you get your Class directly. You can also manually ask for it to Write a new set of settings using `config.Write(customSettings);`.

To have custom descriptions that show up on the tablet and help players understand your settings, you need to declare a *static* field named `TweaksEditorSettings` of type `Dictionary<string, object>[]`. Here is its structure:

```c#
static Dictionary<string, object>[] TweaksEditorSettings = new Dictionary<string, object>[]
{
    new Dictionary<string, object>
    {
        { "Filename", "CustomModuleSettings.json" },
        { "Name", "Custom Module" },
        { 
			"Listings", new List<Dictionary<string,object>>
			{
				new Dictionary<string, object>
				{
					{ "Key", "Timer" }, // Name of the variable inside of the class / inside of the JSON
					{ "Text", "Timer Length" }, // User-friendly name of the variable, can contain spaces
					{ "Description", "Changes how long the timer allowing submissions is." } // User-friendly description of the setting
				}
				// Here, you can put a comma at the end of the previous entry and add another Dictionary<string, object> for your next setting
			}
		}
    }
};
```