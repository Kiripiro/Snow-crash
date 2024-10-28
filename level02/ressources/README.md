# SnowCrash Level02 Guide
This guide will walk you through the process of finding the password for the `flag03` user.

## Procedure

0. **Login to the `level02` user**
    - Run the following command:
    ```shell
    su level02
    # enter previous flag
    ``` 

1. **Examine your environment using Unix commands.**
   
    - `id` : Shows user and group identities. 
    - `pwd` : Displays the current directory you're in. 
    - `ls -la` : Lists files and their permissions in the current directory.
    - Output:
        ```shell
        ...
        ----r--r-- 1 flag02  level02 8302 Aug 30  2015 level02.pcap
        ...
        ```
    
2. **Copy pcap file to your local machine via scp.**
    
   ```bash
   scp -P <port> level02@<dist-machine-ip>:~/level02.pcap ~/. 
   ```
   This command copies the level02.pcap file from the remote machine to your personal directory on your local machine via scp (secure copy).

4. **Install the reauired Python library**
    - You can create a VENV if needed (RTFM) or run: 
    ```bash
    pip install scapy
    ```

5. **Use a Python script with Scapy library to analyze pcap file and extract the useful data (user inputs).**
   
   - Run the following script:
   ```python
    from scapy.all import *
   
    def analyze_pcap(file_name):
        packets = rdpcap(file_name)
        for i, packet in enumerate(packets):
            print(i, packet.sprintf("{Raw:%Raw.load%\n}"))

    analyze_pcap('level02.pcap')
    ```
    - This script displays the raw payload of each packet, which is what was actually transmitted over the network. This could include the password you're looking for.

6. **The password could be hidden among the characters displayed by the script. For instance, '\x7f' represents the 'Delete' key.**

7. **Decipher the password from the output data.**

    We get [packets](script-output.txt) containing the following characters: f t _ w a n d r '\x7f' '\x7f' '\x7f' N D R e l '\x7f' L 0 L

    If we remove the deleted characters and spaces, we get the following:
    `ft_waNDReL0L`