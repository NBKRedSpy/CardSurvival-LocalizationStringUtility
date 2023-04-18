
# **TODO:**
* Change code to use the Game's internal localization dictionary.
	* Add HarmonyPatch to include the loader.
* Finish the docs.
* Add notes about mods that use both a .DLL and ModLoader.  ModLoader can handle applying the translation file.
* Finish showing usage, 
* Show converting pipe output to SimpEn.csv

# Quick Notes

This utility is used to allow DLL based mods to support loading translations from a Localization/SimpEn.csv file.  

## Example
```csharp
val2.CardDescription.DefaultText = cardDescription;
//New line
val2.CardDescription.SetLocalizationInfo();
```

## Description
The new line calls an extension called SetLocalizationInfo().  
It will generate a SHA1 key based off of the LocalizedString.DefaultText, and set the LocalizedString.LocalizationKey to the new key.  

If there is a key that matches the generated key in the a ```[mod's directory]/Localization/SimpEn.csv``` file, then the target's LocalizedString.DefaultText will be set to that key's translation.

If the mod's config file has the LogCardInfo value set to true, the text and the new key will be output to the BepInEx console.  This output can then be used to create the key/translation pair for the ./Localization/SimpEn.csv file.  

See below on how to include and use the utility class.

# Mod Loader Compatibility
The ./Localization/SimpEn.csv is the same format, name, and location as a ModLoader based mod's translation file.  Translations created from this utility can be added to an existing SimpEn.csv without affecting the ModLoader mod in the same folder.


# **Work in Progress Documentation.**

# Basic Overview
Only for mods that create cards without Localization Keys.  

**todo:** Change this.  By default the game doesn't support loading DefaultText after the startup so only mods that also happen to have a ModLoader mod in the same folder would fix up translation text.

Need the utility to be able to force using the existing Localization key.

It is preferred to change the mod's code to use a localization key, but some mods create cards via generation, so this is just easier than possibly messing up the original code.

Generates hash based localization keys  
Exports keys to BepInEx output log.

Keys can then be translated and saved to <mod's folder>/Localization/SimpEn.csv.
The format is Key,English,Chinese

If SimpEn.csv is loaded and the game is in English mode, the DefaultText will be set to the English translation.

# Draft Doc
## Draft Examples:

Creating the Mod's config file to include the LogCardInfo setting to export the key/text combination.

Initing in BaseUnityPlugin.Awake():
```csharp
		LocalizationStringUtility.Init(
			Config.Bind<bool>("Debug", "LogCardInfo", false, "If true, will output the localization keys for the cards")
				.Value,
			Info.Location,
			Logger
		);
```

## Using

Fix this text.  Currently the utility always generates a key and overwrites the existing LocalizationKey.
~~Only use if there is not a LocalizationKey set.~~  
~~Add a .SetLocalizationInfo(); to the object that does not have a LocalizationKey set.~~

```csharp
		val2.CardDescription.DefaultText = cardDescription;
        //New line
		val2.CardDescription.SetLocalizationInfo();
```

Set LogCareInfo to true in the mod's .config file to dump the keys and text.

Take the generated keys from the output:

```
[Info   : MoreFruit] T-38SfL2F+5i4tDCCoAzI5dUCgYPc=|番茄榨成的果汁，可以喝。
[Info   : MoreFruit] T-P/x/SKLT7tC5+j862H4w4IIQ7as=|番茄汁
```

Remove the mod info and convert to 3 column CSV.
Put the English translation in the second column.
The 3rd column can be kept, it won't be used.

Note - output uses Pipes because Google Sheets will interpret certain unicode characters as commas and split incorrectly.
```
T-38SfL2F+5i4tDCCoAzI5dUCgYPc=,,番茄榨成的果汁，可以喝。
T-P/x/SKLT7tC5+j862H4w4IIQ7as=,,番茄汁
```

Take the CSV output and create or merge with the mod's ./Localization/SimpEn.csv

## SimpEn.csv Automatic Usage
Next time the mod is run, it will detect the ./Localization/SimpEn.csv and use the English entry for that key.
The generated key is a hash so it will always be the same for the original text.


---- Older doc start


# CardSurvival-LocalizationStringUtility

A drop in utility for Card Survival mods.

Used to extract the DefaultText of cards and create a hash based Localization key.
Only needed if the created card has no localization key.



# Installation
Copy the LocalizationStringUtility.cs to the target mod and reference it.  The class is self contained.

In the BepInEx startup, init the class:

```csharp
		LocalizationStringUtility.Init(
			Config.Bind<bool>("Debug", "LogCardInfo", false, "If true, will output the localization keys for the cards")
				.Value,
			Info.Location,
			Logger
		);
```


# LogCardInfo Output Example
```
[Info   : MoreFruit] T-wvNNtKOOkKjshMJ2gomQzeEWJn8=|Strange juice
[Info   : MoreFruit] T-fslA/ySW7/koBCmtJDYNBDD88Mc=|The fruit juice squeezed by the dragon fruit can be drank.
[Info   : MoreFruit] T-BPVgcBVdtfkF4H9bwlHbGJRH9r0=|Dragon fruit juice
[Info   : MoreFruit] T-SISBWQqMVqJYgSZqC72fwCgXMmQ=|The juice squeezed by lychee can drink.
```







