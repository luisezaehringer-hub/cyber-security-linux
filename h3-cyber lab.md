# x) read/watch/listen and summarize:
Karvinen 2020: https://terokarvinen.com/2020/command-line-basics-revisited/

Karvinen 2022: https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

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

# a) See Laksu

# b) Install Linux on a virtual machine. Use Debian 13-Trixie
I installed Linux through UCM. Debian 13-Trixie took some time to install it and I wasn't quite sure if it ran correctly in the beginning. Now it looks fine. The Installing process was kind of easy, even though I dont really have that much of experience in running that process. Lastly, I created a VM.

<img width="450" height="49" alt="image" src="https://github.com/user-attachments/assets/69557f7f-d54b-426c-a6e7-b49b20f66cd8" />


<img width="882" height="590" alt="image" src="https://github.com/user-attachments/assets/1823a6d0-338d-41bd-a5e6-785438ac6008" />

further information can be found in my h2-scene report: https://github.com/luisezaehringer-hub/cyber-security-linux/blob/main/h2-scene.md

# c) crack
This was quite challenging, since my terminal didnt accept some of the commands. With a bit of trying out and taking some time, I could get my terminal to running all the commands. The problems were mostly because of my operating system (Mac). 
