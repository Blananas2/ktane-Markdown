## Building in Unity

[Full Tutorial TBA]

## Publishing on the Steam Workshop

[Full Tutorial TBA]

## Publishing on the Repository

You're gonna want to have your module visible on the [Repository](<https://ktane.timwi.de/>) and have its manual accessible by everyone. For this you need to prepare the correct files in the correct folder hierarchy, and then publish them either on Discord asking maintainers to add it for you, or through a GitHub Pull Request to make the process faster.

### The Correct Files

In order to keep the Repository clean, all files must be kept in the correct folder hierarchy. If some folders are empty in your version because the files in them aren't necessary, you can not have them.

The Hiererachy is as follows:
- :file_folder: `HTML`
  - :file_folder: `css`
    - :file_folder: `Modules`
      - :clipboard: `Module Name.css` - If you have custom CSS outside of the main HTML page (only really applicable for modules with lots of rules like RPS Judging or Font Select, or packs of modules sharing the same CSS such as the Mythical Plates)
  - :file_folder: `font` - All of your custom fonts should be placed there, not renamed.
  - :file_folder: `img`
    - :file_folder: `Component`
	  - :floppy_disk: `Module Name.svg` - Mandatory, is the icon in the top-right of your manual. Rare cases such as Big Bike Repair don't have such icons and can ommit this file.
	- :file_folder: `Module Name` - Put all images (SVG preferred, PNG otherwise) used in your manual in here
  - :file_folder: `js`
    - :file_folder: `Modules`
	  - :clipboard: `Module Name.js` - If you have custom JavaScript outside of the main HTML page. Recommended if you [implement Rule Seed](<https://github.com/Blananas2/ktane-Markdown/blob/main/ruleseed.md>).
  - :clipboard: `Module Name.html` - Your Manual. Please Lint it using [this website](<https://juli3.net/ktane-lint>) beforehand!
- :file_folder: `JSON`
  - :clipboard: `Module Name.json` - The JSON containing the data about your module.

Any "Module Name" must be replaced by the actual name of your module, including some special characters like hyphens: Use `3D Maze`, `% Grey`, `1 + 2 = 3`, `The Navy Button`, etc. This should be the same name you put in Unity into the `KMBombModule`.

The JSON file can contain many different values to give all of the data about your module. Use the JSON Creator on the Repository in `Tools > Create new module` to create it.

The JSON file content is given below just as an example, it is not recommended to create yours manually unless you know what you're doing:

All SVG files, including the Component, must be minified using [this website](<https://luminoscity.github.io/svgomg/>) before being posted. All of their text must also be Converted to Paths as font information isn't transferred.

```JSON
{
  "Contributors": {
	"Developer": [ "YourNameHere" ]
  },
  "DefuserDifficulty": "Easy",
  "Descriptions": [
    {
      "Description": "This is the description of your module, in English.",
      "Language": "English",
      "Tags": "one-button, screen, green-background"
    },
    {
      "Description": "Ceci est la description de ton module, en Français",
      "Language": "Français",
      "Tags": "un-bouton, écran, fond-vert"
    }
  ],
  "ExpertDifficulty": "Easy",
  "Ignore": [
    "+SolvesAtEnd",
	"+SolvesWithOthers",
	"+TimeDependant",
	"Lunchtime",
	"The Klaxon",
	"Timing is Everything",
	"-Organization"
  ],
  "Issues": "Gameplay",
  "ModuleID": "moduleName",
  "MysteryModule": "MustNotBeHiddenOrKey",
  "Name": "Module Name",
  "Origin": "Mods",
  "Published": "2026-09-14",
  "Quirks": "SolvesAtEnd, SolvesWithOthers",
  "SortKey": "MODULE NAME",
  "SourceUrl": "https://github.com/Timwi/KtaneContent",
  "Souvenir": { "Status": "Supported" },
  "SteamID": "000000000",
  "Symbol": "Mdnm",
  "TutorialVideos": [
    {
      "Language": "Čeština",
      "Url": "https://www.youtube.com/watch?v=_1IbhHJG4WI"
    },
    {
      "Language": "English",
      "Url": "https://www.youtube.com/watch?v=-V-AQ6JqlqY"
    },
    {
      "Language": "Français",
      "Url": "https://www.youtube.com/watch?v=Dy6_44lmRQg"
    }
  ],
  "Type": "Regular"
}
```

- Contributors can be either a singular name (in which case it is "Author", such as `"Author": "Your Name Here",`), or have categories within brackets, each containing an array of values (`"Contributors": { "Developer": ["Your Name Here], "Manual": ["Some Other Name"] },`).
  - The Possible Contributor Categories are: `Audio`, `Developer`, `Idea`, `Maintainer`, `Manual` or `Manual graphics`, `Modeling`, `Rule seed`, `Twitch Plays`. 
- Difficulties for `DefuserDifficulty` and `ExpertDifficulty` can be either `Trivial`, `VeryEasy`, `Easy`, `Medium`, `Hard`, `VeryHard`, `Extreme`.
  - Even if both difficulties are the same, both fields need to be present.
- Formatting for `Description` changes depending on if a translation is available or not:
  - `Description` (singular) for no translation. It is a singular string containing first the description; then a space; followed by `Tags: ` and all tags, separated by commas, in lowercase.
    - Example: `"Description": "Find out what specific holiday is marked in a calendar. Tags: left, right, month, color"`
  - `Descriptions` (plural) for if there are translations. It is an array of elements, each containing a `Description` which is translated, a `Language` and `Tags` given as a simple comma-seprarated string.
- `Ignore` contains an array of all Quirks (prefixed by `+`) or Modules (Module Names, not IDs) that should be ignored by this module. You can also prepent a module name with a `-` to say it shouldn't be ignored, even if it has quirks that are ignored. This uses the [Boss Module Manager](<https://steamcommunity.com/sharedfiles/filedetails/?id=1669095833>) to connect to the Repository and gather this data dynamically.
- `Issues` tells if the module has issues that make it unplayable or not. Its values are:
  - `None` (can be ommitted in this case)
  - `Superficial` places the yellow triangle, and is for purely cosmetic, non-gameplay issues that don't impact the solving process.
  - `Gameplay` places the red circle, and is for modules that have an actual issue that prevents reliably solving them, no matter how rare they are.
- `ModuleId` must be the same value you put in Unity as the in-game module ID. There is no set naming scheme, though usually it is the name of the module in camelCase, without spaces, sometimes prefixed by an author tag (like `GS` for GhostSalt or `xel` for Panoptes).
  - There are examples on the repository of modules that do not follow this naming scheme (Plumbing being `MazeV2`, or Emoji Math being `Emoji Math` with a space). This doesn't break anything as long as the module id matches in-game, but this is generally frowned upon.
- `MysteryModule` specifies if Mystery Module should act in a specific way towards this module. Possible Values include:
  - `NoConflict`, which is the same as not having the field
  - `MustNotBeHidden` for modules that shouldn't be hidden but can be asked to solve (requires other mods to be solved earlier such as Hogwarts)
  - `MustNotBeKey` for modules that can be hidden but shouldn't be asked to solve (requires data about other mods present such as Mafia)
  - `MustNotBeHiddenOrKey` for modules that shouldn't be hidden nor asked to solve (requires both other mods to be solved and data about other mods)
  - `RequiresAutoSolve` is for modules that require a solve order on other modules (such as Organization or Turn the Keys)
- `Quirks` contains all Quirks the module has, in a single comma-separated string (not an Array). Possible Values include:
  - `SolvesAtEnd` for modules that solve at the end of the bomb. Most "Boss" modules have this quirk.
  - `NeedsOtherSolves` for modules that require *some* other solves before solving, such as Divided Squares.
  - `SolvesBeforeSome` for modules that require to be solved before some or all other modules, such as Turn the Keys or Access Codes.
  - `SolvesWithOthers` for modules that can solve at the same time as another, often paired with other mods. Includes X and Y or Castor and Pollux.
  - `WillSolveSuddenly` for modules that can solve at unexpected times, such as Tyler Verifies or The Swan.
  - `PseudoNeedy` for modules that periodically require attention until they solve, such as The Swan or LOOK AT ME.
  - `TimeDependent` for modules that need players to pay attention to the timer closely, such as Turn The Key or Timing is Everything.
  - `NeedsImmediateAttention` for modules that activate at the start of the bomb and need attention early, such as Tax Returns or Lunchtime.
  - `InstantDeath` for modules that, even in very specific or player-created circumstances, can detonate the bomb instead of striking, such as Turn The Key or Lightspeed.
- `SortKey` is the name of the module, including spaces, in all capital.
- `Souvenir` has 3 possible values for its Status. Some of it will require an additional `"Explanation": "Explanation for this Module"` field to give more info.
  - `Supported` for modules that are currently supported and included in Souvenir.
  - `Considered` for modules that can get Souvenir Support, but do not have it yet. This is accompagnied by an Explanation field.
  - `NotACanditate` for modules that cannot get Souvenir Support. This is accompagnied by an Explanation field.
  - `Unexamined`, which is equivalent to not having this field
- `SteamID` can be omitted if the module is closed-source. One reason why is if it uses KTANE assets directly (such as many Not modules) or if it uses connection to external tools (such as Custom Keys or Tyler Verifies).
- `Symbol` is the Periodic Table symbol for the module. It usually is 1-to-4 characters, letters and numbers only, with only the first letter being capitalized. No duplicate Periodic Table Symbols are allowed on the repo (case-insensitive).
- `Type` can have the following values: `Regular`, `Needy`, `Appendix`, `Widget` or `Holdable`.

In the case of Appendices, `ModuleID` and `SteamID` have a value of `null`, but still are present.
Holdables still have Difficulties and Symbols.

### Using #repo-requests on the Discord

Place all of your necessary files into one .ZIP file, and post it in the [#repo-request](<https://discord.com/channels/160061833166716928/612414629179817985>) channel on the [KTANE Discord](<https://discord.gg/K6uQMyBcYZ>), stating the name of your module and a small description.

Be patient and respectful, even if your module takes a few days to appear on the Repo. Repo Maintainers and Contributors work voluntarily on their free time to update and maintain the repository!

### Making a GitHub Pull Request

For more information about using GitHub, refer to the [Working with GitHub](<https://github.com/Blananas2/ktane-Markdown/blob/main/using-github.md>) article.

[Full Tutorial TBA]