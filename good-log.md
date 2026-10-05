## First off, what is Logging?

If you already know what a module's logging is, skip to the next section.

A Log is a group of messages your module leaves so that players, often using the [Log File Analyzer](https://ktane.timwi.de/More/Logfile%20Analyzer.html), can help themselves understanding how your mod works.\
Often times, people check the log to:
- Verify what answer the module expected them to submit.
- Check how the module got to that answer.
- See why their submission sent a Strike when they expected a Solve.
- Verify if the Strike they got was an error on their part or a bug in the module.

Having a high-quality Log helps people save time while reading it. It can also help players learn and understand your module more easily, which in turns results in more people playing your module!


## How to make a quality Log for my module?

There is no one-size-fit-all formula, but here are some general points:

- Log everything that a player could possibly need to solve the module blind:
  - All pieces of information the module gives, including how they are represented (sound, colour, shapes, text...).
  - All expected solving steps, in order.
  - The final solution, including how to input it on the module.
- Whenever the Defuser does an input, log both what was inputted and what the result was (movement, Strike, Solve, etc). This will help you notice if there are bugs inside of your module's code when unexpected Strikes happen.
- If your module has steps that are done one-way in the module but that the player must do in reverse (like a cipher encrypting and decrypting), log the information from the player's point of view, not from the module's.\
You can always give the module's point of view using a [hidden Log](#hidden-logs-in-lfa), but the main info you show the player must be usable without conversion.
- If your module implements Rule Seed, log the information that is expected to be seen on the manual such as the selected rules or the shuffled tables. You can do this using [hidden Logs](#hidden-logs-in-lfa) to avoid flooding the Log.

### Log Examples
#### "Bad" Log:

```
- Initializing Module...
- 6 wires
- cut fourth wire
- Struck
- module solved
```
- Doesn't give the full read (what are the wires' colours?).
- Doesn't explain how to get the expected answer
- Doesn't say why the module struck (was it an incorrect solution? did the defuser misinput? is the module bugged?)
- Doesn't say why the module solved

#### "Good" Log:

```
- Using Ruleseed 1:
- There are 6 wires.
- Wire colours (top to bottom) are: Blue, Black, Yellow, White, Black, White
- Rule "exactly one Yellow wire and more than one White wire" applies.
- To solve the module, cut the fourth wire (Yellow).
- You cut the second wire (Black). Expected the fourth (Yellow). Strike given.
- You cut the fourth wire (Yellow). That is correct. Module Solved.
```
- Logs the Rule Seed to ensure no issues can be caused by an unexpected mismatch.
- Gives the full read in detail, specifying that the colours are given top to bottom to avoid confusion.
- Explicits what the rule's condition and subsequent expected answer is.
- When receiving an input, tells what was done, what was expected, and what the result is.

## Making a Log appear in the Log File Analyzer

For your Log to be picked up by the LFA, it needs to follow a specific syntax:

```[<MODULE NAME> #<MODULE ID>] <MESSAGE>```

For example: `[The Bulb #1] Bulb is Purple, see-through and off. I is on the left.`

Your Module Name should ideally be the same as the Display Name you set up in Unity (in the KMBombModule component). If it doesn't match, it can be fixed afterwards with a custom LFA, but it's easier if you don't have to do it!

Your Module ID is a positive integer which should be unique for *each instance* of the module. This means that if a bomb has your module three times, one should log using ID 1, another using ID 2, and another using ID 3.

To set this up easily, and make your module compatible with the Log File Analyzer, here is a recommended setup:

```c#
// Logging Data
static int moduleIdCounter = 1;
int moduleId;

void Awake()
{
  // Initialize Logging
  moduleId = moduleIdCounter++;
}
```

You can then create a Method to format your Logging automatically, it's not necessary but it can save your time. Here is a possible implementation:

```c#
void ModuleLog(string message)
{
    Debug.LogFormat("[Genre Divination #{0}] {1}", moduleId, message);
}
```

Having your Module ID be in a variable named "moduleId" is important as the Log File Analyzer will actually grab it to recognize which of your modules struck or solved if multiple are on the same bomb.

### Hidden Logs in LFA

If your Module needs to log a lot of information, such as entire tables (and you should), logging using `<MODULE NAME #ID>` with `<>` instead of `[]` will not show it in the actual LFA while still making it appear in the Filtered Log.

Here is an example implementation that allows both types of Logging:
```c#
void ModuleLog(bool LogInLfa, string message)
{
    if (LogInLfa)
    { Debug.LogFormat("[Genre Divination #{0}] {1}", moduleId, message); }
    else
    { Debug.LogFormat("<Genre Divination #{0}> {1}", moduleId, message); }
}
```


## How can I go beyond and add pictures, tables or diagrams to my log?

For this, you'll need to have a Custom Log File Analyzer! You can either implement it yourself (#TODO add link to the written topic), or ask people in the LFA Support Thread inside of #repo-requests in the KTANE Discord.

Link to the [LFA Support Thread](https://discord.com/channels/160061833166716928/1018575584009400350).\
Invite to the [Discord](https://discord.gg/K6uQMyBcYZ).