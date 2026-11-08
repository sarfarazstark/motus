# Installing Motus

Motus is a Windows desktop app for running a gym. This guide covers installing it, getting
past the warnings an unsigned app always triggers, and checking that the file you
downloaded is the one published here.

Installing Motus means accepting its licence: proprietary, licensed rather than
sold, for use in your own business. The full terms are in [LICENSE](LICENSE), and the
Windows installer shows them before it installs anything.

## Before you start

Grab the installer from the [Releases](../../releases) page. Each release lists its files
and a `SHA256SUMS.txt`.

**System requirements**

| | |
|---|---|
| Windows | Windows 10 or 11, 64-bit |
| Disk | ~600 MB after install |
| Other | Nothing else — no runtime to install, no account to create |

## Verify the download first

An unsigned app asks you to bypass a safety warning, which is also what malware does. The
checksum is how you tell the difference — **check it before you unblock anything**.

**Windows (PowerShell)**

```powershell
Get-FileHash .\motus-setup.exe -Algorithm SHA256
```

**macOS**

```bash
shasum -a 256 Motus-<version>.dmg
```

**Linux**

```bash
sha256sum -c SHA256SUMS.txt      # in the folder you downloaded into
```

Compare the output with the matching line in `SHA256SUMS.txt` on the release page. If it
differs, delete the file and download it again — do not install it.

## Windows

1. Verify the checksum (above).
2. Run `motus-setup.exe`.

   If Windows shows **"Windows protected your PC"**, click **More info → Run anyway**.
   This appears because the installer is not code-signed, not because anything is wrong
   with it.

   You can avoid the dialog entirely by removing the "downloaded from the internet" mark
   first:

   ```powershell
   Unblock-File .\motus-setup.exe
   ```

3. The installer offers a per-user install (no administrator rights needed) and a desktop
   shortcut. Start Motus from there.
4. On first launch, set up your gym and an admin PIN. That PIN is what protects the
   database, so keep it somewhere safe.

**Updates** are built in: Motus checks a moment after launch and tells you when a new
version exists. Nothing installs until you press **Restart and install**.

## macOS

The current builds are **Apple Silicon only** (arm64). Intel Macs are not supported yet.

1. Open the `.dmg` and drag **Motus** to **Applications**.
2. macOS will refuse to open it the first time — the app is not signed or notarised.
   Clear the quarantine flag once:

   ```bash
   xattr -dr com.apple.quarantine /Applications/Motus.app
   ```

3. Open Motus from Applications.

**Automatic updates do not work on macOS while the app is unsigned** — macOS refuses to
apply an unsigned update. Re-download a new version from Releases and repeat step 2.

## Linux

**AppImage (any distribution)**

```bash
chmod +x motus-<version>.AppImage
./motus-<version>.AppImage
```

**Debian / Ubuntu**

```bash
sudo apt install ./motus_<version>_amd64.deb
```

Updates are checked in-app. What happens on install depends on the package:

- **AppImage** — updates silently, like Windows, provided the app runs from a writable
  location.
- **deb / rpm** — updates too, but the package manager needs administrator rights, so
  **your system password prompt will appear** (and it can show up behind this window).
  That prompt cannot be suppressed by the app.

## Troubleshooting

| What you see | What it means |
|---|---|
| "Windows protected your PC" | Unsigned installer — **More info → Run anyway**, or `Unblock-File` first |
| "Motus is damaged" / "cannot be opened" on macOS | The quarantine flag — run the `xattr` command above |
| AppImage does nothing when double-clicked | Not executable — `chmod +x` it |
| A pop-up firewall prompt | Motus opens a local database; no inbound connection is needed. Denying is safe |
| Update offered but nothing happens | macOS only, and expected while unsigned: re-download instead |

## Uninstalling

| Platform | How |
|---|---|
| Windows | Settings → Apps → **Motus Gym Management** → Uninstall |
| macOS | Move Motus from Applications to the Bin |
| Linux | `sudo apt remove motus`, or delete the AppImage |

Your data lives in the app's user folder (`%APPDATA%\Motus` on Windows) and is **not**
deleted by an uninstall — remove that folder too if you want a clean slate. Take a backup
from inside the app first if you may want the data again.
