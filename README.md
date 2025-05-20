# MacAddressChangerPython 🐍

A Python-based command-line utility for viewing and modifying network adapter MAC addresses on Windows systems. This script is inspired by the functionality and user experience of Scrut1ny's "Windows-MAC-Address-Spoofer v9.0" batch file, re-implemented in Python.

It provides an interactive menu to select network adapters and perform actions such as randomizing, customizing, or reverting MAC addresses.

## Features ✨

* **Administrator Privileges Check**: Automatically checks for and requests administrator rights if not present.
* **List Network Adapters**: Enumerates available Network Interface Controllers (NICs) for selection.
* **Interactive Menu**: User-friendly console menu to choose adapters and actions.
* **Random MAC Spoofing**: Generate and apply a new random MAC address to the selected adapter.
    * Ensures the second character of the first octet is one of `A`, `E`, `2`, or `6` for better compatibility with some wireless NICs.
* **Custom MAC Spoofing**: Set a specific, user-defined MAC address.
    * Validates input for correct length (12 hex characters) and valid hexadecimal characters.
* **Revert to Original MAC**: Remove the spoofed MAC address from the registry, allowing the adapter to revert to its factory default MAC.
* **Revise Networking**: Option to release/renew IP configurations and clear the ARP cache (`ipconfig /release`, `arp -d *`, `ipconfig /renew`).
* **Colorized Output**: Uses ANSI escape codes for a more visually organized console interface.
* **View Current MAC**: Displays the current MAC address of the selected adapter before modification.

## Requirements ⚙️

* **Operating System**: Windows (due to reliance on `wmic`, `netsh`, `reg`, and Windows-specific registry paths).
* **Python**: Python 3.x.
* **Permissions**: Administrator privileges are required to modify network settings and registry entries. The script will attempt to request these if not run as admin.
* **Console**: A console that supports ANSI escape codes for colors (e.g., Windows Terminal, modern `cmd.exe`, PowerShell).

## How to Use 🚀

1.  **Download**: Save the script as a `.py` file (e.g., `mac_changer.py`).
2.  **Run the Script**: Execute the script from a command prompt or PowerShell.
    ```bash
    python mac_changer.py
    ```
    If not run with administrator privileges, the script will attempt to re-launch itself with elevated rights (a UAC prompt may appear).

3.  **NIC Selection Menu**:
    * The script will display a list of available network adapters with a corresponding number.
    * Enter the number of the NIC you wish to modify.
    * Enter `99` to "Revise Networking" (release/renew IP, clear ARP).

4.  **Action Menu**:
    * Once a NIC is selected, an action menu will appear:
        * `1`: Randomize MAC address.
        * `2`: Customize MAC address (you will be prompted to enter one).
        * `3`: Revert MAC address to original.
        * `0`: Go back to the NIC selection menu.
    * Enter your desired action number.

5.  **Follow Prompts**:
    * For custom MAC, enter the 12-character hexadecimal MAC address (e.g., `AABBCCDDEEFF`).
    * The script will show the previous and modified/reverted MAC addresses and confirm actions.

6.  **Exit**: To exit, you can typically close the console window or use `Ctrl+C` (though navigating back to the main menu and closing is cleaner).

## How It Works (Briefly) 🛠️

The script performs MAC address changes by:

1.  **Identifying Adapter Index**: Uses `wmic` to get the system index of the selected network adapter (`NetConnectionID`). This index corresponds to a subkey in the Windows Registry.
2.  **Registry Modification**:
    * To **set** a MAC address (random or custom), it writes the new MAC (as a 12-character string without colons) to the `NetworkAddress` value within the adapter's registry key:
        `HKLM\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}\XXXX` (where `XXXX` is the zero-padded adapter index).
    * To **revert**, it deletes the `NetworkAddress` value from the same registry location.
3.  **Adapter Reset**: Uses `netsh interface set interface ... admin=disable` followed by `admin=enable` to disable and then re-enable the network adapter. This is necessary for the registry changes to take effect.

## Important Notes & Disclaimer ⚠️

* **Administrator Privileges**: This script modifies system settings and **must** be run with administrator privileges.
* **Use Responsibly**: MAC address spoofing can be used for legitimate purposes (e.g., privacy, bypassing certain network restrictions, testing). However, it can also be misused. Use this tool responsibly and ethically, and ensure you are complying with all applicable laws and network usage policies.
* **Network Disruption**: The process of disabling and enabling network adapters will temporarily disconnect your network connection for that adapter.
* **Compatibility**: While designed to be broadly compatible, network adapter drivers and Windows versions can sometimes behave unexpectedly. The "revert" option is there to help restore original settings.
* **No Guarantees**: This script is provided "as-is" without any warranties. The authors or contributors are not responsible for any damage or issues that may arise from its use. Always understand what a script does before running it, especially one that modifies system settings.
* **Persistence**: Changes made to the `NetworkAddress` registry value are generally persistent across reboots.

## Acknowledgements 🙏

* This script's functionality and user interface are heavily inspired by the "[Windows-MAC-Address-Spoofer v9.0](https://github.com/Scrut1ny/Windows-MAC-Address-Spoofer)" batch script developed by **Scrut1ny** and its contributors. This Python version aims to replicate that utility's capabilities in a different scripting language.
