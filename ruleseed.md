## What is ruleseed, and how do I add it to my mod?
### Long Story Short:
Rule Seed is a feature of modded KTaNE that uses the [Rule Seed Modifier](<https://steamcommunity.com/sharedfiles/filedetails/?id=2037350348>) mod to dynamically change the rules and data inside of a module, like conditions, tables, or numbers. See [the repository's Glossary](<https://ktane.timwi.de/More/Glossary.html#rule-seed>) for more details.

It uses a number, called *seed*, to shuffle all of that data. Using the same seed will always result in the same rules and data for the modules. The usual "default" Rule Seed, and the way you play when not using Rule Seed, is 1.

It has two different parts: the Module and the Manual. Both have to be coordinated so that using the same seed, for example "6502", will result in the same data inside of the module as what's shown on the manual.

---

### Long Story:
To ensure the same seed results in the same values every time **and** for both manual and modules, pseudo-random actions such as generating a random number or shuffling lists must be done using a common predictable source of randomness, as shown below.\
This randomness can then be used to shuffle tables of data, generate mazes, randomize rules or decide which edgework your module will focus on.

Below are some general steps to add Rule Seed to a module:

1. **The Concept:**
Not all modules can be ruleseeded. Modules with very few rules (Turn The Key), no associated data to vary (Aquarium) or that already random (Custom Keys) cannot easily be ruleseeded.\
Modules that rely on real-life data (The London Underground, Timezones), while they *can* technically be randomized, might or might not be considered as candidates for Rule Seed.

Rule Seed can go further than just shuffling a table. Ask yourself "what can I change about my module while keeping its core idea?".\
Possible answers to that question, with associated examples you can check out, include:
* Shuffle Tables (Who's on First, Colored Squares)
* Shuffle Rules (Wires, The Bulb)
* Vary Numbers (Cheap Checkout, Sky Plate)
* Change Edgework Checks (Radiator, Ladder Lottery)
* Generate Mazes (Crazy Maze, Red Arrows, USA Maze)
* Add new possible Symbols (X-Ray, Interpunct)
* Add or rearrange other type of data (Lion's Share, Maritime Flags, Presidential Elections, Genre Divination)

However, Rule Seed probably shouldn't change the core concept of your module: the Wires module shouldn't start asking for multiple wires to be cut to solve with some ruleseeds, and Morse Code shouldn't ask you to do math with numbers it transmits to know which frequency to submit.

2. **The Module (In Unity):**
    1. Attach to your Module Prefab the `KMRuleSeedable.cs` script. It will allow you to test a specific Rule Seed in Test Harness. The value you use will have **no bearing** on the actual Rule Seed used in KTaNE, it is just for testing in Unity.
    2. Get a reference to your KMRuleSeedable in your script, get the seed, and use it to shuffle information. An example implementation is:
    ```cs
    /// <summary> Component to handle Rule Seeds </summary>
    [SerializeField] KMRuleSeedable ruleseedManager;

    void Start()
    {
	    MonoRandom rng = ruleseedManager.GetRNG();
	    Debug.LogFormat("[InsertModuleNameHere #{0}] Using Rule Seed {1}:", InsertModuleIdHere, Rng.Seed);
	
	    if (rng.Seed == 1)
	    {
		    // Here, initialize the default values that are used for Rule Seed 1
		    // Those are the rules used by players if they don't use Rule Seed, which is the majority of times,
		    // so manually designing those to be as fun as possible is a good idea!
	    }
	    else
	    {
		    // Shuffle your rules and values here otherwise
	    }
    }
    ```
    3. Use the seed to shuffle data about your module.

    If you need to generate a random number, use `rng.Next(minimumInclusive, maximumInclusive);`.\
    If you need to shuffle an Array or List, use `rng.ShuffleFisherYates(ArrayName);`.\
    Using the `MonoRandom` gathered from the `KMRuleseedable` to do those two actions ensures that using the same ruleseed will always result in the same actions, and makes it easier to synchronize with the Manual since equivalent methods exist in JavaScript.

    You can check out the [API for the Rule Seed Modifier mod](https://github.com/CaitSith2/KTANE-mods/wiki/RuleSeedModifier) for more.

3. **The Manual (In Html & JavaScript):**
    1. Add to your Manual `<script src="js/ruleseed.js"></script>` at the top to tell the Manual to load the code for Rule Seed.
    2. Implement the functions for handling the Default Values using `function setDefaultRules(){}` and the Ruleseeded shuffled values using `function setRules(rnd){}`.
        * If you have text strings that should be translated, it is preferred that you separate scripts into a separate JavaScript file while keeping the strings easily available in the html for translators to manage. See Noise Identification's [HTML](<https://ktane.timwi.de/HTML/Noise%20Identification.html>) and [JavaScript](<https://ktane.timwi.de/HTML/js/Modules/Noise%20Identification.js>) as an example.
        * Otherwise, if your ruleseed affects only numbers or symbols, or has very little text, you can implement them directly in your manual html. See [Green Arrows](<https://ktane.timwi.de/HTML/Green%20Arrows.html>) or [Icicle Plate](<https://ktane.timwi.de/HTML/Icicle%20Plate.html>) for examples.\
    An example implementation is:
    ```js
    let originalTable = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"];

    function setRules(rnd){
      var shuffledTable = originalTable.slice() // .slice() to create a copy of the array instead of referencing it
      rnd.shuffleFisherYates(shuffledTable);
	
      var cells = document.querySelectorAll(".data-table td"); // Get all td inside of the object with class "data-table"
      for (var i = 0; i < 10; i ++)
        cells[i].innerText = shuffledTable[i];
    }

    function setDefaultRules(){
      var cells = document.querySelectorAll(".data-table td");
      for (var i = 0; i < 10; i ++)
        cells[i].innerText = originalTable[i];
    }
    ```
    3. Use the seed to shuffle data abour your module.
    
    If you need to generate a random number use `rnd.next(minimumInclusive, maximumInclusive);`.\
    If you need to shuffle an Array or List, use `rnd.shuffleFisherYates(ArrayName);`.\
    Using those methods ensures the same result as `.Next()` or `.ShuffleFisherYates()` in C# which helps synchronizing modules and manuals.