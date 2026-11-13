# Changelog

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
`MainForm.cs` (a few lines: the Names tab, the banner, the title), `Ui/Theme.cs` (colours), `Ui/SlotList.cs` and `Ui/DebugPanel.cs` (one colour each), the project file (name and version), `README.md` and `CREDITS.txt`.
