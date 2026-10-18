# maple-v83-coop changes to MapleEzorsia v2

Fork of [444Ro666/MapleEzorsia-v2](https://github.com/444Ro666/MapleEzorsia-v2) (AGPL-3.0, same license here), used with the
HeavenMS v83 localhost from Cosmic-client for a private co-op server.

## Why
On the HeavenMS localhost, v2's rewritten `CWvsApp::SetUp` failed to create the game window and then relaunched the client
with no limit (`fixWnd()` + `ExitProcess`), so the game never appeared and Task Manager showed a process that couldn't be ended.

## Changes (build 1: diagnostics)
- All of v2's console output goes to `Ezorsia.log` in the game folder (unbuffered).
- `RegisterClassExA` and `CreateWindowExA` calls are logged with their arguments, result and `GetLastError`.
- A failed window no longer relaunches the game: it logs the error, shows a message box once and exits.
- Progress markers in the rewritten `SetUp`.
- GitHub Actions workflow builds `dinput8.dll` (Release x86) on every push; tags publish a release.
