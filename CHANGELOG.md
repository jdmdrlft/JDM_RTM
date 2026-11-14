# Changelog

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
