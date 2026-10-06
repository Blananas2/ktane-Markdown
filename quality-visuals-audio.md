The saying "don't judge a book by its cover" is in reality not how our brains work. The [Aesthetic-Usability Effect](https://en.wikipedia.org/wiki/Aesthetic%E2%80%93usability_effect) is a known phenomena where aesthetic designs are considered more intuitive and pleasant to use than less aesthetic ones.

What this means for us is that **making your Modules pretty matters**: players will receive it more positively and interacting with it will be more fun overall.

## Visuals

Take some time to craft an identity for your module to make it more unique amongst the 2500+ modules that exist.

There is no need to go too far, but a module with a flat colour background and a plain square for a button can be seen as lazy and uninspired.\
Use shaders such as `KT/Mobile/DiffuseTint` to tint the background casing with a colour more fitting of your manual. Spend some time modelling a simple button shape. Use colours that are nice on the eye and that work well together.

If you know what they are used for, [here](https://discord.com/channels/160061833166716928/201105291830493193/847187422604558427) are the UV maps for the models of the default module casing and the needy casing.

### Make interactions feel lively

There are many ways to make interacting with your module feel less stiff. This is called "juice" and helps your module feel more real and fun to interact with.

Take the base game for example: The Button has a smooth round shape, it has a plastic lid that opens up when you select it, the button moves smoothly when pushed and shakes the bomb, the coloured light on the right doesn't stay perfectly still but has a variance in its brightness, etc.

One way to make movements and animations more juicy is to use Easing Functions: Those are simple mathematical equations that add some more realistic and juicy movements. [Easings.net](https://easings.net/) has all basic Easing functions visualized with code examples; and the community modkit contains a class called `Easing` to quickly make use of them in your code.

### Tweening Library

A Tweening Library is a code library that helps you to all that fancy animation with robust time-keeping, interpolation, and provide easing functions for you. They come up bundled with methods to animate pretty much all basic properties of GameObjects in Unity: position, rotation, scale, etc.

A recommended Tweening Library is [LeanTween](https://assetstore.unity.com/packages/tools/animation/leantween-3595), a free tweening library for Unity, which is used in base KTANE's code already.

### The Unity Animator

The Unity Animator is a built-in system that allows you to do more complex animation sequences in a robust way.

For example, in the base KTANE game, The Button's plastic casing opening when you select the module is done using the Unity Animator.

Here is a link to the Documentation for using Unity's Animator in versions 2017.4.X: https://docs.unity3d.com/2017.4/Documentation/Manual/AnimationSection.html

## Audio

Pay attention to your audio: A module without sound when you press a button or when it solves doesn't feel good to interact with.

Attach to your module's prefab a `KMAudio` component, and using a reference to it in your code, you can play a sound.\
Use your own sounds by using `PlaySoundAtTransform(soundName, Transform)` or use one of the game's base sounds using `PlayGameSoundAtTransform(KMSoundOverride.SoundEffect, Transform)`.

> [!NOTE]
> Be careful when using the game's base sounds. Using button presses and wire snipping often is good, but more elaborate sounds such as AlarmClockBeem or Strike might be changed by the player using a custom sound pack.

Consider using custom sounds for your module, and adding unique sounds for everything: Pressing buttons, Solving the module, Striking, Changing screens, Moving around, Inputting a number, etc.\
The more unique the sound identity of your module, the more unique and interesting your module will be.

You can use websites such as [Freesound.org](https://freesound.org/) to get some base sounds, and use softwares such as [Audacity](https://www.audacityteam.org/download/) to edit them if needed.\
**Be careful about licenses for the sounds.** 

### Tips for using Audio

- Mind your sound balance

No need to go too far: just adjust the sound volume in Audacity so they sound not too loud but not too low. Then, test in-game your sounds, surrounded by simple modules such as The Button, Wires or Venting Gas to make sure your audio levels are correct.

- Select your sounds carefully

If your sound crackles, pops, or sounds cheap; it will negatively affect how your module is viewed.

You should also make sure to select audio that sounds appropriate: if your module is mechanical and made out of metal, don't use sounds of plastic buttons being pressed when interacting with it!

## Further Reading

- The legendary "Juice It or Lose It" conference by Martin Jonasson & Petry Purho (subtitles included): https://www.youtube.com/watch?v=Fy0aCDmgnxg