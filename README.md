# SMZ3 Cas' Configs

This repository is for the various YAML files used by the [SMZ3 Cas' Randomizer](https://github.com/Vivelin/SMZ3Randomizer) for rom generation, tracker responses, and the tracker UI. These configs are both bundled with the application install and, if enabled, will be downloaded automatically on launch of the Randomizer.

## Profiles

Within the Profiles folder are the various profiles which can be enabled or disabled in the tracker options. The default profile will always be enabled, and the template profile will always be ignored. While changes to the built in profiles (BCU and Sassy) will be overridden with updates, any user created profiles will be left alone.

## Config merging

All profiles in the **Enabled** section will be merged when you open Tracker. This combines all responses together into a single list that she will pull from. Each config type is merged based on an identifier, which is identified in the comments at the top of each config file.

When loading a config file, it will find if any previous configs already had an entry with that identifier. If it does, it will combine the responses for the two configs. If it doesn't, it'll add it as a new entry. Using this you can add your own items, bosses, etc. which can be tracked.

## Creating a new profile

To create a new profile, navigate to the directory by navigating to **%localappdata%\SMZ3CasRandomizer\Configs** or by clicking on the _Open Profiles Folder_ button in the options window. Once there you create a new folder and name it with the profile name you want to use.

Next you can then either copy the yml files from the Templates folder or create an empty yml file matching the following names:

- **bosses.yml** - Additional boss information
- **dungeons.yml** - Additional dungeons information
- **game.yml** - In game text lines that are inserted into the rom
- **hint_tiles.yml** - Hint tile names and tracker responses to viewing hint tiles
- **items.yml** - Additional items information
- **locations.yml** - Additional locations information
- **msu.yml** - Responses and configurations for MSU details
- **regions.yml** - Additional region information
- **requests.yml** - Adds addition prompt and responses for tracker
- **responses.yml** - Responses for specific actions performed by the user
- **rewards.yml** - Additional reward information
- **rooms.yml** - Additional room information
- **ui.yml** - The various tracker UIs which are displayed

**NOTE:** Contained inside each of the template yaml files is additional details on what can be modified for Tracker to say or understand. It is suggested to copy the templates and edit them.

### Moods

Depending on Tracker's mood, additional mood-specific files may be loaded. To do this, simply add a new file named `<name>.<mood>.yml`. For example, if you wanted to add some responses for when Tracker is happy, simply add a `responses.Happy.yml` file. 

When the randomizer is started, Tracker's mood is randomly selected from the moods that are configured. For example, if she starts happy, any files that end in `.Happy.yml` will be loaded in addition to the normal files. 

## Editing profile configs

Profile configs are written in a language called YAML. You can edit these in any text editor, but to make use of the schema validation, it is recommended to use the [Visual Studio Code](https://code.visualstudio.com/) with the [YAML extension from Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml). This combination will make use of the schemas to make sure that the format and data are structured correctly.

More detailed specifications about YAML can be found elsewhere online, but here some very basic details:

- Values are specified via the format name: value
- Series of items are prefixed with -
- Lines that begin with # are ignored lines and treated like comments
- Whitespacing is important

## Possibilities

Most of the settings are a list of possibilities, which is a series of messages that tracker can say and the likelyhood in which they will be said. Possibilities will show up like this:

```
Field:
- Text: One message that can be stated
- Text: Another message that can be stated
- Text: This message is less likely to be said
  Weight: 0.1
- Text: This message is also less likely to be said
  Weight: 0.1
```

The text line is required and is what will be said by tracker. The weight adjusts the likelyhood of the message being said in a range from 0 to 1. This field is optional, and if no weight is specified it defaults to a value of 1. The higher the value the more likely it is to be said. The closer a value is to 0, the less likely it is to be said.

## Questions

### Can I use some responses from one config, but not all?

Unfortunately you can't pick and choose what comes from a config. You can either copy the profile and remove what you don't want. Or, if you have your own profile, you can increase the weight of your responses to make them way more likely than the original responses.

### Can I create my own items to track?

You can! You can create brand new items as well as bosses which can then be tracked. Simply add a new record such as the following:

```
- Item: Custom Item
  Name:
  - Text: Item name
  - Text: Rare name of the item
    Weight: 0.1
  Multiple: true
  WhenTracked:
    "1":
    - Text: Tracker will say this when you track your first and second ones
    "3":
    - Text: tracker will say this when you track your third one
```

```
- Boss: Custom Boss
  Name:
  - Text: Boss name
  - Text: Rare name of the boss
    Weight: 0.1
  WhenTracked:
  - Text: This is what will be said when the player says "Hey tracker, track boss name"
  WhenDefeated:
  - Text: This is what will be said when the player says "Hey tracker, I defeated boss name"
```

You can then create your own UI Layout in a ui.yml file with them. In the Identifiers, you will need to make sure it matches exactly with what you have next to Item or Boss line, like in the following:

```
  - Type: Items
    Row: 1
    Column: 1
    Identifiers:
    - Custom Item
```

```
  - Type: SMBoss
    Row: 1
    Column: 1
    Identifiers:
    - Custom Boss
```

For the images, create a Sprites/Items folder underneath your custom tracker profile and add the sprite(s) in .png format there using the same text you used next to Item or Boss, but in all lower case. For example, "custom item.png" and "custom boss.png".

In the case of items where you want to be able to track it multiple times, you can include additional images with (1), (2), etc. at the end to indicate different images that will be displayed when you get more of that item. For example "custom item (1).png". When maxed out, it'll use the base image without any numbers.

### How do I have my own content images?

To have your own content images, simply create a tracker profile folder under whatever name you want and add a Sprites/Items folder underneath that profile folder. Add the images you want with the naming convention of content.png, content (1).png, content (2).png, etc.

Once you have added all of that, open the settings window and make sure that your profile is enabled and is at the bottom of the list.

### How can I share my configs with others?

To share any configs you create, simply bundle the folder you created under **%localappdata%\SMZ3CasRandomizer\Configs** into a zip file and share it. In the future, there are plans to allow people to create forks of this repository to upload their own profiles and allow people to add extra urls to download configs from.