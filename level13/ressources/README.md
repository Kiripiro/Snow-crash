# SnowCrash Level13 Guide
This guide will walk you through the process of finding the password for the `flag13` user.

## Procedure:

1. **Examine your environment**

`ls -la` : Lists files and their permissions in the current directory.
```bash
-rwsr-sr-x 1 flag13  level13 7303 Aug 30  2015 level13
```
We have a binary file, `level13`.

2. **Analyze level13**

We execute the binary.
```bash
./level13
```
Returns
```bash
UID 2013 started us but we we expect 4242
```
We can see that the binary is checking the UID of the user running it. Our UID is 2013, but the binary expects 4242.
We can use `ltrace` to see what the binary is doing.
```bash
ltrace ./level13
```
We can see that the binary is calling `getuid()` to get the UID of the user running it.
Now, we can change our UID to 4242, or we can make believe that our program that our UID is 4242.

3. **Create a C file in the `tmp` folder**

Add the following code within the file `mygetuid.c`:
```c
int getuid() {
	unsetenv("LD_PRELOAD");
   return 4242;
}
```
We are creating a shared library so we can use it anywhere:
```bash
gcc -fPIC -shared -o mygetuid.so mygetuid.c  -nostartfiles
```
Export it to the LD_PRELOAD variable:
```bash
export LD_PRELOAD=/tmp/mygetuid.so
```
If you set LD_PRELOAD to the path of a shared object, that file will be loaded before any other library (including the C runtime, libc.so). 

Now go back to the home
```bash
cd
```
And execute the `level13` file using `gdb`:
```bash
gdb ./level13
```
Add the LD_PRELOAD as our generated file in the environment:
```gdb
set environment LD_PRELOAD /tmp/mygetuid.so
```
Run within `gdb`:
```bash
run
```
Returns:
```bash
Starting program: /home/user/level13/level13 
your token is 2A31L79asukciNyi8uppkEuSx
```

Go the the next user.