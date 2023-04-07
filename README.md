TODO:  Finish the docs.
Finish showing usage, 
Show converting pipe output to SimpEn.csv


# Basic Overview
Only for mods that create cards without Localization Keys.

It is preferred to change the mod's code to use a localization key, but some mods create cards via generation, so this is just easier than possibly messing up the original code.

Generates hash based localization keys
Exports keys to BepInEx output log.

Keys can then be translated and saved to <mod's folder>/Localization/SimpEn.csv.
    Format is Key,English,Chinese

If SimpEn.csv is loaded and the game is in English mode, the DefaultText will be set to the English translation.

# Draft Doc
## Draft Examples:

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

Only use if there is not a LocalizationKey set.  
Add a .SetLocalizationInfo(); to the object that does not have a LocalizationKey set.

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







