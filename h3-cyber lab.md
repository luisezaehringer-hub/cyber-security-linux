# x) read/watch/listen and summarize:
Karvinen 2020: https://terokarvinen.com/2020/command-line-basics-revisited/

Karvinen 2022: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

Karvinen 2018: https://www.bionicmechanic.com/images/IoT%20Rapid%20Prototyping%20Laboratory%20Setup.pdf

## Summary Article 1: Command Line Basics Revisited

- The first article describes briefly the basics of commands in Linux. The following points were hereby the most important ones:
  1. Navigation & File Overview:
     - 'pwd' prints the working directory
     - 'ls' lists files in working directoryList files in working directory
    
  2. File Manipulation
     - pico and nano as the most important commands for text editing
     - new directory with NEWFOLDER
     - renaming with NEWNAME
     - removing an empty directory with EMPTYDIR
    
  3. SSH remote control
     - Open a remote command shell with xy@example.com
     - 'exit' for exiting back to own machine
     -  copy FOLDER to a folder in remote machine
    
  4. Help
     - $ man ls shows manual page
     - short builtin help with $ ls --help
       $ wget -h

  5. History and guessing
     - [tab] for typing text and entering filenames or for paths to avoid typos
     - {arrows} move in current command line and command history
     - history can be searched with ctrl-R
    
  6. Important directories
 
     
     <img width="748" height="224" alt="image" src="https://github.com/user-attachments/assets/2527801b-a2d3-4d95-9d4b-0f329c0850a7" />

     -> its hard to insert here own ideas, since the commands dont let creativity flow...


## Summary Article 2: Cracking Passwords with Hashcat
The article gently introduces to the process of cracking a password. The solution is already provided, so you can first experiment with the environment at your own pace and work through the individual steps. The first step is installing Hashcat, then creating a directory for our case. Step two is to identify the Hash Type, this can be done through a hash cat parameter within the command. The hash can now be cracked, based on the hash type. The table helps to understand the meaning of the result.

<img width="753" height="220" alt="image" src="https://github.com/user-attachments/assets/c6422e16-8bd4-4567-af93-f102f7be0ade" />

After that, Hashcat generates a report on that case, that shows the output. The password can now be seen. The key factor in the result is the RockYou dictionary, since it determines whether the password can be found or not. 


## Summary Article 3: IoT Rapid Prototyping Laboratory Setup
- The goal of this approach: Even technical beginners and students from other disciplines should be able to design and build their own Internet of Things (IoT) prototypes in just four days.
- The learning structure: The concept divides the learning process into manageable sections (embedded systems, communication with the cloud, and cloud deployment), so that students can work directly on their ideas without getting bogged down by complex server or wireless issues.
- Software Infrastructure:
A Python program running on the computer handles communication with the Internet.
A web server manages data monitoring and the connection to the web client.
The entire process is based on free and open-source software.
- My question: How do you make the transition from a prototype to a finished, standalone IoT device?


# a) See Laksu

My presentation is about Encryption and Key Management. My working title is the following: Cost of a lost key: Managing Regulatory & Financial Risks in Data Protection.

I especially like this topic, since it has some strong dimensions to my work and can be applied to a wide range of jobs. 

# b) Install Linux on a virtual machine. Use Debian 13-Trixie
I installed Linux through UCM. Debian 13-Trixie took some time to install it and I wasn't quite sure if it ran correctly in the beginning. Now it looks fine. The Installing process was kind of easy, even though I dont really have that much of experience in running that process. Lastly, I created a VM.

<img width="450" height="49" alt="image" src="https://github.com/user-attachments/assets/69557f7f-d54b-426c-a6e7-b49b20f66cd8" />


<img width="882" height="590" alt="image" src="https://github.com/user-attachments/assets/1823a6d0-338d-41bd-a5e6-785438ac6008" />

further information can be found in my h2-scene report: https://github.com/luisezaehringer-hub/cyber-security-linux/blob/main/h2-scene.md

# c) crack
This was quite challenging, since my terminal didnt accept some of the commands. With a bit of trying out and taking some time, I could get my terminal to running all the commands. The problems were mostly because of my operating system (Mac). 
### 1. Installing hash cat:
<img width="568" height="150" alt="image" src="https://github.com/user-attachments/assets/9d6072e2-0df1-4bbb-bc3a-b4580b9b6adc" />

In the screenshot, you can see the installation of the tools needed for password cracking via the Linux terminal (run as the root administrator).

### 2. getting the big dictionary (rockyou):
<img width="759" height="280" alt="image" src="https://github.com/user-attachments/assets/69dae871-a986-4716-a668-d398bc30b621" />

The download of the large password dictionary (rockyou) using wget completed without errors and in record time (approximately 51 MB in 4.7 seconds).

### 3. output result:
<img width="489" height="271" alt="image" src="https://github.com/user-attachments/assets/bb56618b-681b-40dc-a7c3-2ad355217326" />

The final steps in preparing the password dictionary (rockyou.txt) before the actual attack can begin.

explanation of the three commands and their output:
tar xf rockyou.txt.tar.gz: This command extracts the downloaded, compressed archive (.tar.gz), creating the actual text file rockyou.txt—containing millions of passwords—on the hard drive.

rm rockyou.txt.tar.gz: “rm” stands for “remove.” Since the compressed archive has now been successfully extracted, the .tar.gz file is deleted to save disk space.

head rockyou.txt: By default, the head command displays the first 10 lines of a text file.

### 4. Identfying the Hashcode:
<img width="648" height="378" alt="image" src="https://github.com/user-attachments/assets/8968dc64-e29b-4318-8c53-03fb84dc76e5" />

Identifying the hash type using the hashid tool: 
hashid: the command-line tool that analyzes the length and structure of an unknown checksum to identify possible mathematical algorithms.
m: directly displays the appropriate Hashcat modes that will be needed later in Hashcat for the respective algorithm
-> The tool lists various mathematical algorithms that this hash format might correspond to

### 5. result of password:
<img width="733" height="65" alt="image" src="https://github.com/user-attachments/assets/245774fb-66ad-4c4d-9d57-3b8caf1e9181" />

The desired password was found!

### 6. what did Hashcat say?:
<img width="728" height="195" alt="image" src="https://github.com/user-attachments/assets/a49ea2b8-116b-4534-a628-1d30ed0f57bd" />

This summary shows in detail that the Hashcat run was successful. Explanation of the most important lines in the output:
Status...........: Cracked: The password was successfully found and cracked because it was included in the rockyou.txt wordlist used.
Hash.Mode........: 0 (MD5): Confirms that Hashcat processed the algorithm as standard MD5 (mode 0).
Hash.Target......: 6b1628b016dff46e6fa35684be6acc96: The exact checksum (the target hash) that was searched for.
Guess.Base....... : File (rockyou.txt): Indicates that the rockyou.txt dictionary was used as the source for the words.
Speed.#1.........: 20881 H/s: The current processing speed in the virtual machine (approximately 20,881 password attempts per second on the CPU).

# Sources
Karvinen 2018: https://www.bionicmechanic.com/images/IoT%20Rapid%20Prototyping%20Laboratory%20Setup.pdf

Karvinen 2020: https://terokarvinen.com/2020/command-line-basics-revisited/

Karvinen 2022: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

