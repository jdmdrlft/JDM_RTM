# JDM - RTM

Outfit, name and vehicle editor, fast run unlock and custom K/D for **GTA V Online 1.57 on PS4**, run from your PC.

Made by **jdm_drlft**. Version **1.5**.

Based on **[Arise - RTM](https://github.com/Amethxst/Arise-RTM-GTAV-PS4)** by **yAmethxst** (v2.0.0, [release](https://github.com/Amethxst/Arise-RTM-GTAV-PS4/releases/tag/v2.0.0)), used under its MIT license: see `LICENSE` and `CREDITS.txt`. Full thanks to him for the tool this one grows from: [GitHub](https://github.com/Amethxst/Arise-RTM-GTAV-PS4) · [X](https://x.com/yAmethxst) · [Discord](https://amethxst.dev/discord) · [amethxst.dev](https://amethxst.dev).

It includes the **Names** tab, the **Vehicle** tab, the **Garage Editor** tab and ps4debug-NG v1.3.4 (it adds firmware 14.02). What is different from Arise - RTM is listed in [CHANGELOG.md](CHANGELOG.md), and there is a step by step guide in [HOW_TO_USE.md](HOW_TO_USE.md).


## Features

- **Outfit editor**: rename your saved outfits (31-byte limit), edit every component, and import an outfit from text or from a `.txt` file on your PC. The format is in `outfit_example.txt` and is also shown inside the tool.
- **Create your name**: the "Create your name here" button opens https://amethxst.dev/gta-name. Build the name there and paste it in the tool.
- **Teleport**: a button that moves your character straight to the clothing shop.
- **Fast run**: unlocks the permanent fast run on the character you choose.
- **Custom K/D**: set the K/D ratio, or the kills and deaths on their own.
- **Names tab**: reads and writes the names of your organization, office, MC club, clubhouse, yacht, nightclub, acid lab, acid lab product and mansion pets. It has colour, bold, italic, size and symbol buttons with a live preview that follows the game's rules, and shows how many bytes the name uses (up to 41). The new name is saved in your game; **only you see it**: other players keep seeing the name approved by Rockstar. It connects by itself when you press **2. CONNECT GTAV**, always uses the **Stats character** (Character 1 or 2) chosen in the toolbar at the top (it has no character buttons of its own: until you choose one it asks you to), and writes its messages in this window's log (lines starting with `Names:`).
- **Vehicle tab**: edits the look of the car you drive (or the last car you drove) with galleries and one **Apply changes** button. Paint (161 colours grouped like the GTA V Save Editor, 62 chameleon paints, custom RGB), neon, xenon, tire smoke (with Patriotic), window tint, wheels of 10 types with pictures, 13 plate styles with pictures, plate text of any character (with a **GHOST PLATE** button), horns and the other mods. **Save to garage** makes the game keep the look and writes the plate, colours and wheels straight into the slot of the car in your garage. It can also **add any game vehicle to a garage** of your character (category Vehicle, with an undo). See [HOW_TO_USE.md](HOW_TO_USE.md) and the section below.
- **Garage Editor tab** (new in 1.5, by jdm_drlft): it reads your garages by itself when you open it and lists them (apartments, offices, clubhouse, hangar...) with the cars in each one. Choose a garage, then a car, and edit it with the same kind of menu as Vehicle **without taking it out**. Only the options whose place in the saved car is known are there (plate, colours, interior, dashboard, rims, tint, neon colour, headlights, turbo, tyre smoke with its colour, engine, brakes, transmission, suspension, armor, horn, livery and the other mods of each model); a copy of the slot is kept and **Undo garage** puts it back.
- **Debug tab** (advanced): write or read any stat by name, set a packed value by index, or run a list of writes from a `.txt` file on your PC. Nothing is bundled: the tool writes only what you type or load. See below.

## Requirements

- PS4 on **firmware 14.00/14.02**, jailbroken with GoldHEN.
- GTA V **1.57**, EU (`CUSA00411`) or US (`CUSA00419`).
- A Windows PC on the same network as the console, with .NET Framework 4.8.1.

Tested on firmware 14.02 with GTA V 1.57 (EU).

Firmware **14.02**: `ps4debug.bin` v1.3.4 adds support for it. A ps4debug that is already running is not replaced, so restart the console and jailbreak it again after updating `ps4debug.bin`.

## How to use

1. Make sure the PS4 is on firmware **14.02**.
2. On the console, open https://webkitty.arabpixel.net/ and jailbreak it.
3. In GoldHEN, open **Server Settings** and enable **FTP** and **Payload Loader**.
4. Start GTA V.
5. Download `JDM-RTM-GTAV-PS4.zip` from the [Releases](../../releases) page and unzip it. Open `JDM - RTM.exe`, type the IP address of your PS4, then press **1. CONNECT PS4** and **2. CONNECT GTAV**.
6. Go online, then use the tool. If the outfit list is empty, press **Refresh**.
7. For names, choose **Character 1 or 2** in the toolbar, then open the **Names** tab. Pick a name in the list, type the new name (the buttons insert colours, styles, sizes and symbols) and press **WRITE & SAVE**. Wait for the bar to finish and for the message "is set and saved" before changing session.
8. For vehicles, open the **Vehicle** tab with a car of yours out (you can be on foot next to it), choose what you want in the categories and press **Apply changes**. The whole guide is in [HOW_TO_USE.md](HOW_TO_USE.md).

### Notes

- The outfit editor works on the character currently loaded in the game. Press **Refresh** after going online or after changing character.
- Fast run and K/D need **Character 1** or **Character 2** chosen in the toolbar: the buttons stay disabled until then. Change session to see the new values.
- Debug tab: values are written straight to the game, with no check. A wrong value or list can damage your character or your account, so use lists from sources you trust. "Check a list" reads a file and tells you what is in it without writing anything. A long list shows its progress and can be stopped with **Cancel**.
- `ps4debug.bin` is sent to the console automatically when it is not running yet.
- To look at the whole window without a console, start it with `--preview`. Nothing can be written in that mode.
- The FTP port is used only to read the game version from the console, so the tool can check that it is 1.57.
- Use it at your own risk.

## Vehicle tab

Categories on the left, a gallery on the right, and the result of every action in the box below it. **Read vehicle** loads the car (the tab does it by itself when you open it), you click what you want (nothing is written yet) and **Apply changes** writes only what you changed, after one confirmation that lists it. **Revert** goes back to what the car has.

- **Natives are checked before every action**: the tool finds the game functions by name in a table for GTA V 1.57 and refuses to run if the table does not agree with the game.
- **Save to garage** (on by default, personal cars only) makes the game keep the look when the car is saved, writes the options whose place in the garage slot is known and asks the game to save. The options that are not written there are applied to the car and the game keeps them when it saves the car.
- A **diamond** on a colour means it is not obtainable in that category (sheet "Color Shenanigans"). It does not block anything.
- The **Garage Slot** category has advanced tools (**Dump slot**, **Snapshot** and **Compare**) to find where an option is saved in the garage. They change nothing.
- **Add a vehicle to your garage** (category **Vehicle**, no car needed): choose a model of the game and a garage, and the program creates a new saved car in a free slot of your character and puts it in the first free space of that garage (the cells of the slot, the model stat and the garage map). It refuses a model the game does not have, reads every part back and **Undo last add** removes the car. Choose a garage you own: the list has every garage of the game.
- What you change is seen by the other players of the session, and what is saved in the garage is saved in your account: **use a cheap car for your first tests.** Chameleon paints belong to the Expanded and Enhanced version of the game and may not exist on PS4 1.57.

## Garage Editor tab

Edits the cars saved in your garages **without taking them out**, with the same kind of menu as the Vehicle tab.

1. Open the tab: your garages and cars are read by themselves the first time (**Refresh** reads them again). The first list shows your garages with the cars in each one, the second list shows the cars of the chosen garage.
2. Choose a car. The menu has the options that can be edited in a saved car: plate style and text, primary, secondary, pearlescent, rim, interior and dashboard colours, neon colour, window tint, front rims, headlights, turbo, tire smoke, and the mods of the model (engine, brakes, transmission, suspension, armor, horn, livery and the cosmetic ones). How many options a model has comes from its mod kit; the names of the liveries are learned the first time you read a car of that model with **Read vehicle** in the Vehicle tab.
3. **Apply changes** writes straight into the saved car, after one confirmation that lists the changes. A copy of the slot is kept in memory and in the `garage_backups` folder, and **Undo garage** puts it back. Request the car or change session to see it.
4. Do not edit a car that is out in the world: when you store it, the game saves its look over your edit.

The neon sides, the secondary RGB and the rear rims are not editable in a saved car yet. What is saved in the garage is saved in your account: **try it on a car you do not mind losing.**

## Debug tab

For advanced users. Values are written straight to the game, with no check, so a wrong value can damage your character or your account.

- **Character**: your two characters are called `MP0_` (Character 1) and `MP1_` (Character 2) in the game. Choose one at the top of the tab or in the toolbar: it is the same choice.
- **Set a stat / Read a stat**: type the name without the prefix, for example `CHAR_ABILITY_1_UNLCK`. *Character stat* adds `MP0_` or `MP1_` for you, *Account stat* adds `MPPLY_`, *Exact name* uses what you typed. The line "Will use" shows the full name. Pick the type that matches the stat: **int** is a whole number, **bool** is true / false, **float** is a decimal number.
- **Packed value**: a flag (or a small number) stored by its number, the index, instead of by a name. It belongs to the chosen character.
- **Run a list from a file**: a `.txt` file from your PC, one write per line (`stats_example.txt` shows it). **Check a list** reads it and tells you what is inside without writing anything. **Run a list** asks for confirmation, shows its progress and can be stopped with **Cancel**.

```
int    MP_CHAR_ABILITY_1_UNLCK   -1       a stat, 32-bit integer
bool   MP_SOME_STAT              true     true / false (or 1 / 0)
float  MPPLY_SOME_STAT           6.66     a decimal number
pbool  1234                      true     a packed flag by index
pint   1234                      5        a packed integer by index
```

`MP_` at the start of a name means the character you chose. Any other name is used as typed. `#` starts a comment.

## Credits

- **jdm_drlft**: this tool, including the **Names** tab (`Names/NamesRtm.cs`, MIT, see `LICENSE-NamesRTM`) and the **Vehicle** tab (`Ui/VehiclePanel.cs` and the `Vehicle` folder).
- **yAmethxst**: the original **Arise - RTM** this tool is based on (v2.0.0, MIT, copyright notice in `LICENSE`): https://github.com/Amethxst/Arise-RTM-GTAV-PS4 · [X](https://x.com/yAmethxst) · [Discord](https://amethxst.dev/discord)
- **ps4debug-NG** by OpenSourcerer (OSR), maintained by Pharaoh2k: https://github.com/Pharaoh2k/ps4debug-NG. `ps4debug.bin` is v1.3.4 of this payload (see `ps4debug-NG-SOURCE.txt`), licensed GPL-3.0. Its own thanks go to golden, Ctn, SiSTRo, DeathRGH and Pharaoh2k.
- **GoldHEN** by SiSTRo: https://ko-fi.com/sistro/shop
- **afterglow** by well0nez/m0zz: https://well0nez.github.io/afterglow-web/
- `libdebug`: the C# client library used to talk to ps4debug, by golden (jogolden): https://github.com/jogolden/ps4debug
- **XB36Hazard**: author of the **GTA V Save Editor**. The pictures of the chameleon paints, the wheels and the number plates of the Vehicle tab are cropped from screenshots of the galleries of his editor, and the names and groups of the colours and wheels come from it. They are his; they are only used as pictures of the options.
- **Velora** (menu for PS4): the table of native names and addresses for GTA V 1.57 (`lua/natives.lua`) the Vehicle tab is generated from.
- **Seren** (Serendipity GTAO Mod Menu): reading its code showed how the personal vehicles are found in the game's data. No code of it is included.
- **DurtyFree** ([gta-v-data-dumps](https://github.com/DurtyFree/gta-v-data-dumps)): names and values of the 161 base colours and the names of the car models.
- **Official Nugzz Crew**: the "Color Shenanigans" sheet, source of the diamonds (HighDutchMan89, 420Corruption and Fam Bam are thanked in it).
- **GTA Wiki** (chameleon paint numbers) and the **RAGE Multiplayer Wiki** (horn indexes).

Full details in [CREDITS.txt](CREDITS.txt).

## License

The code of this tool is under the [MIT License](LICENSE): use it and change it freely, but keep the copyright notice ("Copyright (c) 2026 yAmethxst") if you reuse it. The third-party material listed above keeps its own terms.

Grand Theft Auto, GTA, Rockstar Games and PlayStation are trademarks of their respective owners. This tool is not affiliated with or endorsed by Rockstar Games, Take-Two Interactive or Sony.
