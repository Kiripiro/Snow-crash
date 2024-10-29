# SnowCrash Level09 Guide
This guide will walk you through the process of finding the password for the `flag09` user.

## Procedure:

1. **Examine your environment**
   
`ls -la` : Lists files and their permissions in the current directory.
```bash
-rwsr-sr-x 1 flag09 level09 7640 Mar  5  2016 level09
----r--r-- 1 flag09 level09   26 Mar  5  2016 token
```

2. **Analyze token**

We examine the file token.
```bash
cat token
```
Returns
```bash
f4kmm6p|=�p�n��DB�Du{��
```
We try to access at the flag09 user.
```bash
su flag09
Password:
```
Returns
```bash
su: Authentication failure
```
The token seems to be encrypted.

3. **Analyze level09**

We execute the file level09.
```bash
./level09
```
Returns
```bash
You need to provied only one arg.
```
We try to execute the file with different arguments.
```bash
./level09 a
a
./level09 b
b
./level09 abc
ace
./level09 test
tfuw
```
With the token:
```bash
./level09 'f4kmm6p|=pnDBDu{'
f5mpq;vEyxONQ
```
We try to access at the flag09 user with this token.
```bash
su flag09
Password:
```
Returns
```bash
su: Authentication failure
```
The token seems to be encoded by ./level09.
We are going to decode it.

4. **Understand the algorithm used to encrypt the token**

After some tests, we find that the token is encrypted with the following algorithm:
Each character of the argument is incremented by the position of the character in the argument.
```bash
./level09 abcdef
acegik
```
```
a -> a + 0 = a
b -> b + 1 = c
c -> c + 2 = e
d -> d + 3 = g
e -> e + 4 = i
f -> f + 5 = k
```

5. **Create a C script to decrypt the token**

We are going to write a C script to decrypt the token.
We don't have the right to write in the current directory, so we are going to write the script in `/tmp`.
```c
#include <string.h>
#include <stdio.h>

int main(int argc, char *argv[])
{
  if (argc != 2) {
    return 1;
  }

  for (int i = 0; i < strlen(argv[1]); ++i) {
    argv[1][i] -= i;
  }

  printf("%s\n", argv[1]);
}
```

6. **Decrypt the token**

Make sure you are in the `/tmp` folder.
Compile the program using the `-std=c99` in order to use the `for`.
```bash
gcc test.c -std=c99 && ./a.out $(cat $HOME/token )
```
Returns
```bash
f3iji1ju5yuevaus41q1afiuq
```
We try to acces at the flag09 user with this token.
```bash
su flag09
Password:
```
Returns
```bash
Don't forget to launch getflag !
```
We execute the getflag command and log to the next user.