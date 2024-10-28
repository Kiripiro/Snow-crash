# SnowCrash Level00 Guide
This guide will walk you through the process of finding the password for the `flag00` user.

## Procedure

0. **Watch the video on the Intra**
    - You can see that the word "find" is in uppercase. We might have to use this command...

1. **Examine your environment using Unix commands.**
   
    - `id` : Shows user and group identities. 
    - `pwd` : Displays the current directory you're in. 
    - `ls -la` : Lists files and their permissions in the current directory.
    - Output: Nothing really interesting...
    
2. **Use 'find' to search for user-specific files.**

    - `find / -user level00` and `find / -user flag00 2>/dev/null`: 
      These commands search the filesystem for any files belonging to `level00` and `flag00` respectively. The `2>/dev/null` part suppresses error messages. 
    - Output: Using the first command, nothing looks suspicous, whereas when we run the second command:  
    ```shell
        ...
        /proc/2465/coredump_filter
        /proc/2465/io
        /usr/sbin/john
        /rofs/usr/sbin/john

    ```
    - John ? Does this mean something ?..
   

3. **Inspect and decipher them.**

    - Run `cat /usr/sbin/john` and `cat /rofs/usr/sbin/john`. 
    - Collect the output: 

        ```shell
        cdiiddwpgswtgt
        ```

4. **Decrypt the output.**

    - Paste `cdiiddwpgswtgt` in your browser. A Caesar cipher decoder link from `dcode.fr` will appear.
    - Use the decoder and get `nottoohardhere`. This Caesar cipher, also known as a shift cipher, works by shifting the letters in the alphabet by a certain number of places.

5. **Access `flag00`.**

    - Use `su flag00` and input `nottoohardhere` when asked for password.
    - You can now access the `flag00` account! (Using `getflag` command)

## Useful link

    - https://www.dcode.fr/