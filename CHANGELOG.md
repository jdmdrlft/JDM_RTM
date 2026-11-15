# Changelog

## 1.5 - JDM - RTM

### Added
- **Garage Editor tab** (made by jdm_drlft): it edits the cars saved in your garages **without taking them out**. It reads your garages and cars by itself the first time you open it (**Refresh** reads them again) and chooses the first garage. The first list shows your garages (apartments, offices, clubhouse, hangar, arcade, auto shop, agency, mansions...) with the cars in each one; the second list shows the cars of the garage (model name, plate and slot). The garage of each car comes from the garage map of the game, read as data, and the table of garages of the Seren plugin. Choose a car and the menu of the Vehicle tab appears, with the options that can be edited in a saved car: plate style and text, primary, secondary, pearlescent, rim, interior and dashboard colours, custom primary RGB, neon colour, window tint, front wheel, headlights, turbo, tyre smoke with its colour, and the mods of the model: engine, brakes, transmission, suspension, armor, horn, **livery** and the cosmetic ones (spoilers, bumpers, skirts, exhaust, grille, hood, fenders, roof, trims...). The saved car is read and written in the game's memory, so no game function is run and the car does not need to exist in the world.
  - How many options a model has comes from its mod kit (`vehicleModKits.json` of DurtyFree, built in) or from what the program learned when you read that model in the Vehicle tab (`mod_counts.ini`). The **names of the liveries** are learned from the game the first time you read a car of that model with **Read vehicle** (`livery_names.ini`); until then they show as Livery 1, Livery 2...
  - **Apply changes** asks once, names the car and lists the changes, keeps a copy of the slot in memory and in the `garage_backups` folder, writes, reads the result and asks the game to save (**Save after write**). **Undo garage** puts the slot back as it was before the last garage edit.
- **Add a vehicle to your garage** (Vehicle tab, category **Vehicle**): choose any model of the game (type to search) and a garage, and the program creates a new saved car in a free slot of your character and puts it in the first free space of that garage. It does the same steps as the Seren plugin: the cells of the new slot, the model stat of the slot (`MPx_MPSV_MODEL_n`) and the entry of the garage map (the global plus its three packed stats). It refuses a model the game does not have, checks that the character of the toolbar is the one you are playing, reads every part back and takes everything back if one part is not kept. **Undo last add** removes the car. Leave and enter the garage again, or change session, to see it.
- **Read vehicle** also reads the number of options of every mod type and the names of the liveries of the car, and keeps them for the Garage Editor.
- **Dump slot** (Garage Slot category) compares the real values of the car with the cells of its slot: the known cells, the mods one by one (the value of the car next to the cell 10 + mod type), the turbo, the tyre smoke and the xenon, and which cells hold the other values. It changes nothing.
- Files the program creates next to it: `mod_counts.ini`, `livery_names.ini` and the `garage_backups` folder.

### Changed
- Tabs: Main, Names, **Vehicle**, **Garage Editor**, Debug. The window title says v1.5.
- **Save to garage** writes more cells of the slot of the car: interior and dashboard colours, headlights (xenon and colour), turbo, tyre smoke with its colour, and engine, brakes, transmission, suspension, armor and horn, besides the ones of 1.4. Taking off the custom primary RGB also clears its switch in the slot (bit 9 of the switches cell), because with the switch on the game ignores the primary colour number. A cell is written when it holds a plain number (it no longer has to hold what the car had when you read it), and every write is read back.
- The program is bigger (about 2.9 MB): the names of the car models, the mod kits of every model and the tables of the garages are inside `JDM - RTM.exe`.
- `README.md`, `HOW_TO_USE.md` and `CREDITS.txt` have the new material and the credits for the data used (DurtyFree, Seren).

### Removed
- The **Auto test** of the Garage Slot category: it set every mod of the car to its maximum at once and could crash the game. **Dump slot** and **Snapshot** + **Compare** are still there.

### Things to know
- What is saved in the garage is saved in your account, and the vehicles you add are saved there too. A car that cannot normally be owned, or one that does not suit the garage, can break the garage or make your online account be flagged. Try everything on a character and cars you can afford to lose.
- The list of garages has every garage of the game: the program does not know which ones you own. Choose one you own when you add a vehicle.
- Found with Dump slot and Compare by jdm_drlft on firmware 14.02, GTA V 1.57 (EU): the cells of the mods (10 + type, index + 1), the xenon (colour + 2), the interior (cell 97), the dashboard (cell 99) and the tyre smoke colour (cells 62-64). **Not tried on a console yet**: adding a vehicle, and the cosmetic mods and the livery of a saved car (they use the same rule as the engine and the other mods; Dump slot checks it).
- Not editable in a saved car yet: the neon sides, the secondary RGB, the rear rims and the older kind of livery that some vehicles use.

### Files of the Arise code that were touched
`MainForm.cs` (the Garage Editor tab and the title), the project file (version), `README.md`, `HOW_TO_USE.md`, `CREDITS.txt` and this file. Changed: `Ui/VehiclePanel.cs`. New files: `Vehicle/VehicleNames.cs`, `Vehicle/GarageTables.cs` (with the packed stats of the garage map), `Vehicle/ModKitData.cs`, `Vehicle/ModCountStore.cs` and `Vehicle/LiveryNameStore.cs`.

## 1.4 - JDM - RTM

### Added
- **Vehicle tab** (made by jdm_drlft: `Ui/VehiclePanel.cs` and the `Vehicle/` folder). It edits the look of the car you drive, or of the last car you drove when you are on foot. Categories on the left, a gallery of options on the right, a **Search** box, **Read vehicle**, **Revert** and one **Apply changes** button: you choose in as many categories as you want, and a single confirmation lists everything that will change. Only what you changed is written. The result of every action appears in the box below the gallery, so the gallery never changes under you.
  - **Paint**: Respray Primary, Secondary, Pearlescent, Interior Color, Rim Color and Dashboard Color. The 161 base colours are grouped and named like the GTA V Save Editor (Chrome, Hidden, Matte, Metallic, Metals, Other, Rim, Util, Worn). The 62 **chameleon paints** (161-222) are also in Primary, Secondary, Pearlescent, Interior and Dashboard, with their pictures. Primary and Secondary have a custom RGB strip. A **diamond** on a tile means that colour is not obtainable in that category, following the "Color Shenanigans" sheet (the Metallic group is not built in: see `colour_availability.csv` in HOW_TO_USE.md).
  - **Lights and glass**: Neon Layout (left, right, front, back), Neon Color, Headlights (xenon and colour), Tire Smoke (colours, a custom RGB strip and **Patriotic**), Windows (tint).
  - **Wheels**: Front Rims and Rear Rims show the wheels of 10 wheel types with their names and pictures (Tuner, Track, SUV, Street, Sport, Race, Offroad, Muscle, Lowrider, High End). Choosing one sets the wheel type and the wheel. Benny's and bike wheels are not in the gallery.
  - **Plate**: 13 plate styles with their pictures. **Plate Text** accepts up to 8 characters of any kind, has a **GHOST PLATE** button (8 spaces) and a field for raw bytes in hex.
  - **Mods**: Armor, Brakes, Engine, Horn, Suspension, Transmission, Turbo, Livery and every other mod type the car has. The horns have their names for the first 35 (indexes 0-34); the rest are shown as "Horn N" because their order is not confirmed.
  - The natives are found **by name** in a table of addresses for GTA V 1.57 (`Vehicle/NativeAddresses.cs`) and are **checked automatically before every action**: the table must agree with the 7 stat handlers of the profile and every native must start like a handler. If not, nothing is run. There is no check button.
  - The tab reads the car by itself when it is opened. If you press **Apply changes** in another car, nothing is changed: the new car is read and you choose again.
- **Save to garage** (check box, on by default, personal cars only): sets the flag the game puts on a car modded in a mod shop (`Veh_Modded_By_Player`, with the value of `Player_Vehicle`), writes straight into the slot of the car in your garage the options whose cell is known (plate style and text, primary, secondary, pearlescent, rim colour, front wheel, wheel type, window tint, custom primary RGB, neon RGB) and asks the game to save (it uses **Save after write** of the toolbar). Every cell is checked before it is written: it must hold what the car had when you read it. A plate is only written to the garage when the game accepted it on the car.
- **Garage Slot** category (advanced): **Snapshot**, **Auto test**, **Compare** and **Dump slot** to find where an option is saved in the garage slot of a car. Auto test changes the car with a fixed set of unusual values: use a cheap car.
- Optional file `colour_availability.csv` (or `.tsv` / `.txt`) next to the program: the sheet exported or pasted, to complete and correct the diamonds. See HOW_TO_USE.md.
- `HOW_TO_USE.md`, a guide for the whole program and the Vehicle tab.

### Changed
- Tabs: Main, Names, **Vehicle**, Debug. The window title says v1.4.
- The program is bigger (about 2.5 MB): the pictures of the wheels, plates and chameleon paints and the table of native addresses are inside `JDM - RTM.exe`.
- `README.md` and `CREDITS.txt` have the new material and the credits for the images (XB36Hazard) and the data used.

### Things to know
- The changes of the Vehicle tab are seen by the other players of the session. What is saved in the garage is saved in your account. Try everything on a car you do not mind losing.
- Chameleon paints belong to the Expanded and Enhanced version of the game. On PS4 1.57 they may not exist and the car can look wrong: try them first without Save to garage.
- Not every option of the garage slot is mapped yet (interior, dashboard, smoke, mods and the switches of neon, xenon and turbo are not). Those are applied to the car and the game keeps them when it saves the car.
- Tested by jdm_drlft on firmware 14.02 with GTA V 1.57 (EU).

### Files of the Arise code that were touched
`MainForm.cs` (the Vehicle tab and the title), `StatEngine.cs` (a few helpers: `AddressBase`, `HandlerAddresses`, `GetExecutableRegions`, `ScratchOutput` and `CallAddress`, which only runs a native found in the game code and starting like a handler), the project file (version, the embedded pictures and `HOW_TO_USE.md`), `README.md`, `CREDITS.txt` and this file. New files: `Ui/VehiclePanel.cs`, `Vehicle/NativeAddresses.cs`, `Vehicle/PaintColours.cs`, `Vehicle/PaintGroups.cs`, `Vehicle/ChameleonSwatches.cs`, `Vehicle/ColourAvailability.cs`, `Vehicle/WheelPictures.cs` and `Vehicle/wheels.zip`.

## 1.3 - JDM - RTM

### Changed
- **Names tab**: **Office** is merged into **Organization (CEO)** and **MC clubhouse** into **MC club**. Writing the Organization also writes the Office name, and writing the MC club also writes the clubhouse name, with the same text; the other one is no longer a separate option. If a character does not have the linked stat, it is skipped and the log says so. The game is watched afterwards for both. `name_stats.ini` has a new syntax for this (`label = STAT1, STAT2 & LINKED1, LINKED2`); a list of the previous version is replaced and a copy is kept in `name_stats.old.ini`.
- The status bar no longer says **"core UNVERIFIED (first run on this build)"** for GTA V 1.57 EU (`CUSA00411`): the first bytes of its 10 stat functions, read on a console on firmware 14.02 and checked against a code dump of the same console, are built in (`KnownSignatures.cs`), so the core is verified from the first run. A signature stored in `signatures.ini` still wins, and if the bytes of the game differ from the table, or the title is another one, nothing changes: the function stays unverified until its first success, as in Arise.

## 1.2 - JDM - RTM

### Added
- **Notification icon**: an **Icon** box and a **Test** button in the toolbar. The notifications of the tool on the console use that icon number (222 by default). The number is saved in `ui.txt` (which still reads an old file with only `log=0`/`log=1`). Values outside 0-9999 are ignored and shown in red. The ps4debug payload only lets you pick an icon the console already has; it cannot show an image of your own.

## 1.1 - JDM - RTM

Based on **Arise - RTM v2.0.0** by yAmethxst ([repository](https://github.com/Amethxst/Arise-RTM-GTAV-PS4), [release](https://github.com/Amethxst/Arise-RTM-GTAV-PS4/releases/tag/v2.0.0)), used under its MIT license. Everything not listed here is his code, unchanged.

### Added
- **Names tab** (made by jdm_drlft, `Names/NamesRtm.cs`): reads and writes the names of the organization, office, MC club, clubhouse, yacht, nightclub, acid lab, acid lab product and mansion pets, with colour, bold, italic, size and symbol buttons, a live preview that follows the game's rules, HUD colour and image tag lists, and a byte counter (41 bytes). It connects by itself when Arise connects to the game, always uses the **Stats character** chosen in the toolbar, and writes its messages in the window's log.

### Changed
- The program is called **JDM - RTM** (`JDM - RTM.exe`).
- Tabs: Main, **Names**, Debug.
- Green theme (#455217) in the whole window; pointing at a button turns it #884466. Warnings stay amber. All the colours are in `Ui/Theme.cs`.
- Banner: it says "made by jdm_drlft" and keeps a **Credits** link that opens `CREDITS.txt` (licenses and third-party notices).
- The credit and the Discord/X links of the banner were replaced by the line above; the original copyright notice stays in `LICENSE`, and `CREDITS.txt` credits yAmethxst.
- An Arise operation is not started while the Names tab is working, and the other way round.

### Same as Arise - RTM v2.0.0
- `ps4debug.bin` is ps4debug-NG v1.3.4 (adds firmware 14.02), the very same file (SHA-256 `8bfe0507972718e6078778c5f9c44d84293197c0547239ab4227bdb351299b3e`). Not tested on a console by jdm_drlft.
- Outfit editor, fast run, custom K/D, teleport and the Debug tab.

### Files of the Arise code that were touched
`MainForm.cs` (a few lines: the Names tab, the banner, the title, the notification icon), `StatEngine.cs` and the new `KnownSignatures.cs` (built-in signatures), `PS4Connection.cs` (the notification icon), `Ui/Theme.cs` (colours), `Ui/SlotList.cs` and `Ui/DebugPanel.cs` (one colour each), the project file (name and version), `README.md` and `CREDITS.txt`.
