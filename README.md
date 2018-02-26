# CyberSecurity-Project-II
Mooc Project

Overview of the procedure to follow can be found in the "Is it easier to fix the applicationa than to detect attacks.docx" file, which is the more readable version of my essay.
You can clone the repository or download the zip. I have put a snort directory in the Virtual Machine network share and the snort1.bat and snort2.bat files refer to that directory. That way, I could more easily work on my Ubuntu 17.10 desktop in msfconsole and edit rules files, the wsnort.conf etc... . wsnort.conf is the windows snort.conf file and has the right slashes etc... .
The first commit has the rules file and wsnort.conf so that the attacks generate alerts. The fifth and sixth commits have rules files and wsnort.conf that generate no alerts. The diff clearly shows which changes are necessary to eliminate the alerts

Software versions used:
- snort on the Virtual Machine: Snort_2_9_11_1_Installer
- winpcap on the Virtual Machine: WinPcap_4_1_3
- metasploit: Framework: 4.16.40-dev- ; Console : 4.16.40-dev-
- nmap: Nmap version 7.60 ( https://nmap.org )

Network setup: Virtualbox gave me a vboxnet0 and vboxnet1 ethernet. The VM was reachable on 172.28.128.3. A second networkcard seemed to only receive on 10.0.2.15. My desktop was on 172.28.128.4.

# More detailed instructions for reproducing the attacks
Available upon request. Once you know the exploits to use in msfconsole, the main thing is to set LHOST, RHOST and the PORTS.
