## What is Twitch Plays?

KTANE Twitch Plays is a way to expert for KTANE while using Twitch.tv as an interface. A livestream of a bomb is happening, you can get reads for the edgework and modules directly from the stream, and input commands through the Twitch chat which are processed by the modules.

The current common place for KTANE TwitchPlays players to regroup is on the [TP KTANE Discord Server](https://discord.gg/QJfr3xA), and playing is currently done on [MrPeanut1028](https://www.twitch.tv/mrpeanut1028)'s livestream. There is also a [general list of available commands here](https://samfundev.github.io/KtaneTwitchPlays/).

Solving modules awards points based on their scores, with a higher score being given to harder and longer modules.\
Module scores are continuously being monitored and maintained, and can be found in the [Official Twitch Plays Scoring Sheet](https://docs.google.com/spreadsheets/d/1G6hZW0RibjW7n72AkXZgDTHZ-LKj0usRkbAwxSPhcqA/edit?gid=0#gid=0).

Here is a [great (but slightly outdated) tutorial](https://www.youtube.com/watch?v=_VT0DicIiU0) explaining the basics of Twitch Plays.

## What is needed for a module to have Twitch Plays support?

**[Here is a link to the documentation](https://github.com/samfundev/KtaneTwitchPlays/wiki/External-Mod-Module-Support) for much more details.**

The main requirement is for the module to implement both the `ProcessTwitchCommand(string command)` method and the `string TwitchHelpMessage` variable.

When a user sends a command targetting your module, Twitch Plays will call `ProcessTwitchCommand`, passing along the command as a parameter. It's then up to you to convert this command into button presses and other interactions.

The `TwitchHelpMessage` is what users will see when they use the `!1 help` command in the chat, and is supposed to describe in details what the available commands are, what they do, and how to format them.\
You can use `{0}` to mark where your module is should be placed, such as `Press the button with !{0} press.` which will be formatted as `Press the button with !14 press.` if your module has been given ID 14 on the current bomb.

Here is an example for both of those elements in a fake module:

```c#
string TwitchHelpMessage = "Press one of the numbered buttons 0-9 with “!{0} press 6”.";
bool bModuleSolved;

IEnumerator ProcessTwitchCommand(string command)
{
  // Log the received command, in case in-depth debugging is needed. Check the Logging article for more information.
  Debug.LogFormat("<Module Name #{0}> Received Command “{1}”", moduleId, command);
  if (bModuleSolved) { return null; }

  // Split every word into their own lowercase string. Credit to Royal_Flu$h.
  string[] commandParts = command.ToLowerInvariant().Split(new[] { ' ', ',', ';' }, StringSplitOptions.RemoveEmptyEntries);

  // Even if not specified, allow shortcut commands like "!1 s" instead of the full "!1 submit"
  if (commandParts[0] == "submit" || commandParts[0] == "s" || commandParts[0] == "press" || commandParts[0] == "p")
  {
    if (commandParts.Length < 2)
    {
      return null;
    }

    int buttonIndexToPress == -1;
    int.TryParse(commandParts[1], out buttonIndexToPress);

    if (buttonIndexToPress == -1)
    {
      return null;
    }

    return new KMSelectable[] { numberedButtonSelectables[buttonIndexToPress] };
  }
}
```

It is not a perfect implementation, but it accounts for people using uppercase letters, shortcuts or using "submit" instead of "press" as it's a common keyword.

A more robust implementation would probably use Regex to check for commands, include additional safety checks and send more error messages in chat depending on what is wrong with the user command; but this is a good general purpose implementation template.

### Process Twitch Command as an IEnumerator

Many implementations of Twitch Plays require more advanced actions than just pressing some buttons in a row. In this case, we implement `ProcessTwitchCommand()` to return an `IEnumerator` instead of a `KMSelectable[]`.

This allows us to:
- Send messages back to chat (for example to explain why a command isn't formatted correctly)
- Wait in-between presses (for example *The Stopwatch* or *Light Cycle*)
- Press buttons at specific times (for example *Crazy Talk* or *Press X*)
- Keep a button held instead of just pressing it once (for example *The Button* or *Hold On*)
- Send bonus points for solves (for example *Souvenir* or *Iconic*)
- Have an animation in-between the final interaction and its solve (for example *Red Arrows* or *Cheap Checkout*)
- Rotate the bomb to see the module with different angles (for example *Perspective Pegs* or *Perpecticolored Squares*)
- Etc.

The [Twitch Plays Documentation](https://github.com/samfundev/KtaneTwitchPlays/wiki/External-Mod-Module-Support#advanced-implementation) has a full list of the possibilities.

One specific detail to think about is interacting with your KMSelectables:

You must interact with them manually using `buttonSelectable.OnInteract()` if a specific interaction (holding for some time) isn't needed.\
You must also send `yield return null` before any interaction with your module, as not returning anything will make TP consider the submission as invalid.

### Implementing a custom Autosolver

The `void TwitchHandleForcedSolve()` method is called by Twitch Plays when your module is "Forced Solved", which is when it receives the command to solve by an admin.

It is not necessary to implement, as the module will just receive a `HandlePass()`; however if you want to implement a custom Autosolver that presses the buttons necessary to solve the module (for additional flair) or to edit what is shown on the module to correspond to its finalized solved state, you can do so within that method.

This method **should not**, under any circumstance, strike. If a Forced Solve strikes, it will get reported to you as a bug needed to be fixed.

This method should also end up with your module solving.

A Needy needs to either keep its timer at 99 seconds forever, or get disarmed every time it activates, depending on the Needy.

## Other features and good-to-know elements about KTANE TP Support.

### Dynamic Scoring
You can use the command `yield return awardpoints 30;` to award 30 bonus points immediately, or `yield return awardpointsonsolve 30;` to award 30 bonus points when the module solves.

Using this in combination with variable usage, you can get dynamic scoring by altering the amount of points you give depending on states such as number of stages given, questions asked, or other module-specific variables.

Examples of modules implementing Dynamic Scoring include [Cipher Machine](https://github.com/obachs971/CipherCollection/blob/main/Assets/Scripts/cipherMachine.cs#L605) or [100 Levels of Defusal](https://github.com/Espik/100-Levels-of-Defusal/blob/master/Assets/100%20Levels%20of%20Defusal/OneHundredLevelsOfDefusal.cs#L1162).\
You can also find those in the [Official Twitch Plays Scoring Sheet](https://docs.google.com/spreadsheets/d/1G6hZW0RibjW7n72AkXZgDTHZ-LKj0usRkbAwxSPhcqA/edit?gid=0#gid=0).

### Delay between final interaction and the module's solving

If your module has a delay between the final player interaction and the time where the module solves, it can misattribute solves to the streamer instead of the user.

To avoid this, you **must** use the Advanced Implementation returning an `IEnumerator`, and right after the button is pressed (but before the `ProcessTwitchCommand()` returns), you must send a `yield return "solve";`.

This asks Twitch Plays to remember who send the last command, and attributes the solve points to them.

### Calling `ProcessTwitchCommand()` from another `ProcessTwitchCommand()`
In some cases, such as modules contained within other modules, you might need to call a `ProcessTwitchCommand` from within another `ProcessTwitchCommand`. In the case of returning `KMSelectable[]`, this is fairly simple; but in the case of `IEnumerator`, it is slightly more complex and a basic understanding of them  is needed:

IEnumerator is a type that steps through code and executes it. When the Twitch Plays mod receives a command and sends it to your module to process it, it gets back an IEnumerator and executes the code inside of it.

In this instance, when you want to call `ProcessTwitchCommand()` from another `ProcessTwitchCommand()`, you cannot just get the `IEnumerator` and return it directly, you must process the `IEnumerator` manually. 

This can be done through this example code: 

```c#
[SerializeField] KtaneModuleScript scriptToCall;

IEnumerator ProcessTwitchCommand(string command)
{
  // Get the IEnumerator from the desired script
  IEnumerator commandProcessToRun = scriptToCall.ProcessTwitchCommand(command);

  // IEnumerator starts initialized with an "index" or -1
  // First we must MoveNext to get to the first yield return
  // MoveNext() returns true while there's valid code to be executed
  while (commandProcessToRun.MoveNext())
  {
    // Yield Return what the script does inside
    yield return processedCommand.Current;
  }
}
```

One such example of a module needing this implementation is [Allmighty Sinnoh](https://github.com/thunder725/MythicalPlates/blob/main/Assets/MythicalPlates/Scripts/AllmightySinnoh.cs#L1282), a module which summons other modules from its own selection.

### Rotating the Bomb through the Advanced Implementation

You can yield return a rotation through a `Quaternion` or a `Quaternion[]` to rotate the bomb.

You can use `Quaternion.Euler(x, y, z)` to get a Quaternion from 3 angles in degrees;

Those rotations are **relative** to the bomb's rotation at the start of the command (with your module facing towards the camera). The bomb also automatically resets to its starting rotation, so there is no need to do it manually.