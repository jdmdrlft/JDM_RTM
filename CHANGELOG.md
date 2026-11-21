# Changelog

## 1.8.42 - JDM - RTM

### Fixed
- **Office garage entrance outside did not show** (the elevator of the office worked). The game keeps the office garages as "garage 1 / 2 / 3 of your office", **103, 104, 105**, and finds the building from the office (`am_mp_property_ext.c` func_1058: garage 112 is yours when the office is 90 and slot 8 is 103). 1.8.27 wrote 112, 113, 114 (the garages of Maze Bank Tower as they are), which the entrance outside does not take (func_7087). Unlock properties writes 103, 104, 105 again, as Seren. **Press Unlock properties again, then change session.**
- **Two stat indexes of Seren's table were the wrong slots on 1.57** (hashes read on the console with Check properties of 1.8.41). Index 3233 is `PROP_CLUBHOUSE` (Seren writes 83 there): it now gets 92, the clubhouse, and it is no longer kept as an apartment slot. Index 4032 is slot 11, which the game sets to 115 itself (Seren writes the clubhouse, 92, there): it is no longer written.

### Changed
- The window title says v1.8.42.

### Things to know
- `docs/OFFSETS.md` has a new section with the property slots: slot, index, stat and the office garage rule.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `docs/OFFSETS.md`, `README.md` and this file. Files of JDM - RTM: `Options/PropertyUnlockOption.cs`.

## 1.8.41 - JDM - RTM

### Changed
- **Check properties** also logs the hash of every stat index, to find which stat each index really is on 1.57.
- The window title says v1.8.41.

### Things to know
- **The stat indexes of Seren's Own All Property Slots are shifted on 1.57** (Check properties of 1.8.40). Before the first change, index 1279 and `PROPERTY_HOUSE` were both 42 (the same stat). After it, index 3233 (Seren: `MULTI_PROPERTY_5`) holds 92, the value written to `PROP_CLUBHOUSE`, and indexes 3235, 8980, 8983, 10442 and 10445 hold the values written by name to `MULTI_PROPERTY_5` to `_9`. Index 4032 (Seren: the clubhouse) holds 115. So Unlock properties writes the right values by name, but by index it writes into other stats. The hashes logged now show which ones.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/PropertyUnlockOption.cs`.

## 1.8.40 - JDM - RTM

### Added
- **Debug > Set a stat > Check properties.** Reads every stat Unlock properties writes (by index and by name) and logs each property slot, and every value that is not what Unlock properties writes (`DIFFERENT`). Apartment slots are only shown (they are kept). It writes nothing. The packed ints cannot be read, so they are not checked.

### Things to know
- **The CEO name "Organization" in the office is caused by the Explosive mod menu** (log of 1.8.39): `NETWORK_HAVE_USER_CONTENT_PRIVILEGES` jumps into `explosive_temp.sprx`. With it the game thinks the account has no user content privileges, sets bit 28, and the office script copies "Organization" into the CEO name when you register as boss (1.8.38).
- The office garages are right: in `freemode.c` building 71 is office 90 with garages 112, 113 and 114 (and 87 + 103-105, 88 + 106-108, 89 + 109-111).
- The window title says v1.8.40. Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/PropertyUnlockOption.cs` (`Check`), `Ui/DebugPanel.cs`.

## 1.8.39 - JDM - RTM

### Changed
- **CEO name privileges** first checks if another program has hooked the natives it uses. On the console (log of 1.8.38), `NETWORK_HAVE_USER_CONTENT_PRIVILEGES` started with `FF 25 00 00 00 00` (`jmp [rip]`): a hook that is not of this tool (the tool's hooks start with `E9`). For each hooked native the log now says where it jumps to and the name of the memory region there, so you can see which plugin or mod menu put it. When one is hooked, nothing is changed.
- The window title says v1.8.39.

### Things to know
- A program that hooks the user content privileges can make the game hide every name made by players, the CEO name too (bit 28; see 1.8.38).
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `StatEngine.FrameHook.cs` (`ForeignHook`), `Ui/DebugPanel.cs`.

## 1.8.38 - JDM - RTM

### Added
- **Debug > CEO name privileges** (it replaces **CEO name reset flag**, which was not the cause). It reads `NETWORK_CHECK_USER_CONTENT_PRIVILEGES`, `NETWORK_HAVE_USER_CONTENT_PRIVILEGES`, `NETWORK_HAVE_ONLINE_PRIVILEGES` and `NETWORK_HAVE_SCS_PRIVATE_MSG_PRIV` and logs them. When a user content privilege is "no", it makes `NETWORK_CHECK_USER_CONTENT_PRIVILEGES` and `NETWORK_HAVE_USER_CONTENT_PRIVILEGES` always return true (their first bytes become `mov rax,[rdi]; mov qword [rax],1; ret`), until the game is closed. Pressed again, it puts the old bytes back.

### Things to know
- **Why the CEO name becomes "Organization" in the office.** In the office, as boss, the office script (`am_mp_property_int.c` func_10159) copies the organization name into the CEO name stats. That name comes from func_1200, which gives "Organization" (`GB_REST_ACC`) when bit 28 of the player is set. freemode and the office set bit 28 when `NETWORK_CHECK_USER_CONTENT_PRIVILEGES` fails (the account has no user content privileges). When you retire, the office sign shows the office name (45 + 46) again. That is what was seen: boss = "Organization", retired = the name.
- Only a function that starts with `push rbp; mov rbp, rsp` is changed (so it is longer than the 11 bytes). If a value read is not 0 or 1, nothing is changed.
- **Watch CEO name** no longer stops when the write spy cannot start (on 1.57 the table address of `GET_THIS_SCRIPT_NAME` is not a function).
- The window title says v1.8.38. Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`Reset`; `SetBoolByHash` removed), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `StatEngine.FrameHook.cs` (`ForceTrue`, `RestoreForced`), `Ui/DebugPanel.cs`.

## 1.8.37 - JDM - RTM

### Added
- **Watch CEO name catches who writes the CEO name.** While it runs, a hook on `STAT_SET_STRING` (the same kind of hook as the frame hook: the first bytes jump to a stub of the tool) records each write to `GB_GANG_NAME` / `GB_GANG_NAME2`: the text written and the name of the script that writes it (`GET_THIS_SCRIPT_NAME`, called inside the handler, on the script's own thread). The log shows `WRITE GB_GANG_NAME = "..." by script "..."`. The stub writes nothing; the write goes on as before. The hook is removed when the watcher ends or is cancelled.

### Things to know
- The log of 1.8.36: about 20 seconds after the session loaded, only `GB_GANG_NAME` became "Organización". The office name, the MC name and flags 350 and 351 did not change. So it is not `freemode.c` func_20271 (it would also write the MC name). In the decompiled scripts (a newer build) nothing else writes only that stat without the game keyboard, so on 1.57 it is a script that is different there. The label is in Spanish, so it is written on the console by a script, not by a server.
- Do not write names with the Names tab while Watch CEO name runs: `GET_THIS_SCRIPT_NAME` needs a script thread, and Names writes from the tool's thread.
- The stub was checked byte by byte (each jump), as the frame hook. Not tried on the console yet.
- The window title says v1.8.37.

### Files of the Arise code that were touched
`StatEngine.cs` (`Reset`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `StatEngine.FrameHook.cs` (`InstallStringSpy`, `RemoveStringSpy`, `SpyWrites`, `SpyStub`), `Ui/DebugPanel.cs`.

## 1.8.36 - JDM - RTM

### Changed
- **Watch CEO name** also shows reset flag 351 (`_GET_STAT_HASH_FOR_CHARACTER_STAT(2, 351, ...)`).
- The window title says v1.8.36.

### Things to know
- Every writer of stats 155 + 157 in the scripts was checked (`freemode.c`, `am_mp_property_int.c`, `appinternet.c`). The writers in the office and website scripts only run when you type a name with the game keyboard. The only automatic one is `freemode.c` func_20271, cases 1, 3 and 6 (155 + 157 without the office name 45 + 46, as in the log of 1.8.34). It runs when Social Club rejects the CEO name itself (type 1: the game does not mark it as checked when the session loads, so it is checked again every session), or the MC name (type 3), or when flag 351 is set. The MC name is not proven to be the cause; the watcher shows it, with flag 351.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/DebugPanel.cs`.

## 1.8.35 - JDM - RTM

### Changed
- **Watch CEO name** also shows the MC club name (`MC_GANG_NAME` + `2`).
- The window title says v1.8.35.

### Things to know
- **Why the CEO name goes back to "Organization".** The watcher of 1.8.34 showed it: about 20 seconds after the session loaded, the CEO name (stats 155 + 157) became "Organization" while the office name (45 + 46) kept "jdmm", and flag 350 stayed false. In `freemode.c` func_20271, only cases 1, 3 and 6 write 155 + 157 without 45 + 46, and they write the **MC club name** (47 + 48) too: the game resets the MC name and, with it, the CEO name. That happens when Social Club rejects the MC name (func_20274) or when flag 351 is set. The MC name had been set to one with symbols (`‹ ∑ ›`) and colour codes.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/DebugPanel.cs`.

## 1.8.34 - JDM - RTM

### Added
- **Debug > Read a stat > Watch CEO name.** For 10 minutes it reads, every 2 seconds, the CEO name (`GB_GANG_NAME` + `2`), the office name (`GB_OFFICE_NAME` + `2`) and the reset flag (bool 350), and logs each change with its time, also while the session changes. It only reads. Cancel stops it.

### Things to know
- Even a plain name ("jdmm") went back to "Organization" after a change of session, and the Names tab read "Organization" from the stats. So the game writes that label into the stats; it is not the display of the office, and not a stat the tool forgets.
- In the scripts, the only path that writes "Organization" into the stats without the keyboard is `freemode.c` func_20271: at load when bool 350 is set (func_20270), or when Social Club rejects the name. The watcher shows which one and when.
- The character had no office before Unlock properties (`properties_backup_mp1.txt`: all `PROP_OFFICE*` = 0). 1.8.24 already saw "Organization" on an office given by Unlock properties and not bought.
- No stat name of the tool hashes to the reset flag (checked against every name in the code).
- The window title says v1.8.34. Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/DebugPanel.cs`.

## 1.8.33 - JDM - RTM

### Added
- **Debug > CEO name reset flag.** Reads bool stat 350 of the character (`_GET_STAT_HASH_FOR_CHARACTER_STAT(2, 350, character)`) and clears it when it is set. In `freemode.c` (func_20270), when the session loads and this flag is set, the game writes "Organization" (`GB_REST_ACC`) into the CEO name stats and clears the flag. The scripts never set it, so it comes from outside (the servers).

### Things to know
- The log of 1.8.32 showed that no CEO name stat was missing: stats 155 + 157 are `GB_GANG_NAME` + `GB_GANG_NAME2`, and 45 + 46 are `GB_OFFICE_NAME` + `GB_OFFICE_NAME2`. The office reads 155 + 157 when you enter it (`am_mp_property_int.c` func_463) and shows them as they are.
- Max CEO office cash does not touch the name: it writes only int stats and packed values.
- The window title says v1.8.33. Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`CharacterStatHash` type, `TryGetBoolByHash`, `SetBoolByHash`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/DebugPanel.cs`.

## 1.8.32 - JDM - RTM

### Changed
- **Names, Organization (CEO): the tool also writes the stats the game itself writes.** When you rename the organization in the game, `freemode.c` (func_20271, case 0) writes the text stats number 155 + 157 and 45 + 46 of `_GET_STAT_HASH_FOR_CHARACTER_STAT(3, number, character)`. The tool now gets the hash of those four and writes each one that is not among `GB_GANG_NAME`, `GB_GANG_NAME2`, `GB_OFFICE_NAME`, `GB_OFFICE_NAME2`. The log says which stat each number is ("already written", or "was missing" with its hash). The office showed "Organization" after the name was changed; a missing stat may be why.
- The window title says v1.8.32.

### Things to know
- `_GET_STAT_HASH_FOR_CHARACTER_STAT` is called at 0xACF480 (relative), the address the main tool finds for it on 1.57. Names checks that it starts like a function first.
- Not tried on the console yet. Change session after the name is written.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Names/NamesRtm.cs` (`TextStatHash`, `SetString` by hash, `WriteGameCeoStats`).

## 1.8.31 - JDM - RTM

### Changed
- **Give all weapons and Max ammo run on the game's own thread**, through the frame hook of 1.8.30, as Seren does. Through RPC (the tool's own thread) the game froze at 17/97, the first firearm, inside and outside buildings (log of 1.8.29). Before anything is given, the hook is checked: `PLAYER_PED_ID` on the game thread must give the same ped as through RPC. If the hook cannot be installed or the check fails, nothing is called and the log says why. The hook is removed at the end.
- The check of 1.8.29 for buildings is removed: they work anywhere.
- Max ammo now asks for all weapons at once (has it, its maximum) and fills them in a second pass.
- The window title says v1.8.31.

### Things to know
- **CEO name shows "Organization" in the office.** In the game scripts (`freemode.c` func_20270), when the session loads the game reads a bool stat of the character (number 350 of its list, `_GET_STAT_HASH_FOR_CHARACTER_STAT(2, 350, ...)`). When it is set, the game replaces the CEO name with the label `GB_REST_ACC` ("Organization") and clears it. 351 is the MC name and 353 the yacht name. The tool never writes it. It is likely set by the Rockstar servers after a name check, so a name with colour codes (`~r~`) or symbols may be the cause. Try a plain name first.
- The display can also show "Organization" when the account has no user-content privileges (bit 28 in the player's broadcast data, `NETWORK_CHECK_USER_CONTENT_PRIVILEGES`).
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/WeaponsOption.cs`, `StatEngine.FrameHook.cs` (`CheckedNative`, `HookOutput`).

## 1.8.30 - JDM - RTM

### Added
- **Frame hook, phase 1** (Debug tab, Packed value: **Test frame hook (changes nothing)**). The first step to run natives on the game's own thread, as a mod menu does. Today the tool calls them from its own thread, next to the game's; that is why Give all weapons froze the game inside the office.
  - It patches the first bytes of a native the game's scripts call every frame (`GET_NETWORK_TIME`, else `IS_PAUSE_MENU_ACTIVE`, `IS_PLAYER_ONLINE`, `GET_CLOCK_HOURS`) with a jump to a small stub of the tool. The stub runs the calls the tool leaves for it, then the moved bytes, then the rest of the native.
  - Only a native whose first bytes are simple (push, mov between registers, sub rsp) is hooked. The log shows the first bytes of each one.
  - The test: install, count the calls in 1 second, run `PLAYER_PED_ID` on the game thread and compare it with the same call through RPC, then remove the hook (the first bytes are put back and read back).
  - Nothing else uses it yet. When the test passes on the console, phase 2 moves Give all weapons and Max ammo to it.

### Changed
- The window title says v1.8.30.

### Things to know
- Run the test in Online, with your character on screen. Send `rtm.log` after it.
- It writes into the game code for about 1 second. If the game freezes, restart it: nothing is saved by the test.
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`partial`, `Reset`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `StatEngine.FrameHook.cs` (new), `Ui/DebugPanel.cs`.

## 1.8.29 - JDM - RTM

### Fixed
- **Give all weapons and Max ammo froze the game at 17/97** (the first firearm, `WEAPON_PISTOL`) with the character inside the office. In 1.8.27, outside, the list went much further. Both now check `GET_INTERIOR_FROM_ENTITY` of the character first: inside a building nothing is called and the log says to go outside.

### Changed
- The window title says v1.8.29.

### Things to know
- After **Unlock properties**, the office name and the office cash went back to stock when you spawned in the office. Set them again (Names, Max CEO office cash) after you have been in the office once, wait a few seconds, then change session.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/WeaponsOption.cs`.

## 1.8.28 - JDM - RTM

### Fixed
- **Give all weapons froze the game** (twice in the log of 1.8.27, about 11 seconds into the list, where the 12 DLC weapons the game lists with `GET_NUM_DLC_WEAPONS` and the tool had not got were given). The tool no longer reads or gives those DLC weapons; it gives only its own list of the weapons of 1.57, without `GADGET_PARACHUTE` (it came just before them). Max ammo uses the same list. Seren gives the DLC weapons from inside the game's own thread; the tool calls from outside it.
- **Unlock properties froze the game** when it ran 4 seconds after the game had started again. **Unlock properties**, **Undo properties**, **Unlock all**, **Give all weapons** and **Max ammo** now first check that the game is in an online session (`NETWORK_IS_SESSION_STARTED` and `NETWORK_IS_GAME_IN_PROGRESS`, the same check the Seren plugin makes). If not (loading, Story Mode), nothing is changed and the log says to wait.

### Changed
- The window title says v1.8.28.

### Things to know
- Unlock all worked on the console in 1.8.27 (12,291 writes in about 8 seconds, no failures).
- Wait until you are fully in Online (you can move your character) before you use these buttons.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/WeaponsOption.cs` (`SessionReady`, list only), `Options/PropertyUnlockOption.cs`, `Options/UnlockAllOption.cs`.

## 1.8.27 - JDM - RTM

### Fixed
- **Max ammo** stopped with "SET_PED_AMMO: 0xF38EA0 does not start like a native handler". The address was right (the Seren plugin calls the same one): the handler calls nothing, so it does not start with `push rbp; mov rbp, rsp`. The tool now also accepts such a handler when it starts on a 16-byte boundary and reads the native context (`[rdi]`, `[rdi+8]` or `[rdi+0x10]`) in its first bytes (`StatEngine.LooksLikeHandler`).
- **Give all weapons gave only part of the weapons.** As Seren does, it now also gives every DLC weapon the game lists with `GET_NUM_DLC_WEAPONS` / `GET_DLC_WEAPON_DATA` (1.8.26 left them out for the same reason as Max ammo). Max ammo fills them too. The log says how many came from each list.

### Changed
- **Unlock all is the Unlock All of the Seren plugin**, read from its file (its 22 steps: awards, also every award by index; heists and contacts; clothes; collectibles; tattoos; weapon unlocks, tints and sticky bombs). Its Arena War stats are left out and the Arena War stats of the Explosive unlock-all are written instead. The Seren office step is replaced by the values of **Max CEO cash** (Seren turns the office items off). Max skills and the Pegasus vehicles stay. About 15,000 writes, in the batch loop of 1.8.26. The ints are read first and only raised; -1 and 0 are written as they are. The money is not touched.
- **Unlock properties gives everything again** (asked for, at your own risk): every value of Seren's "Own All Property Slots" (office and its garages, clubhouse, hangar, facility, nightclub, arcade, the apartments, their options and Seren's packed ints), with two changes checked in the game scripts:
  - Office: **Maze Bank Tower** (90, the most expensive office) with **its own garages 112, 113 and 114**. In `freemode.c` (func_1540) 87 = Lombank West, 88 = Maze Bank West, 89 = Arcadius, 90 = Maze Bank Tower, and garages 103-105 are the ones of Lombank West. Seren gives office 90 with garages 103-105, which do not go together; that may be why the office of 1.8.22 was not right.
  - Nightclub: **West Vinewood** ($1,700,000 on the GTA wiki, the most expensive), number 4 in the order of the game (`freemode.c` func_6637). Seren gives Downtown Vinewood (9).
  - The apartment and garage slots you already use are kept, with your cars (in 1.8.22 changing one left the cars out of the garage). The empty ones get Seren's Eclipse Towers apartments, with the three Eclipse Towers penthouses (83, 84, 85).
- The window title says v1.8.27.

### Things to know
- Unlock properties: the clubhouse, hangar, facility and arcade keep Seren's place and options; which place is the most expensive could not be checked for them. There are no mansions in 1.57 (Seren also writes `MANSION_*_OWNED`, which this game does not have). The packed ints cannot be read, so **Undo properties** cannot put them back; the rest it puts back from the copy made the first time.
- Seren also gives the sticky bombs and changes some tunables of the session (holiday clothes): the tool does not. Use **Give all weapons**.
- Like Seren, Unlock all marks the old heists and Doomsday as done and sets `WAS_CHAR_TRANSFERED`.
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`CallAddress`, `LooksLikeHandler`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/UnlockAllOption.cs`, `Options/UnlockAllList.txt`, `Options/WeaponsOption.cs`, `Options/PropertyUnlockOption.cs`.

## 1.8.26 - JDM - RTM

### Changed
- **Unlock all runs like a mod menu.** The tool writes a small loop of its own into the game (x86-64, checked with a disassembler; only when that memory is executable) that runs hundreds of native calls per trip to the console. Unlock all reads the current values in the same way first and only writes what is missing: an int lower than its target, a bitset not yet -1. So it takes a few trips instead of thousands, nothing is lowered and nothing already done is written again. The Arena career, the award tattoos and the Seren clothes and skills go in the same batches.
- **Max CEO cash** turns the office items on, as the Explosive SPRX menu does: packed bools 7553-7594 and 9400-9414 on, packed ints 9357 = 4 and 7671 = 100 (Seren turned 7553-7594 off, "clutter cleared"). In `freemode.c` (func_12834) these are the item displays of the office. The cash piles themselves (levels 1-24, packed int 7666 = 25) only show up to the money of the character: the game shows level n when bank + wallet reach the amount of that level, so a character with little money gets fewer piles. The log says so.
- **Unlock properties** only fills the empty apartment and garage slots (`PROPERTY_HOUSE`, `MULTI_PROPERTY_1` to `_10`), which is what the Garage Editor needs. The business properties of Seren's option (office, clubhouse, hangar, facility, nightclub, arcade, agency) are not given any more: given that way the office was not really owned, and in it the organization name went back to the default and the cash was not right. **Undo properties** takes away what older versions gave.
- **Give all weapons / Max ammo**: on 1.57 the natives table has a wrong address for `GET_NUM_DLC_WEAPONS` (0x9A2CF0 is not a function start, log of 1.8.25), so the tool has its own list of the weapons of 1.57 and gives them with `GIVE_WEAPON_TO_PED` (9999 rounds) / fills them with `GET_MAX_AMMO` and `SET_PED_AMMO`. The attachments are not given (they came from the same DLC natives). If a native address is wrong, nothing is called and the error shows the bytes that are there.
- The window title says v1.8.26.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (the batch loop, `ReadIntsBatch`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/UnlockAllOption.cs`, `Options/OfficeCashOption.cs`, `Options/PropertyUnlockOption.cs`, `Options/WeaponsOption.cs`, `Options/TattooAwardsOption.cs`, `Options/PegasusUnlockOption.cs`.

## 1.8.25 - JDM - RTM

### Fixed
- **Garage cars disappeared after an edit (1.8.23 and 1.8.24).** Those versions turned on bit 10 of cell 104 of every slot they wrote, taken for "save this slot". In the game scripts bit 10 goes with bits 1, 6 and 16: the garage only shows a car when none of them is on, and bit 10 is the destroyed-car claim. That is gone, and **Read garages** (also when the Garage Editor tab opens) turns bit 10 off on every saved car, so the hidden cars show again (on a car really destroyed it is a free insurance claim). The log says which cars.

### Added
- **The garage you are in shows the changes at once** (as in the video of the other tool). Every slot the tool writes gets bit 12 of cell 104, which the garage interior reads as "create this car again" (`am_mp_property_int.c` func_683; the game sets it itself when two cars change places, `freemode.c` func_12339): the car on display is deleted and created again from the slot. Edits, imports, Add vehicle, Replace, Repair and Undo garage. Outside a garage nothing happens.

### Changed
- **Unlock all is about three times faster.** The stat writes go in batches (`StatEngine.RunBatch`): the arguments of up to 160 calls are written to the console at once, then the calls are run, and results are read only where needed (the hashes of the stats by index). About one trip per value instead of three.
- The window title says v1.8.25.

### Things to know
- Saving garage edits to the account (so they stay after Story Mode or a restart) is not solved yet: the request the game uses for it could not be confirmed on 1.57, so the tool does not write it. A safe way: after an edit, move the car to another space with **Manage Vehicles** in the garage (the game saves the garage itself), or take the car out and store it again.
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`RunBatch`, `HandlerAddress`, `StatHashHandler`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `docs/OFFSETS.md`, `README.md` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs` (`GlobalMemory.Recreate`, `ShowHiddenCars`), `Ui/GarageManager.cs`, `Ui/GarageReplace.cs` (messages), `Options/UnlockAllOption.cs`, `Options/SerenUnlocks.cs`.

## 1.8.24 - JDM - RTM

### Changed
- **Unlock properties no longer replaces what you have.** Seren's "Own All Property Slots" sets every slot to a fixed property: on a real account (1.8.22) it changed the apartment of the character (`PROPERTY_HOUSE` 30 -> 1), so its cars were no longer in the garage, and it gave an office that was not bought. Now it reads every value first and only writes where the character has nothing (0). To get back what 1.8.22 / 1.8.23 changed, press **Undo properties** (it uses the copy made before the first change).
- **Garage edits are saved by the game within about 20 seconds** (before: 3 minutes, so going to Story Mode right after an edit lost it). The slot is put in the game's list of pending saves (`Global_2739239`) with a timer that already ran out, so freemode saves it on its next pass over the slots. Only when that list looks as expected; otherwise the 3 minutes of 1.8.23 stay.
- The window title says v1.8.24.

### Things to know
- The office name back to "Organization" and fewer cash piles were seen on a character with the office given by Unlock properties of 1.8.22 (not bought). Undo properties takes that office away.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/PropertyUnlockOption.cs`, `Ui/VehiclePanel.cs` (`GlobalMemory.SaveSoon`), `Ui/GarageManager.cs`, `Ui/GarageReplace.cs` (messages).

## 1.8.23 - JDM - RTM

### Changed
- **Garage cars stay after the game is closed.** The game keeps a saved car in two places: the slot array in memory (what the Vehicle and Garage Editor tabs write) and character stats (what goes to your account). A write in the array alone was lost when the game was closed. In freemode.c the game copies a slot to the stats by itself when bit 10 of its cell 104 is on (3 minutes later it saves the whole car and asks for a save). Now every write of the tool in a slot turns that bit on (edits, imports, Add vehicle, Repair, Replace, Undo garage), so the game saves the car itself. **Wait about 3 minutes before closing the game.**
- **The CEO name (and every name of the Names tab) stays after the game is closed.** The Names tab saved with `STAT_SAVE(0, false, 3, false)`, the old arguments that 1.8.11 had already changed in the rest of the tool: the names stayed for the session but were back to the old ones after the game was closed. It now uses `STAT_SAVE(0, true, 3, false)`, as the Seren plugin.
- The window title says v1.8.23.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `docs/OFFSETS.md`, `README.md` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs` (`GlobalMemory`: the save bit), `Ui/GarageManager.cs`, `Ui/GarageReplace.cs` (messages), `Names/NamesRtm.cs` (`Save`).

## 1.8.22 - JDM - RTM

### Added
- **Unlock properties** and **Undo properties** (Main tab, **Stats** section): the "Own All Property Slots" of the Seren plugin (read from its code, `Options/PropertyUnlockOption.cs`). It gives the chosen character every property slot (`PROPERTY_HOUSE`, `MULTI_PROPERTY_1` to `_10`, office 90 and its garages 103-105, clubhouse 92, hangar 116, facility 117, nightclub 118, arcade garage 126, agency 128 and the "owned" stats), about 140 character stats by index, and some packed ints, so every garage can be used. **It replaces the properties the character has**: before the first change the old values are saved to `properties_backup_mp0.txt` / `_mp1.txt` (in `.gitignore`), and **Undo properties** writes them back (the packed ints cannot be read, so they stay).
- **Weapons** section (Main tab, middle column): **Give all weapons** and **Max ammo**, as the "Edit weapons" section of Seren (`Options/WeaponsOption.cs`). Give all weapons gives every weapon of the game's weapon list with 9999 rounds and its attachments (not the finishes and camos), and Max ammo fills every weapon you have to its maximum. They work on the character you play, with natives from the tool's thread: **experimental** (in 1.8.0 a native on the character crashed the game once).

### Changed
- **Arena lvl 1000** writes the values of the "Arena tier level 1000" of the Explosive SPRX mod menu: `MPx_ARENAWARS_AP` = 1000, `_AP_TIER` = 1000 and `_AP_LIFETIME` = 47,551,850 (before: Seren's values, with the skill and career stats; **Unlock Pegasus** still writes those).
- Main tab: the rows of the **Stats** section are closer (the hint under them was taken out) and the **Import** box is shorter, to make room for the new buttons.
- The window title says v1.8.22.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`Options/ProgressionLevelOption.cs` (Arena), `Ui/OutfitsPanel.cs` (Weapons section, Import box), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt`, `.gitignore` and this file. Files of JDM - RTM: `Ui/StatsSection.cs`. New files: `Options/PropertyUnlockOption.cs`, `Options/WeaponsOption.cs`.

## 1.8.21 - JDM - RTM

### Added
- **Unlock all** also does the **Max All Skills** and **All clothes** of the Seren plugin (read from its code, `Options/SerenUnlocks.cs`):
  - skills: `MPx_SCRIPT_INCREASE_STAM`, `_STRN`, `_LUNG`, `_DRIV`, `_FLY`, `_SHO`, `_STL`, `MPx_STAMINA`, `_STRENGTH`, `_LUNG_CAPACITY`, `_WHEELIE_ABILITY`, `_FLYING_ABILITY`, `_SHOOTING_ABILITY` and `_STEALTH_ABILITY` = 100;
  - clothes: about 600 character stats by index (as the game scripts use them) = -1, the packed bools of the clothes (28319-28321, 32407-32409, 51187-51378, 54569-54786, 59971-60117) = true, 12 indexes (11826-11831, 12222-12227) = -1 as a stat and as a packed int, and the `MPx_DLC1/22024-2026PSTAT_BOOLn` and `MPx_CASINOHSTPSTAT_BOOLn` stats = -1. Many of them are of DLCs after 1.57: the game ignores them. The tunables that Seren also turns on are not written (they only last for the session).

### Changed
- **Unlock all is much faster**: it writes every value straight away, with no read before and no read-back (1.8.19 read each counter first so as not to lower it, which doubled the time). A counter that was higher than the list is set to the list value.
- The window title says v1.8.21.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. Files of JDM - RTM: `Options/UnlockAllOption.cs`. New file: `Options/SerenUnlocks.cs`.

## 1.8.20 - JDM - RTM

### Changed
- **Apply is instant in the Vehicle and Garage Editor tabs**, as in the Outfit Editor:
  - after **Apply changes** the car is not read again when every change went well (that read of every mod, colour and livery name took seconds); it is read only when something failed, to show what the car really has;
  - the natives check runs once per connection, not before every Apply;
  - a notification on the console when the car or the garage car is updated ("Vehicle updated!", "Garage car ... updated!").
- The window title says v1.8.20.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs`.

## 1.8.19 - JDM - RTM

### Added
- **Unlock all** (Main tab, **Stats** section, under **Max CEO cash**): unlocks everything the stats of the chosen character can unlock: clothes (shop unlocks and gifts), weapons and tints, car mods, awards and their counters, and more, then the tattoos (**Unlock tattoos**) and the Arena career with the Pegasus vehicles (**Unlock Pegasus**). The list (2,649 values, `Options/UnlockAllList.txt`, built into the exe) is the Recovery of the Explosive SPRX mod menu, without what can break the character: story/heist flow stats, worn tattoos, `FIXER_HQ_OWNED`, favourite outfits, casino stats, play times, values of 0, stats this game build does not have, and packed bools 7553-7594 (they take the cash of the CEO office away). Counters are only raised. The money is not touched. One save at the end.

### Changed
- **Custom RGB of the paints, rewritten.** In a saved car the game keeps the custom paint switches in cell 77 as bit 13 (primary) and bit 12 (secondary), and one RGB for both in cells 71-73 (read from the save and load of a car in the decompiled freemode script). The tool used bit 9, which is "tyres cannot burst": so the custom paint did not stay, or came back after it was removed. Now:
  - the garage switches are set to what you choose, compared with what the slot holds (not with the car out);
  - the custom secondary RGB is saved in the garage too (Vehicle and Garage Editor tabs, and imports). When both are on, the garage keeps the primary RGB, as the game does;
  - with the car out, removing a custom RGB also paints the palette colour again, so it shows at once, and the tool checks that the game turned it off.
- **The actions are instant**, as in the Outfit Editor:
  - no confirmation window (unless you tick **Ask before applying**, now off by default);
  - the window is not covered by "Working..." any more: a click while something runs waits for its turn instead of being refused;
  - every change that went well shows a notification on the console;
  - **Save after write** asks the game to save and goes on (before, it waited up to 15 seconds for the end of the save). Wait a couple of seconds before changing session.
- The window title says v1.8.19.

### Things to know
- If a custom primary was set in the garage with 1.8.18 or before, bit 9 of cell 77 ("tyres cannot burst") of that slot may be on. Bulletproof tyres are bought in the mod shop, so check that car there.
- Properties cannot be unlocked with stats (they must be bought), and the PS4 trophies are given by the game when it gives the award. Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (`RunAsync`: queue, no lock, notifications, title), `StatEngine.cs` (`SaveStats`), the project file (version, the list as a resource) and `build/ai.cs` (version), `docs/OFFSETS.md`, `README.md`, `CREDITS.txt` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs`, `Vehicle/GarageCarFile.cs`, `Ui/StatsSection.cs`. New files: `Options/UnlockAllOption.cs`, `Options/UnlockAllList.txt`.

## 1.8.18 - JDM - RTM

### Changed
- **Unlock Pegasus** again makes all the normal Pegasus vehicles owned, as in 1.8.15. 1.8.16 left it out to test if the ownership bits stopped the deliveries: they did not (with the old values Pegasus did not deliver either), and without them the Pegasus menu is empty. Before the first change of a character it saves the old values to `pegasus_backup_mp0.txt` / `_mp1.txt` (an existing backup is kept), so **Undo Pegasus** can put them back.
- **Unlock Pegasus**: `ARN_VEH_MONSTER` and `ARN_VEH_ISSI` (from Seren, refused by the game) are now `ARN_VEH_MONSTER3`, `_MONSTER4`, `_MONSTER5` and `ARN_VEH_ISSI6` = 1000 (the names of the Explosive SPRX list, read on the console with **Compare with the game**).
- The window title says v1.8.18.

### Things to know
- The Pegasus deliveries still do not arrive on the test account. Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/PegasusUnlockOption.cs`, `Options/PegasusRestoreOption.cs` (`OwnAll`).

## 1.8.17 - JDM - RTM

### Added
- **Debug > Run a list from a file > Compare with the game (writes nothing)**: reads the value of every stat of a list on the chosen character and saves the ones that differ in `list_compare_<list>_mp0.txt` / `_mp1.txt` next to the exe (in the list format, with the value of the game in a comment). Stats that cannot be read are listed as comments. Packed integers cannot be read and are only counted. It writes nothing to the game. It is to find which stats an unlock of another tool changes (for example, the Pegasus deliveries).
- The window title says v1.8.17.

### Things to know
- The report has values of your account: do not share it. It is in `.gitignore`.

### Files of the Arise code that were touched
`Options/StatList.cs` (`Compare`), `Ui/DebugPanel.cs` (button), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `.gitignore` and this file.

## 1.8.16 - JDM - RTM

### Added
- **Undo Pegasus** (Main tab, **Stats** section, under **Unlock Pegasus**): puts the Pegasus ownership stats of the chosen character back to the values they had before 1.8.14 / 1.8.15 changed them. The old values are read from `pegasus_backup_mp0.txt` or `pegasus_backup_mp1.txt` next to the exe. Each value is read back. Then go to Story Mode and back to Online, because the game keeps a copy of them in memory.

### Changed
- **Unlock Pegasus** writes only the Arena stats again, as the Seren plugin does. With the ownership bits of 1.8.14 / 1.8.15, Pegasus listed every vehicle but delivered none (the blip went away and no vehicle came), not even the ones the game gave. The game gives the Pegasus vehicles of the Arena career itself.
- The window title says v1.8.16.

### Things to know
- In the delivery script (`am_vehicle_spawn`), Pegasus cancels a delivery without a message when its delivery transaction fails twice. Not confirmed yet which check fails.
- Not tried on the console yet.

### Files of the Arise code that were touched
`Ui/OutfitsPanel.cs` (height of the stats section), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `.gitignore` and this file. Files of JDM - RTM: `Options/PegasusUnlockOption.cs`, `Ui/StatsSection.cs`. New file: `Options/PegasusRestoreOption.cs`.

## 1.8.15 - JDM - RTM

### Changed
- **Unlock Pegasus**: on the console, 1.8.14 did not change the Pegasus vehicles. The log said "0xECA6D0 does not start like a native handler": the natives table has a wrong address for `_GET_STAT_HASH_FOR_CHARACTER_STAT` (0xACA6D0). In the native list this handler comes between `STAT_GET_LOAD_SAFE_TO_PROGRESS_TO_MP_FROM_SP` (0xACF460) and `STAT_SET_INT` (0xACF4B0), so the tool now looks for the one function start between those two (it only reads memory) and uses it. The log shows the address it found. If it does not find exactly one, nothing is called.
- The window title says v1.8.15.

### Things to know
- The checks of 1.8.14 stay: each Pegasus stat must be readable, and stat 1725 must be 0 or a time, or nothing is written.
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`CharacterStatHash` finds its handler), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `docs/OFFSETS.md` and this file.

## 1.8.14 - JDM - RTM

### Changed
- **Unlock Pegasus** now also makes all the normal Pegasus vehicles yours. Before, Pegasus did not answer the phone: the Arena stats unlock the career, but the account owned no Pegasus vehicle. The game keeps the owned Pegasus vehicles as a set of bits in 5 character stats that the scripts use by index (1726-1730, with the time of the last change in 1725), not by name. The tool asks the game for their hashes (`_GET_STAT_HASH_FOR_CHARACTER_STAT`), reads them, adds the bits of every vehicle (0-138), and reads them back. Bits are only added, never removed. The special vehicles of Pegasus (the 7 that the game leaves out of the "has Pegasus vehicles" check) are not added.
- The window title says v1.8.14.

### Things to know
- The stat indices come from the freemode script of a newer game build. As a check, the tool reads stat 1725 first: if it is not 0 or a time, it changes nothing and says so in the log. The log also shows the hash and the old value of each stat.
- Change session, then call Pegasus. Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`CharacterStatHash`, `SetIntByHash`, `TryGetIntByHash`), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `docs/OFFSETS.md` and this file. Files of JDM - RTM: `Options/PegasusUnlockOption.cs`.

## 1.8.13 - JDM - RTM

### Added
- **Unlock Pegasus** (Main tab, **Stats** section, next to **Max CEO cash**): completes the Arena War career of the chosen character, which unlocks its Pegasus vehicles. It writes the two lists of the "Arena War unlocked" option of the Seren plugin: 94 Arena stats (career tier and unlocks, skill, awards, wins and matches, and the use of every Arena vehicle, `ARN_VEH_*`) and 23 Arena awards set to true, `AWD_PEGASUS` among them. Unlike Seren, a counter that is already higher is not lowered. It asks first and follows **Save after write** and **Verify read-back**.

### Changed
- **Arena lvl 1000** writes what Seren writes: besides the tier and the Arena Points, `MPx_ARENAWARS_SKILL_LEVEL` = 20, `MPx_ARENAWARS_SP` and `_SP_LIFETIME` = 209, and `MPx_ARENAWARS_CARRER_UNLK` = 44 (the career unlocks). Before, the tier showed 1000 but the career rewards, the Pegasus vehicles among them, stayed locked.
- **Max CEO office cash** is now the half-width button **Max CEO cash**.
- The window title says v1.8.13.

### Things to know
- Not tried on the console yet. Which stat opens each Pegasus vehicle is not known one by one; the two buttons write what Seren writes.

### Files of the Arise code that were touched
`Options/ProgressionLevelOption.cs` (Arena), `Ui/StatsSection.cs` (buttons), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. New file: `Options/PegasusUnlockOption.cs`.

## 1.8.12 - JDM - RTM

### Changed
- **Tattoo Editor**: the tattoos are read only when you open the Tattoo Editor tab (and when you change character while it is open). Connecting the game or choosing a character in another tab no longer runs **Read tattoos**. When you leave the tab, a read that is still waiting is dropped.
- The window title says v1.8.12.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.8.11 - JDM - RTM

### Changed
- **Save after write forces the save**, as the Seren plugin does: it waits while the game has a save pending (`STAT_SAVE_PENDING_OR_REQUESTED`), calls `STAT_SAVE(0, true, 3, false)` (before: `STAT_SAVE(0, false, 3, false)`), then waits up to 15 seconds until the game has finished saving, and the log says so ("The game finished saving."). Some values (for example **Max CEO office cash**) were lost on a session change. It is used by every option and by the garage writes.
- **Tattoo Editor**: choosing Character 1 or 2 in the toolbar loads the tattoos of that character. Before, the read started while the Names tab was reading the names of the new character and was refused. Now it waits a moment, waits until the tool is free and tries again (3 times).
- The window title says v1.8.11.

### Things to know
- From the game scripts: the contraband sell script only raises the packed int 7666 while it is under 24, so the office cash value of the tool (25) is not changed back by the game.
- Not tried on the console yet.

### Files of the Arise code that were touched
`StatEngine.cs` (`SaveStats`), `Options/OptionTypes.cs` (the save note in the log), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.8.10 - JDM - RTM

### Changed
- **Max CEO office cash** writes the packed int 7666 = **25** (it was 24, as Seren). The office has 24 cash sets (`Cash_Set_01`-`24`). The game's freemode script checks the levels 1 to (packed 7666 - 1), so with 24 the last cash set was never shown.
- The window title says v1.8.10.

### Things to know
- From the same script: each cash level is only shown when your money (bank + wallet) reaches an amount set by the game's tunables for that level. With less money, fewer piles show, whatever the stats say. The tool does not change your money.
- The `LIFETIME_*` stats it also writes (as Seren) are not used by that part of the script.
- Found in https://github.com/calamity-inc/GTA-V-Decompiled-Scripts (`freemode.c` func_12834 and func_12849, `am_mp_property_int.c`). Those scripts are of a newer build; the packed indices 7666 and 7553-7594 are the same that Seren uses on 1.57.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Options/OfficeCashOption.cs`.

## 1.8.9 - JDM - RTM

### Changed
- Tattoo Editor tab: the note under the list is shorter ("Tick the tattoos you want and press Save to character (it must be the character you play).").
- The window title says v1.8.9.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.8.8 - JDM - RTM

### Added
- **Max CEO office cash** (Main tab, **Stats** section, under the two level buttons): fills the CEO office of the chosen character with cash, as the Seren plugin does. It writes `MPx_LIFETIME_BUY_COMPLETE`, `MPx_LIFETIME_BUY_UNDERTAKEN`, `MPx_LIFETIME_SELL_COMPLETE` and `MPx_LIFETIME_SELL_UNDERTAKEN` = 1000, `MPx_LIFETIME_CONTRA_EARNINGS` = 25,000,000, the packed int 7666 = 24 and the packed bools 7553-7594 = false (the office clutter). Like the other options it asks first and follows **Save after write** and **Verify read-back**. Seren also deletes the clutter objects around you; the tool does not (deleting objects from outside the game thread is not safe): enter the office again to see the cash.

### Changed
- Tattoo Editor tab: **Tattoo scan** is removed, and **Unlock tattoos** is one button under the list, with no text next to it (its confirmation still lists what it writes). The list is taller.
- The window title says v1.8.8.

### Things to know
- Not tried on the console yet: **Max CEO office cash**.
- `Options/TattooScan.cs` is not used by any button any more; it is kept in the source.

### Files of the Arise code that were touched
`Ui/StatsSection.cs` (new button), `Ui/OutfitsPanel.cs` (a taller Stats section), `MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. New file: `Options/OfficeCashOption.cs`. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.8.7 - JDM - RTM

### Changed
- **English names** in the Tattoo Editor: the first column and the picture show the English name. The name the game shows (in the language of the console) is in the **In game** column, to find the tattoo in the shop.
- The window title says v1.8.7.

### Things to know
- Tattoo prices are not stats of the character: the shop works them out as base price x a multiplier from the game's tunables (script globals, set by the server each session). Making every tattoo cost $100 means writing those globals, which reset each session; their place in 1.57 is not known yet.
- The game has no "bought" state for a tattoo besides wearing it (`MPx_TATTOO_FM_CURRENT`).

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.8.6 - JDM - RTM

### Added
- **Base tattoos in the Tattoo Editor**: the 67 tattoos of the base game (the award ones like Clown, Clown and Gun, Clown Dual Wield, Racing Brunette, Racing Blonde, Skull, the scrolls, and the first tattoos of the shop) are in the list now, with their English name, price and picture. They can be ticked and saved like the others.
  - Their bit is their index in the tattoo list of the game's `tattoo_shop` script (bits 0-70 of `MPx_TATTOO_FM_CURRENT` / `_UNLOCKS`; the DLC tattoos start at 129, as the same script and the Seren plugin say). Their overlay names follow the rules of that script (`FM_Tat_Award_`, `FM_Tat_`, `M_` / `F_`).
  - The table (bit, label, male and female overlay, zone, price) is inside the program. It was checked against https://github.com/root-cause/v-tattoos: 66 of 67 male and 65 of 67 female tattoos have the same label and overlay there. The other one (bit 70, `TAT_FM_101`) is known to no list and has no picture.
  - The crew emblems (bits 13-16, 71, 72) are not in the list and **Save to character** does not touch them.
- `tattoo_images/urls_fivemad.txt` has the artwork of the base tattoos too: run `download_fivemad.ps1` again to get them (the others are skipped).

### Changed
- The window title says v1.8.6.

### Things to know
- Not tried on the console yet: saving a base tattoo.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. Files of JDM - RTM: `Options/TattooEditor.cs`, `Ui/TattooPanel.cs`.

## 1.8.5 - JDM - RTM

### Changed
- **Tattoo pictures are the artwork of fivemad.com** (clear background) when it is in `tattoo_images/art`, else the GTA wiki picture. Every tattoo the list knows has one now: 674 male and 677 female (before, 612 and 614). `tattoo_images/download_fivemad.ps1` downloads them, for your own use: they are the work of fivemad.com and are not inside the program.
- The artwork is drawn on light grey in the editor, as the web page shows it, so the dark ink can be seen; the files keep their clear background.
- The window title says v1.8.5.

### Things to know
- Checked: for 1348 of the 1351 tattoos the English name of fivemad.com is the same as the one of v-tattoos. The other 3 (Vespucci Beauty, Shark, Wheel) have wrong names on fivemad.com (jackets), but their pictures are right: the picture is chosen by the overlay hash, which is the one the game gives. A sample of pictures was compared by eye with the names the game shows.
- The base tattoos of the game (the award ones like Clown, Racing Brunette, and the first ones of the shop) are not in the editor list: the list comes from `GET_TATTOO_SHOP_DLC_ITEM_DATA`, which gives only the DLC tattoos. Their bits are 0-128 of `MPx_TATTOO_FM_CURRENT` (the DLC ones start at 129). **Unlock tattoos** unlocks them anyway.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. Files of JDM - RTM: `Options/TattooEditor.cs`.

## 1.8.4 - JDM - RTM

### Added
- **Online tattoo editor web** (Tattoo Editor, under the picture): opens the Ped Decorations tool of https://www.fivemad.com/tools/decorations. With a tattoo chosen in the list, it opens on that tattoo (`?q=` + its overlay name), with its artwork and its preview on a freemode ped.
- `tattoo_images/pictures.txt` has the overlay name of each tattoo (for example `MP_Bea_M_LArm_001`), from https://github.com/root-cause/v-tattoos.

### Things to know
- The artwork of fivemad.com (with a clear background) is not downloaded into the tool: its `robots.txt` asks that `/media/` is not fetched by programs and says those files are its own work. The link shows them instead.
- The window title says v1.8.4.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`, `Options/TattooEditor.cs`.

## 1.8.3 - JDM - RTM

### Added
- **Show on character now** (Tattoo Editor, on by default): after **Save to character**, the tattoos are put on your character on screen, as the Seren plugin does. New tattoos are added one by one (`ADD_PED_DECORATION_FROM_HASHES`); when one was taken off, the character is cleared (`CLEAR_PED_DECORATIONS_LEAVE_SCARS`) and all are put on again. The stats are saved first, so a problem here does not lose the save. Experimental: in 1.8.0 this crashed the game once; untick it if it happens again.
- **English** column and English names in the search: the game shows the names in its own language.

### Changed
- **Unlock tattoos**: **Unlock all tattoos** and **Unlock award tattoos** are one button now. It sets `MPx_TATTOO_FM_UNLOCKS_0`-`_51` and `MPx_TATTOO_FM_VIEWED_0`-`_51` to -1 and raises the award stats, with one save at the end.
- **Pictures** are matched by the overlay hash of each tattoo (the same in every language), not by name. Before, the names of the game (in Spanish on a real console) were compared with the English names of the wiki: most pictures were missing and some were wrong. The list is `tattoo_images/pictures.txt`: overlay hash, gender, English name, price, picture, label. English names: https://github.com/root-cause/v-tattoos. A tattoo with a picture of the other gender only uses that one. 612 male and 614 female tattoos have a picture; about 60 per gender are not on the wiki page.
- **Prices**: 248 tattoos per gender have no price in the game's shop data (0). Their price now comes from the list (v-tattoos, then DurtyFree `pedOverlayCollections.json`).
- 18 shop items with no label, no name and no price are not shown any more (they are not tattoos of the shop).
- Bigger picture (16:9, 328 x 185) and 16:9 thumbnails. `tattoo_images/download.ps1` downloads 480 px pictures into `tattoo_images/big`.
- The window title says v1.8.3.

### Things to know
- **Unlock award tattoos** was tried on a real console (Character 2): the 22 award stats were written and saved with no errors.
- Not tried on the console yet: **Show on character now**, **Unlock tattoos** as one button.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`, `Options/TattooEditor.cs`, `Options/TattooUnlockOption.cs`, `Options/TattooAwardsOption.cs`.

## 1.8.2 - JDM - RTM

### Added
- **Unlock award tattoos** (Tattoo Editor tab): the "Unlockable" tattoos of the shop (Clown, Clown and Gun, Clown Dual Wield, Clown Dual Wield Dollars, Racing Brunette, Racing Blonde, the skulls, the scrolls...) are not opened by `MPx_TATTOO_FM_UNLOCKS` but by award stats. This button raises each of those stats to the number its tattoo needs, from the list of the GTA wiki (for example `MPx_AWD_HOLD_UP_SHOPS` = 20 for the clowns, `MPx_AWD_RACES_WON` = 50 for Racing Brunette). A stat that is already higher is left as it is. The confirmation lists every stat. `MPx_KILLS_PLAYERS` (1000) changes your K/D.
- **Tattoo pictures** in the editor: a small picture on each row and a big one of the chosen tattoo. They are read from the folder `tattoo_images` next to the program; `tattoo_images/index.txt` (made from the GTA wiki, 1358 pictures of 720 tattoos) says which picture goes with which tattoo, by zone, name and gender. The pictures themselves are not in the program (they are pictures of the game).

### Things to know
- The stat names of **Unlock award tattoos** are in the stat list of the Seren plugin, so they exist in 1.57. Which stat opens which tattoo comes from the wiki's conditions; not tried on the console yet. A stat the game does not have is skipped and named in the log.
- Pictures are matched by the tattoo names the game gives, so they show after **Rebuild list** has read the names.
- The window title says v1.8.2.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt`, `.gitignore` and this file. New file: `Options/TattooAwardsOption.cs`. Files of JDM - RTM: `Ui/TattooPanel.cs`, `Options/TattooEditor.cs`.

## 1.8.1 - JDM - RTM

### Added
- **Tattoos load by themselves**: when the game connects and when you change character in the toolbar, the Tattoo Editor reads which tattoos that character has (54 stats, a few seconds). It waits for the Names tab to finish first.
- **Gender of each character** (Male / Female in the Tattoo Editor): the tool sees it from the model of the character you play and keeps it in `tattoo_characters.ini`. You can also choose it by hand; the choice is kept.
- **Rebuild list** reads the whole tattoo shop of the gender you play once (a few minutes) and keeps it in `tattoo_list_male.txt` / `tattoo_list_female.txt`. After that the list opens at once.

### Changed
- **Save to character no longer crashes the game.** After saving, 1.8.0 put the tattoos on your character on screen (`ADD_PED_DECORATION_FROM_HASHES`). Run from the tool, that crashed the game on a real console (the stats were already written and saved). The tattoos are not put on screen any more: change session to see them. **Show on character** is removed, and **Clear all** only writes the stats.
- **Read tattoos** is now **Reload** (a few seconds, it uses the kept list).
- Reading the shop makes one call less per tattoo (no `DOES_TEXT_LABEL_EXIST`), so it is faster.
- The window title says v1.8.1.

### Things to know
- **Unlock all tattoos** writes the same stats as Seren for Online, and all 54 `MPx_TATTOO_FM_UNLOCKS` stats are at -1 after it. Some tattoos still show locked. The shop gives 141 male tattoos a lock of their own, in 6 packs (labels `TAT_HP_`, `TAT_L2_`, `TAT_BB_`, `TAT_BUS_`, `TAT_X2_`, `TAT_LX_`). What opens those locks is not known yet.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (automatic load of the tattoos, title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/TattooPanel.cs`, `Options/TattooEditor.cs`.

## 1.8.0 - JDM - RTM

### Added
- **Tattoo editor** (Tattoo Editor tab), made like the one of the Seren plugin:
  - **Read tattoos** reads the tattoo shop of your character's gender and the tattoos saved on the character. Hair overlays and prints for clothes are left out.
  - The tattoos are shown by body zone: Back, Back Full, Chest, Stomach, Head, Left Arm, Right Arm, Left Leg, Right Leg. There is a search box. The ones saved on the character start ticked.
  - **Show on character** puts the ticked tattoos on your character now (a preview, only for you, nothing is saved).
  - **Save to character** saves the ticked tattoos: for each shop tattoo, bit (shop index + 129) of `MPx_TATTOO_FM_CURRENT_<bit / 32>` is set or cleared, and the saved ones are marked unlocked in `MPx_TATTOO_FM_UNLOCKS`. Only the stats that change are written. It asks first and follows **Save after write** and **Verify read-back**.
  - **Clear all** takes every tattoo off: `MPx_TATTOO_FM_CURRENT_0` to `_53` = 0. Unlocked tattoos stay unlocked.
  - The character chosen in the toolbar must be the one you play; the tool checks it (`MPPLY_LAST_MP_CHAR`).

### Changed
- **Unlock all tattoos** now does what Seren does: `MPx_TATTOO_FM_UNLOCKS_0`-`_51` and `MPx_TATTOO_FM_VIEWED_0`-`_51` = -1. It never touches `MPx_TATTOO_FM_CURRENT`, so no tattoo is put on the character.
- Natives table: `GET_FILENAME_FOR_AUDIO_CONVERSATION` (the text of a label) is at `0x9E0A80`, not `0x9DCEE0`, as the Seren plugin calls it. The tattoo names are read again (Tattoo editor and Tattoo scan).
- The window title says v1.8.0.

### Things to know
- Over 27 tattoos the count turns orange: the game may not show them all at once (Seren calls 27 the "stock" limit).
- The livery names of the Vehicle tab still are not read: `GET_MOD_TEXT_LABEL` (`0xB22020`) is also wrong in the table, and its right address is not known yet.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md`, `CREDITS.txt` and this file. New file: `Options/TattooEditor.cs`. Files of JDM - RTM: `Ui/TattooPanel.cs`, `Options/TattooUnlockOption.cs`, `Vehicle/NativeAddresses.cs`, `docs/OFFSETS.md`.

## 1.7.3 - JDM - RTM

### Added
- **Unlock all tattoos** (Tattoo Editor tab) works now. It sets `MPx_TATTOO_FM_UNLOCKS_0` to `_53` of the chosen character to -1 (every bit on). Like the other options, it asks first (**Ask before applying**) and follows **Save after write** and **Verify read-back**. Change session to see the tattoos in the shop.

### Things to know
- Found with **Tattoo scan** on a real account (1.57, firmware 14.02): the game has the 54 stats `MPx_TATTOO_FM_UNLOCKS_0`-`_53` and `MPx_TATTOO_FM_CURRENT_0`-`_53`. A character with every tattoo unlocked had all 54 unlock stats at `0xFFFFFFFF`; another one had only a few bits on in `_0`-`_2`.
- The shop has 676 male tattoos: 535 have no lock and 141 have one of 7 locks. Some DLC tattoos may need other stats. Check the shop after unlocking, and scan again.
- The tattoo names are still not read: at the address of `GET_FILENAME_FOR_AUDIO_CONVERSATION` in the natives table (`0xDDCEE0`) the game has `4E 1D BB EB 30 E8 F6 79...`, which is not the start of a function. The same happens with `GET_MOD_TEXT_LABEL` (`0xF22020`, livery names of the Vehicle tab). Those two table addresses are wrong for this game.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. New file: `Options/TattooUnlockOption.cs`. Files of JDM - RTM: `Ui/TattooPanel.cs`.

## 1.7.2 - JDM - RTM

### Added
- **Chameleon paints in Rim Color** (Vehicle and Garage Editor), like in the other colour categories. The chameleon warning of **Apply changes** also looks at the rim colour.

### Changed
- **Custom RGB of Respray Primary and Respray Secondary**:
  - Changing only the custom RGB (turning it on or off, or a new colour) now marks the category as changed, and the category shows "Custom RGB r,g,b". Before, only the paint number was looked at.
  - Turning the custom RGB on when it has no colour yet starts from the colour of the paint the car has, instead of black (0,0,0).
  - Garage Editor, **Respray Secondary**: the custom RGB is not shown any more, with a note in its place. Its cells in the saved car are not known yet, so it was never written ("Nothing to apply"). The Vehicle tab still sets it on the car out in the world.
- The window title says v1.7.2.

### Things to know
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (title), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs`.

## 1.7.1 - JDM - RTM

### Added
- **Pictures of the Benny's wheels** (Bennys Originals and Bennys Bespoke), cut from the Save Editor gallery like the others, in `Vehicle/wheels.zip`.

### Changed
- **Garages can be changed inside a building** (a garage, an office, an apartment...), as other tools do. Before, **Save to garage**, the plate in the garage, **Add vehicle**, adding to free spaces and **Undo last import** waited until you walked out onto the street. Now they are done at once. Inside, the report says how to see the change: leave and enter the garage again, or change floor. Do not move cars between spaces before that.
- **Automatic load with no questions**: **Read vehicle** only reads, so it no longer asks "Run 'Read vehicle'?". The Vehicle tab reads your car and the Garage Editor reads your garages by themselves when you open them, and also when the game connects while one of them is open. If the Names tab (or another operation) is working, they wait for it (up to 30 seconds) instead of being skipped.
- **Tattoo scan** no longer stops with "0xDDCEE0 does not start like a native handler". That address is `GET_FILENAME_FOR_AUDIO_CONVERSATION`, used only for the names of the tattoos. When it cannot be used, the catalogue is saved without names (the labels `TAT_...` are kept) and the log shows the first bytes found at that address, to find the right one.
- The window title says v1.7.1.

### Things to know
- On a real console the address of `GET_FILENAME_FOR_AUDIO_CONVERSATION` in the natives table did not start like a function, so the names of the liveries (Vehicle tab) may not be read either.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (automatic load, title), `Ui/IToolHost.cs` (`IsIdle`), the project file and `build/ai.cs` (version), `README.md` and this file. Files of JDM - RTM: `Ui/VehiclePanel.cs`, `Ui/GarageManager.cs`, `Options/TattooScan.cs`, `Vehicle/wheels.zip`, `docs/OFFSETS.md`.

## 1.7.0 - JDM - RTM

### Added
- **Tattoo Editor tab**: everything about tattoos is now in its own tab. It has **Tattoo scan** (moved from the Debug tab; it only reads) and **Unlock all tattoos**. Unlock all is off for now: the stats that hold the tattoos in 1.57 are not known yet. The tattoo scans are made to find them.
- **Benny's wheels** (Vehicle and Garage Editor, **Front Rims** and **Rear Rims**): **Bennys Originals** (wheel type 8) and **Bennys Bespoke** (wheel type 9), 31 wheels each. They have no picture yet (added in 1.7.1).
- **Max performance** (Vehicle and Garage Editor, next to the gallery title): chooses the last option of armor, suspension, brakes, transmission and engine, and turbo on.
- **Random mods** (same place): chooses a random option, or stock, for every mod the car has, except the wheels. Press it again for other options.
- Both buttons only choose options, like clicking tiles. Nothing is written until **Apply changes**, which asks first as always.

### Changed
- **Windows** (window tint): tint 0 is now called **Stock** and tint 4 is called **Clear**. The values written are the same. Importing a car file still reads the game's names (`None` = 0, `Stock` = 4), and `Clear` too.
- **Diamonds on the colours**: the built-in table now has all 161 colours of the "Color Shenanigans" sheet, the Metallic group too, for Primary, Secondary, Pearlescent, Rim, Interior and Dashboard. Not ticked in the sheet = diamond. Before, the Metallic colours had no data, so they had no diamonds.
- **Garage list**: the office garages (and the clubhouse, hangar, etc.) always show their own name. Before, the stat of the slot could hold an apartment (on a real account slot 8 = 43 and slot 9 = 64), and then **Office garage 1** and **Office garage 2** were shown with the name of an apartment. Only apartment garages take the name of their apartment now. The property id still shows under the name.
- The window title says v1.7.0.
- The project file references `System.Net.Http` and `System.IO.Compression`. Without them `dotnet build` on Windows stopped with errors in `Ui/GaragePreview.cs` (`HttpClient`) and `Vehicle/WheelPictures.cs` (`ZipArchive`).

### Things to know
- The Primary column of the sheet has no tick at all, so every Primary colour has a diamond. Before, a Primary column with no tick was taken as "unknown" and had no diamonds.
- Not tried on the console yet.

### Files of the Arise code that were touched
`MainForm.cs` (new tab, title), `Ui/DebugPanel.cs` (Tattoo scan moved out), the project file and `build/ai.cs` (version), `README.md` and this file. New file: `Ui/TattooPanel.cs`. Files of JDM - RTM: `Ui/VehiclePanel.cs`, `Vehicle/GarageCarFile.cs`, `Vehicle/GarageTables.cs`, `Vehicle/ColourAvailability.cs`, `docs/OFFSETS.md`.

## 1.6.1 - JDM - RTM

### Added
- **LSCM lvl 1000** (Main tab, **Stats** section): sets the LS Car Meet reputation of the character chosen in the toolbar to **level 1000**. The game has no level stat for the Car Meet: it works the level out from the reputation points, so the button writes `MPx_CAR_CLUB_REP = 999500`, the points needed for level 1000.
- **Arena lvl 1000** (Main tab, **Stats** section): sets the Arena War sponsorship tier of the chosen character to **1000**. It writes `MPx_ARENAWARS_AP = 5045000` (the Arena Points of tier 1000 in the GTA Wiki table), `MPx_ARENAWARS_AP_TIER = 1000` and `MPx_ARENAWARS_AP_LIFETIME = 5045000`.
- `docs/OFFSETS.md` and `docs/garage_map.csv`: every offset, global, cell, stat and native the Vehicle and Garage Editor tabs use, in one place.
- Both buttons work like **Unlock fast run**: they stay disabled until Character 1 or 2 is chosen, ask before writing (**Ask before applying**) and follow **Save after write** and **Verify read-back**. A stat the game does not have is skipped and named in the log. Change session to see the new level.

### Changed
- The **Stats** section has a new row with the two buttons; the note under them is shorter so everything fits.
- The project file leaves the `build` folder out: `build/ai.cs` repeats the assembly attributes that `dotnet build` already makes, so `dotnet build` (Visual Studio, VS Code) stopped with "duplicate attribute" errors. The Linux script `build/build.sh` still uses `build/ai.cs` as before.
- The window title says v1.6.1.

### Things to know
- Tried by jdm_drlft on firmware 14.02, GTA V 1.57 (EU), Character 1: the four stats exist in this build of the game and were written and saved with no errors (written 1 and written 3, skipped 0, failed 0). **Verify read-back** was off, so the values were not read back yet.
- Going from a low level to 1000 at once is easy to notice. Try it on a character you can afford to lose.
- `MPx_ARENAWARS_AP_LIFETIME` is set to 5,045,000 even if your character had more.

### Files of the Arise code that were touched
`Ui/StatsSection.cs`, `MainForm.cs` (the title), the project file (version and the `build` folder), `build/ai.cs` (version), `README.md` and this file. New files: `Options/ProgressionLevelOption.cs`, `docs/OFFSETS.md`, `docs/garage_map.csv`.

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
