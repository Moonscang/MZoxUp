# MZoxUp README  
**Read this in other languages: [English](README.md), [中文](README_zh.md).**

## Important things bear repeating: the project has NOT been uploaded yet, and it still hasn’t, and once more—it hasn’t!  
## Tentative plan: the first usable Beta will appear around 2028-2029(although we may ship an x86_64-only preview first). Stay tuned!  
MZoxUp is a dual-mode (UEFI & Legacy BIOS) boot loader targeting x86-64, ARM64, RISC-V64 and LoongArch64.  
It offers a three-tier privilege scheme—King / root / admin—and a secure-mode rescue terminal named `MZUsh#king->`.  
Together with btrfs snapshots, it enables **second-level** system rollbacks.

---

1. Core Features

| Feature        | Description |
|----------------|-------------|
| Dual Boot      | One source tree builds both `MZoxUp.efi` (UEFI) and `MZoxUp.mbr` (Legacy BIOS). |
| Three Privilege Levels | King (firmware), root (read-only system + writable config), admin (regular user). |
| Safe Mode      | Press **F8** or trigger on boot failure; offers GUI rescue and `MZUsh#king->` shell. |
| btrfs Snapshots| One-click rollback, transactional updates, crash-second recovery. |
| One-Time Token | External `AllowWrite.token` unlocks write access to the system volume. |

---

2. Quick Start (x86-64 QEMU)

```bash
git clone --recurse-submodules https://gitee.com/mirrors/edk2.git
cd MZoxUp
make ARCH=x86_64 MODE=uefi   # produces MZoxUp.efi
./scripts/qemu-run.sh        # test immediately
```

---

3. Directory Layout

```
MZoxUp/
├── cfg/          # Boot-menu configs
├── docs/         # Docs & screenshots
├── scripts/      # Build, flash, QEMU helpers
└── src/
    ├── common/   # Shared algorithms & utils
    ├── arch/     # arch-specific (x86_64, arm64 …)
    ├── fs/       # FAT32 / btrfs parsers
    ├── gui/      # Safe-mode GUI
    ├── shell/    # MZUsh#king-> terminal
    └── token/    # AllowWrite.token gen/verify
```

---

4. Privilege Model

| Identity | Active in | System Volume Writable | Config / Home Writable |
|----------|-----------|------------------------|------------------------|
| King     | Safe mode | ✅ any sub-volume      | ✅ all                 |
| root     | Normal OS | ❌ read-only           | ✅ `/etc /var /opt /home` |
| admin    | Normal OS | ❌ read-only           | ✅ own home only       |

---

5. Flashing  
Project not yet uploaded—please wait!

6. Development & Contributing  
- Use branch `dev/<arch>`; ensure `make lint` passes before PR.  
- Check milestones in `docs/milestones.md`.

---

7. License  
MIT © 2025 Moonscang | ZexoMinx
