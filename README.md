How to install Windows with boot.wim and install.wim

How to download Windows .iso file
===========================================================================

[This website](https://tb.rg-adguard.net/public.php), if working properly,
returns an **official** link to the Windows .iso file according to the
options choosed. If it does not return an official link (i.e. a link to
https://software-download.microsoft.com), please **DO NOT DOWNLOAD IT**.

For the latest stable build, it's possible to download directly from
Microsoft website by accessing one of the following links with a Linux
user agent (because Windows user agent did not open ISO download page in
my tests):

- Windows 10: <https://www.microsoft.com/en-us/software-download/windows10>
- Windows 11: <https://www.microsoft.com/en-us/software-download/windows11>

Also, check out <https://superuser.com/questions/1108085/where-can-i-get-a-clean-iso-of-a-specific-build-of-windows-10>.

How to set up the bootable media
===========================================================================

1. Install [Ventoy](https://github.com/ventoy/Ventoy) on your USB flash
   drive.
2. Install Ventoy's official [Wimboot Plugin](https://www.ventoy.net/en/plugin_wimboot.html).
3. Extract `boot.wim` and `install.wim` from sources directory on Windows
   image (.iso) and copy them to the flash drive.

How to install Windows
===========================================================================

1. Boot the flash drive with `boot.wim`.
2. On the installation start screen, open terminal with `Shift+F10`.
3. Format partition with diskpart:
     ```
     diskpart
     ```
   Example commands on diskpart's CLI:
     ```
     select disk N
     select partition N
     format quick
     ```
   For creating partitions, see [this guide](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/oem-deployment-of-windows-desktop-editions-sample-scripts?preserve-view=true&view=windows-10#-createpartitions-uefitxt).
4. Apply `install.wim` image to C:\
     ```
     dism /Apply-Image /ImageFile:D:\install.wim /Index:6 /ApplyDir:C:\
     ```
     Index 6 corresponds to the Windows Professional edition.
5. Setup EFI partition with the files just installed in `C:\Windows`
     ```
     bcdboot C:\Windows
     ```
     The EFI partition is detected automatically. For explicitly setting
     the EFI partition:

     1. Assign a letter to it with diskpart:
        ```
        select disk N
        select partition N
        assign letter="S"
        ```
        In the example above, "S" was chose by convention.
     2. Add the option: `/s S:` to `bcdboot`, where `S:` is the letter
        chosen for EFI partition plus a colon.

Activate Windows
===========================================================================

If you are a developer, take a look [here](https://github.com/pasqualirb/MicrosoftLovesDevelopers).

Otherwise, go to the following websites for buying your license:

- Windows 10: https://www.microsoft.com/en-us/windows/get-windows-10
- Windows 11: https://www.microsoft.com/en-us/windows/get-windows-11

Disable PortableOperatingSystem flag if running on an USB drive
===========================================================================

When Windows is installed on an USB drive, Windows Update does not install
updates and displays the following error message:

> "You can't install Windows on a USB flash Drive using Setup"

For fixing this issue, disable the flag PortableOperatingSystem on Windows
Registry by running the following command as administrador:

```
reg add HKLM\SYSTEM\CurrentControlSet\Control /v PortableOperatingSystem /t REG_DWORD /d 0
```
