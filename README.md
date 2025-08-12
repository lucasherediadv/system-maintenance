# Arch Linux System Maintenance Cheat Sheet

Regular system maintenance is crucial for ensuring the stability, performance, and security of Arch Linux over a period of time. These are the steps I follow periodically to keep my system in optimal conditions:

## Systemd

List systemd units.
```
systemctl
systemctl list-units
```

List masked systemd units.
```
systemctl list-unit-files
```

Show locations and content of configuration files.
```
systemctl cat <service>
```

Check unit status
```
systemctl status <unit>
```

Check for any services that have failed to start or are in a failed state.
```
systemctl --failed
```

Startup time
```
systemd-analyze
systemd-analyze blame
```

## Logging

Systemd has its own logging system called the journal.
```
journalctl
```

Examine the journal for messages from the current boot.
```
journalctl --boot
```

Filter the journal to display only error-level messages
```
journalctl --boot --priority err # 0, 1, 2, 3 ...
```

Last lines for a specific unit
```
journalctl --lines 20 --no-pager --unit <service>
journalctl --since yesterday --until "1 hour ago"
```

Kernel messages
```
journalctl --dmesg
```

> Additionally, manually inspect log files located in /var/log/ for any irregularities or error messages.

## Pacman

Always use -Syu for updating the system
```
pacman -Syu
```

Search installed packages
```
pacman -Qs <name>
```

Identify packages that were installed as dependencies but are no longer required by any explicitly installed package (Orphans).
```
pacman -Qtd
```

Recursively remove orphans and their configuration files.
```
pacman -Qtd | pacman -Rns -
```

List packages that are not from the official Arch Linux repositories.
```
pacman -Qm
```

Find the package which installed file
```
pacman -Qo <file>
```

Show all files provided by local package
```
pacman -Ql <package>
```

Remove all cached packages that are not currently installed on you system and clean up the unused sync databases
```
pacman -Sc
```

Delete all cached versions of installed and uninstalled packages, keeping only the most recent three.
```
paccache -r
```

Dealing with new configuration files.
```
find /etc -name '*.pacnew' -o -name '*.pacsave'
```

Also you can use the pacdiff utility (part of the pacman-contrib package).
```
pacdiff
pacdiff --output
```

## Mirrors

A well-sorted mirrorlist ensures faster and more reliable downloads during updates.
```
reflector --latest 5 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
