# Linux CPU scaling helper

This tool lets you switch performance preferences on Linux for CPUs with cpufreq driver
(no need for `cpufreq-utils`). Features:

-   Get current modes for CPU cores
-   Set new mode for all cores
-   Enable/disable "extreme powersave mode" (that is, force the lowest possible frequency for all cores)

## Usage, simplest case

The simplest way to use this script is just download it and run as root (see [examples](#Examples)).

```
wget https://raw.githubusercontent.com/kolayne/linux-cpu-scaling-helper/master/cpu.sh
chmod +x cpu.sh
sudo cpu.sh get
```

That's it! :tada:

## Run as non-root

To let (some) users other than root run the script, perform the following initial setup (as root):

1.  Create a group for users allowed to run the script: `groupadd cpu_tuners`

2.  Add user(s) to the group: `usermod -aG cpu_tuners $USER`

3.  Create a systemd service to set appropriate permissions on system files:
    -   Copy the service file: `cp update_sysfs_cpu_permissions.service /etc/systemd/system/`
    
    -   Run `systemctl enable --now update_sysfs_cpu_permissions.service` to set permissions this time, and
       enable the service to run on every boot (needed because permissions of virtual files do not survive
       shutdown/reboot).

## Run automatically on charger connect/disconnect events

It might also be convenient to run the script on laptop charger connect/disconnect events (via udev).
For that:

1.  If you have several partitions, put the script on the root partition for it to run
    when udev discovers the charging hardware. For example, `cp ./cpu.sh /usr/local/bin/cpu`.

2.  Put the path to your script to the `99-cpu_energy_preference_on_charger.rules` file.

3.  Put the rules file to the rules directory:
    `cp 99-cpu_energy_preference_on_charger.rules /etc/udev/rules.d/`

## Examples

Manual running examples:
```bash
./cpu.sh get          # View current preferences
./cpu.sh list         # List valid options for `set` (for the first core)
./cpu.sh set default  # Set a mode for all cores
./cpu.sh extreme      # Enter extreme powersave mode (force lowest possible frequency)
./cpu.sh unextreme    # Exit extreme powersave mode
```
