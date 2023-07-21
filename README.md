# MythicGME-2e-Obsidian
A starter vault to use Mythic GME 2e by Tana Pigeon in Obsidian.

At the moment only the Meaning Elements Tables are included, it will probably grow with time.

## Change log

### v5.0
- Added a shortcut file that implements Event Crafter from Word Mill Games using Inline Scripts.
### v4.0
- merged PR for alternate table view of Meaning Tables.
### v3.1
- Fixed Descriptions second roller rolling on first table instead of second.
### v3.0
- Removed number columns to have cleaner output.
- Removed dice rollers in single files.
### v2.0
- Added a note hosting all rollers for ease of use.
### v1.0
- Uploaded Meaning Tables.

## Setting up
Just download this repository as a zip file and unzip it in your chosen folder.
Create an Obsidian Vault from that folder and have fun!

### Using the Meaning Tables
To use the automatic table roller, the plugin [Dice Roller](https://github.com/valentine195/obsidian-dice-roller) is needed.
Once installed and activated, just keep the Obsidian pane in Reading mode and click on the result text to reroll.

### Using the Event Crafter shortcut
To use the Event Crafter shortcut file:
1. Install the plugin [Inline Scripts](https://github.com/jon-heard/obsidian-inline-scripts) and activate it.
2. In its settings, import the default library. The shortcut file state.sfile is needed to work and saving state of last session.
3. Click on "Add shortcut-file", which will add a new empty slot at the end of the list.
   ![image](https://github.com/Zhrack/MythicGME-2e-Obsidian/assets/5031873/034e8373-4a82-4eef-9466-58b32fadd7f8)

4. Write "support/customScripts/eventcrafter.sfile" (or the path you chose starting from the root of the Vault) in it and toggle the shortcut active
   ![image](https://github.com/Zhrack/MythicGME-2e-Obsidian/assets/5031873/79f871c8-bedf-4471-984c-a25bc452f30c)

5. Now it should work. To test it, write ";;help eventcrafter::" (without the ""). As soon as you finish writing the last :, it should substitute the text with ![image](https://github.com/Zhrack/MythicGME-2e-Obsidian/assets/5031873/e4ab3a63-f71e-4e93-baae-3b391c547b55)

For tutorials on how to use Inline Scripts, go to it's own page https://github.com/jon-heard/obsidian-inline-scripts!

Here is a list of available commands. While typing each one, a description is also present.
![image](https://github.com/Zhrack/MythicGME-2e-Obsidian/assets/5031873/5bb6b2be-b28a-4b49-9326-2b783b7c9186)


## Credits

Mythic Game Master Emulator Second Edition, by Tana Pigeon, published by Word Mill Games, and licensed for our use under the [Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)](https://creativecommons.org/licenses/by-nc/4.0/) license. Find out more at [www.wordmillgames.com](http://www.wordmillgames.com/).
