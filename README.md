# Linux CPU scaling helper

This tool lets you switch performance preferences for a CPU on Linux with cpufreq driver
(no `cpufreq-utils` needed). Features:

-  Get current modes for CPU cores
-  Set new mode for all cores
-  Enable/disable "extreme powersave mode" (that is, force the lowest possible frequency for all cores)

## Usage, simplest case

The simplest way to use this script is just download it and run as root (see [#Examples](examples)).
That's it!
:TODO: HOORAY:

## Usage as non-root

To make the usage more convenient, one may want to give some non-root users permissions to run this script.
For that, they should set the appropriate permissions on the virtual system files, which control the CPU scaling.

The `update_sysfs_cpu_permissions.service` systemd service file will take care of the permissions for you.
To set it up, run (as root):

1.  Create a group for users allowed to run the script: `groupadd cpu_tuners`
2.  Add user(s) to the group: `usermod -aG cpu_tuners $USER`
3.  Create a systemd service to give permissions:
    -  Put the file `cp update_sysfs_cpu_permissions.service /etc/systemd/system/`
    -  Run `systemctl enable --now update_sysfs_cpu_permissions.service` to set permissions this time, as well
       as on every boot.

## Usage together with udev

It is also possible to enable udev to trigger actions when laptop charging is connected/disconnected. For that:

1.  Put the script file on your root partition (otherwise the udev rule may not work on boot, as other partitions
    may not be mounted yet). For example, put it to `/usr/local/bin/`.
2.  Put your path to the `99-cpu_energy_preference_on_charger.rules` file (the default script path is
    `/usr/local/bin/cpu`).
3.  Put the rules file to the `/etc/udev/rules.d/` directory.

## Examples

Manual running examples:
```
./cpu.sh get          # View current preferences
./cpu.sh list         # List valid options for `set` (for the first core)
./cpu.sh set default  # Set a mode for all cores
./cpu.sh extreme      # Enter extreme powersave mode (force lowest possible frequency)
./cpu.sh unextreme    # Exit extreme powersave mode
```
