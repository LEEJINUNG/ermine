<p align="center">
  <img src="/img/ermine_banner.png" alt="Ermine Banner" width="100%"/>
</p>

<h1 align="center">Ermine</h1>

<p align="center">
  <b>Anti-Forensics Research Tool for DFIR Training</b>
</p>

---

> **Wait!** Are you afraid of your traces being discovered?
>
> If you use the cute little **Ermine**, it will snatch away your program logs before they pile up on the computer and run off with them.
>
> Feed your traces to Ermine. It's hungry.

---

## Features

- **Anti-Forensics Mode** - Restrict permissions on event logs, forensic artifacts, and registry keys
- **Secure File Erasure** - 3-pass overwrite (0x00 → 0xFF → random) with filename randomization on each pass
- **Clean Erasure** - Timestamp wipe + secure erase + NTFS journal flooding
- **Journal Flooding** - Pollute NTFS `$UsnJrnl` with massive dummy file create/delete operations
- **Command Execution** - Run arbitrary commands as part of the anti-forensics pipeline
- **One-Click Restore** - Revert all permission changes back to their original state

## Usage

> **Requires Administrator Privileges.** Right-click → Run as administrator.

### Enable Anti-Forensics Mode

```
ermine.exe disabled
```

Restricts file/folder permissions to read-only, locks registry keys, and executes predefined anti-forensics commands.

### Restore Normal State

```
ermine.exe enabled
```

Reverts all permission changes made by `disabled`.

### Secure File Erasure

```
ermine.exe eraser <filepath[;filepath2;...]>
```

Each file goes through:
1. Rename to random name → overwrite with `0x00`
2. Rename to random name → overwrite with `0xFF`
3. Rename to random name → overwrite with random data → delete

```
ermine.exe eraser C:\temp\secret.txt;C:\temp\log.txt
```

### Clean Erasure (Erase + Journal Flood)

```
ermine.exe cleaneraser <filepath[;filepath2;...]> <dirpath> <count>
```

Performs secure file erasure, then floods the NTFS journal with dummy operations to cover traces.

```
ermine.exe cleaneraser C:\temp\secret.txt C:\Windows\Temp 500000
```

### Journal Flooding

```
ermine.exe floodjournal <dirpath> <count>
```

Creates and deletes random temporary files to pollute NTFS `$UsnJrnl` entries, obstructing timeline analysis.

```
ermine.exe floodjournal C:\Windows\Temp 500000
ermine.exe floodjournal %Temp% 100000
```

### Help

```
ermine.exe /help
```

## Architecture

```
ermine.exe
├── AntiForensics       - Target list & permission control orchestrator
├── FilePermission      - NTFS ACL query & display
├── FilePermissionModifier - Set read-only / restore permissions
├── RegistryPermission  - Registry key ACL control
├── CommandExecutor     - Shell command execution module
├── FileEraser          - 3-pass secure file deletion
├── CleanEraser         - Timestamp wipe + erase + journal flood
└── TransactionCleaner  - NTFS journal pollution engine
```

## Disclaimer

> **This tool is created strictly for educational and DFIR training purposes only.**
>
> Any misuse of this tool is solely the responsibility of the user. The author assumes no liability for any damage, legal consequences, or unauthorized use arising from the use of this software.

## Source Code Distribution

The source code of this project is available for distribution. If you need the full source code and additional ideas behind this project, please contact:

**dlwlsdnd951@naver.com**

Please include your **full name** and **affiliation** in your request. Upon verification, the complete codebase and supplementary materials will be provided.

---

<p align="center">
  <sub>Made with persistence by <a href="https://github.com/LEEJINUNG">LEEJINUNG</a></sub>
</p>
