# MonsterTrainGameData
Data dumps for Monster Train game data, so AssetStudio no longer needs to be used. This is to help with modding Monster Train.
If you are new to modding Monster Train, here's a [tutorial](https://github.com/brandonandzeus/Trainworks2/wiki) to get you started.

## Motivation
Using AssetStudio is a pain for searching for GameData objects

1. Asset names do not align with the associated Card/Character/etc. in-game. This is especially true for Artifacts/Relics.
2. Looking up how a specific object works take time and effort. One would need to flip through multiple assets. For instance, CardUpgrades are their assets and must be looked up.
3. When viewing GameData, you see all of the fields, even if they are their default value, making it harder to decipher what fields are important.

## How to use
This repo only contains JSON files, each representing a particular GameData object. The JSON files are minimized, meaning all unimportant fields are removed. Additionally, the data is expanded from what you would see in AssetStudio, allowing you to easily see how a specific GameData object works or how a particular field is used.

### Example JSON file

`CardData/Rootseeds.json`
```
{
  "nameKey": "CardData_nameKey-1e0345136eea7880-6757db998f7279743be5e849a3f29fa4-v2",
  "cost": 1,
  "overrideDescriptionKey": "CardData_overrideDescriptionKey-3635c32966b3e70b-6757db998f7279743be5e849a3f29fa4-v2",
  "targetless": false,
  "effects": [
    {
      "effectStateName": "CardEffectAddTempCardUpgradeToUnits",
      "targetMode": 12,
      "targetTeamType": 3,
      "paramCardUpgradeData": {
        "bonusDamage": 2
      }
    },
    {
      "effectStateName": "CardEffectDrawAdditionalNextTurn",
      "targetTeamType": 3,
      "shouldTest": false,
      "paramInt": 1
    }
  ],
  "linkedClass": "ClassAwoken",
  "cardLoreTooltipKeys": [
    "CardData_data-2c87eaad8f1a6731-6757db998f7279743be5e849a3f29fa4-v2"
  ],
  "name": "Rootseeds"
}
```

### Converting to a Custom Modded Card
From the JSON and [TrainworksModdingTools](https://github.com/brandonandzeus/Trainworks2), it should be fairly intuitive to go from this JSON representation to the appropriate Builder object.
The only pieces one needs to provide are the respective IDs for the Builders.

```
new CardDataBuilder
{
  CardID = "MyRootSeeds",
  Name = "My RootSeeds",
  Description = "Apply +[effect0.upgrade.bonusdamage][attack].[halfbreak]Draw +[effect1.power] next turn.",
  Targetless = false,
  EffectBuilders = {
    new CardEffectDataBuilder
    {
      EffectStateType = typeof(CardEffectAddTempCardUpgradeToUnits), // Switch from Name to Type
      TargetMode = 12, // or TargetMode.DropTargetCharacter
      TargetTeamType = 3, // or Team.Type.Heroes | Team.Type.Monsters
      ParamCardUpgradeDataBuilder = new CardUpgradeDataBuilder
      {
        UpgradeID = "MyRootSeedsUpgrade",
        BonusDamage = 2,
      }
    },
    new CardEffectDataBuilder
    {
      EffectStateType = typeof(CardEffectDrawAdditionalNextTurn",
      TargetTeamType = 3, // or Team.Type.Heroes | Team.Type.Monsters
      ShouldTest = false,
      ParamInt = 1,
    }
  }
  // etc
}
```

## Last Note
This repo is still a work in progress, as more content must be added.
