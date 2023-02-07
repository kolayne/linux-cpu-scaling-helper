# Linux CPU scaling helper

This tool lets you switch performance preferences on Linux for CPUs with cpufreq driver
(no need for `cpufreq-utils`). Features:

-  Get current modes for CPU cores
-  Set new mode for all cores
-  Enable/disable "extreme powersave mode" (that is, force the lowest possible frequency for all cores)

## Usage, simplest case

The simplest way to use this script is just download it and run as root (see [examples](#Examples)).
That's it! :tada:

## Usage as non-root

To let (some) users other than root run the script, the following setup (performed by root) is needed:

1.  Create a group for users allowed to run the script: `groupadd cpu_tuners`
2.  Add user(s) to the group: `usermod -aG cpu_tuners $USER`
3.  Create a systemd service to set appropriate permissions on system files:
    -  Copy the service file: `cp update_sysfs_cpu_permissions.service /etc/systemd/system/`
    -  Run `systemctl enable --now update_sysfs_cpu_permissions.service` to set permissions this time, and
       enable the service to run on every boot.

## Usage together with udev

It is also possible to make laptop charger connection/disconnection to trigger the script run via udev.
For that:

1.  Put the script file on your root partition (otherwise the udev rule may not work on boot, as other partitions
    may not be mounted yet). For example, `cp ./cpu.sh /usr/local/bin/cpu`.
2.  If using a different path, update the `99-cpu_energy_preference_on_charger.rules` file accordingly.
3.  Put the rules file to the rules directory:
    `cp 99-cpu_energy_preference_on_charger.rules /etc/udev/rules.d/`

## Examples

Manual running examples:
```
./cpu.sh get          # View current preferences
./cpu.sh list         # List valid options for `set` (for the first core)
./cpu.sh set default  # Set a mode for all cores
./cpu.sh extreme      # Enter extreme powersave mode (force lowest possible frequency)
./cpu.sh unextreme    # Exit extreme powersave mode
```
