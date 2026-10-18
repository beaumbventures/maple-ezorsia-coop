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
- GitHub Actions workflow builds `dinput8.dll` (Release x86) on every push to main; when `RELEASE_TAG` names a new tag, that build is published as a release.

## Build 2 (coop-2): the actual fix
- `CWvsApp::CWvsApp` rewrite: `TSingleton<CWvsApp>::ms_pInstance` (0xBE7B38) was set only if `m_hWnd` was non-null,
  but `m_hWnd` is uninitialized at that point. With zeroed memory the singleton stayed NULL, the game's window procedure
  returned 0 for every message (including `WM_NCCREATE`), and `CreateWindowExA` failed with `GetLastError` = 0, which
  upstream then "fixed" by relaunching forever. Now it always stores `this`. Found from build 1's `Ezorsia.log`.
