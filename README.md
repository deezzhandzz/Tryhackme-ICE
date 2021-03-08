# Tryhackme-ICE
Task 1 is you connecting to the network Task 2: Recon  #1 Start up and deploy the machine! It took 2 minutes for me but it can take up to 5 depending on your computer #2 Launch the scan to the target machine. The scan command will be in the hint button, You can use whatever scan you want for this lab you just have to make sure to get all ports  #3 Once the scan is done, you should see a number of ports that are open on this machine. And if you haven't guessed already, yes the firewall has been disabled. So currently there's not much protecting this program. One of the interesting ports that is open is named “Microsoft Remote Desktop” (MSRDP) for short. RDP usually runs on port 3389. Task 3: Gain Access #1 Now that we have  identified some interesting services running on the targets machine, let’s look a little more deeper into one of the weirder services we identified: Icecast, or at least this version running on our target. This is heavily flawed and is a high level vulnerability with a number of  7.4.  #3 By now you should have found the vulnerability, let’s find our exploit. For this part of the room, you are going to use the Metasploit module associated with this exploit. Let’s start Metasploit using...`msfconsole` #4 After Metasploit has been opened and started, let’s search for our target exploit. We are going to be using... ‘search icecast’. #5 Start and select this module for use...`use icecast` or `use 0`  #7 Let’s put that last option to our target IP. Now, let’s run our exploit using...`exploit` Task 4: Escalate #1 We have gained a part into the persons machine!  #2 The commands used in this question and the next are taken from the Metasploit’ room. #5 Now that we know the architecture of the process, let’s perform some further recon. run the following...`run post/multi/recon/local_exploit_suggester`.btw this can take like 1 mins so be patient and let it complete* #6 Running the local exploit suggester will return quite a few results for potential escalation exploits. What is the full path (starting with exploit/) for the first returned exploit? #7 Now that we have an exploit in mind for elevating our privileges, let’s background our current session using the command `background` or `CTRL + z`. Take note of what session number we have, this will likely be 1 in this case. We can list all of our active sessions using the command `sessions` when outside of the meterpreter shell. #8 select our previously found local exploit for use using...`use FULL_PATH_FOR_EXPLOIT` #9 Local exploits require a session to be selected, set this using...`set session SESSION_NUMBER` #10 What is the name of this option? Answer: lhost #11 You might have to check your IP on the TryHackMe network using...`ip addr` If you get a notification of “Exploit completed, but no session was created.”, try checking that you have to correct IP address set because I didn't the first time around. #13 A new session will be opened. Interact with it now using `sessions SESSION_NUMBER`    #14 We can now make sure that we have expanded permissions using...  `getprivs`.  Task 5: Looting #1 the service responsible for authentication within Windows. First, let’s list the processes using the command `ps`. Note, we can see processes being run by NT AUTHORITY\SYSTEM as we have escalated permissions  #2 In order to interact with lsass we need to be ‘living in’ a process that is the same architecture as the lsass service and a process that has the same permissions as lsass.  #3 Migrate to this process now with the command `migrate -N PROCESS_NAME` #4 Let’s check what user we are now with the command `getuid`. What user is listed? #5 Now that we’ve made our way to full administrator permissions we’ll set our sights on looting. Mimikatz is a rather infamous password dumping tool that is incredibly useful. Load it now using the command `load kiwi` (Kiwi is the updated version of Mimikatz) #6 Loading kiwi into our meterpreter session will expand our help menu, take a look at the newly added section of the help menu now via the command `help`.    #8 Run this command now. Mimikatz allows us to take this password out of memory even without the user ‘Dark’ logged in as there is a scheduled task that runs the Icecast as the user ‘Dark’. It also helps that Windows Defender isn’t running on the box ;) bothfirewall is down =^.^= Task 6: Post-Exploitation #1 let’s revisit the help menu one last time in the meterpreter shell. the rest of questions are there   #3 While more useful when interacting with a machine being used,   we can modify timestamps of files on the system.  #6 Mimikatz allows us to create what is called `golden ticket`, allowing us to authenticate anywhere easily.  #
