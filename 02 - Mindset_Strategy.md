# Mindset and Strategy

This was a fairly late addition and did not exist in my notes. However this is something I feel should be addressed early. 

First of all, OSCP is an exam and it is important to treat it like any other exams you have taken in your academic life.

That means understanding the exam format and the environment. I see OSCP more like a puzzle game or more accurately a CTF. There are rabbit holes and there are intended paths. If something looks suspicious, it is (probably) suspicious and is the intended path. 

How do you know it is a rabbit hole? Well you cant really say 100% for sure but if you tried everything you know of and it doesn't work for like three times and after reverting the machine, it still doesn't work? it is probably a rabbit hole. 

How do you prepare for CTFs or puzzle games? By doing more CTFs (i.e. Challenge Labs and PG Boxes) and understanding how a puzzle is designed (by offsec).

This is why I do not really recommend doing any other additional boxes that ARE NOT by OffSec because OffSec design their boxes in a certain manner.

You need to also keep in mind that your end goal is to **PASS** the exam. While it is good that you want to broaden your horizon and learn everything under the sun for pentesting, you need to recognise the facts that: 

(1) the OSCP exam, PWK and Challenge Labs access expires; 

(2) You have limited time in your waking hours especially if you have full time work or have kids; and 

(3) you spent 2k USD to get the certification so focus on that. If you want to learn, there are better ways of spending that 2k USD such as on HTB's CPTS or Zero Point Security's CRTO. 

Now I cannot guarantee you will pass the exam with my notes because of the RNG nature of the exam, your physical and mental state on the exam day, the "stability" of the exam environment and your own preparation. So treat my set of notes as yet another datapoint to increase your chance of passing.

Some videos and articles that I found to be actually useful: 

(Sorry to be a little annoyed at the many self-congratulatory articles on medium about how they pwned the OSCP in five hours or tried harder and all that truism mantra but offer little practical advice other than **YOU GOTTA TRY HARDER**. Ok rant over but You can probably tell by now about my thoughts on "Try Harder")

https://medium.com/@redefiningreality/your-bs-less-guide-to-acing-oscp-4eccaf497410 - This is truly a bs-less guide. My inspiration actually.

https://www.youtube.com/watch?v=pvNYaUs0aqc - How to Pass the OSCP+ Exam on Your First Try (Full OSCP+ Success Guide 2025). I went through this video and can attest that this is an excellent and comprehensive video.

https://www.youtube.com/watch?v=pIdbPp6vNmE - I was initially unsure when I saw the preview image as it resembles one of the many (not so useful) videos on how to pass OSCP but having gone through the video, It is not only hilarious but VERY USEFUL AND PRACTICAL.

## Enumerating, Attacking and Pwning a Box

I am not going to talk about tools or commands here. Rather I want to talk about broad concepts on how to approach a box in general. 

When you don't have a (OSCP-ish) methodology or don't question what a tool is supposed to achieve, that is how you fail the OSCP. The course does not cover every scenario and tools that you will need to pass the exam (whether this is fair or not is another matter all together). Try to have different ways of achieving the same goals. Have an admin shell but cant get its password or hash? Use your admin powers to create a new admin user and then dump hashes via nxc if mimikatz has issues.

To maximize your chance of passing and to not just rely on the ambiguous advice of "TRY HARDER", think of the approach to a box in this manner:

**Objective**: Get Administrator/Administrator-level/root access on the box

< W I P>

If you have not achieved so already, obtain a foothold on the machine, this can be looking for credentials in configuration files, insecure services, RCE, path traversal, exploits etc.

Assuming that you have already gotten into the system, if you have an unstable or limited shell, try to upgrade it to enable more functionality and readability within the session with Python.

**On Windows :**
Manual enumeration should ideally target easy to exploit and find vulnerabilities such as `whoami /priv` which tells you straight away whether Spoofer or Potato exploits can elevate your permissions into a SYSTEM user. 
Unquoted spaces can also be a potential privilege escalation vector that is simple enough to check and abuse.

Always check the User directories for traces of important files. The `C:\` drive can sometimes provide information about available services or hints that can be inferred to point towards the intended path.

**On Linux :**
Crontabs usually generate less output compared to Window's task scheduler which can also be an easy vector to privilege escalate. 
If your user has sudo permissions, you can check them with `sudo -l`. Looking for SUID permissions on binaries also fall into this category where if you have permissions to run certain binaries with sudo permissions, you can refer to GTFObins to aid you with the commands required to privilege escalate.
Other simple to exploit vectors include checking for capabilities as well as writable files/directories (adding a root level user on /etc/passwd would be super simple to do)



## The Exam

My suggestion is to discuss with your partner/family that you are going to be doing this intense 23h45 mins exam and set aside a day with no distractions and where you will be well rested the day before, so no night shifts or crazy busy days. Take a half day off the day before if you need to.

The day before, ensure that your VM is working, your camera set up is working and all your tools are working. Then sleep early

Schedule your exam around 2 to 3 hours after your normal waking hours. I would suggest getting a light exercise, a nice breakfast and a cup of coffee.

30 minutes before the exam, open up the various tabs from the guide on the proctered exam that offsec provides to the troubleshooting guide.

Set up your favourite notetaking software if you had not.

Once the exam start, this is my recommendation:

1) Start full nmap scans for the three standalones. 

2) Go after the AD first. I think the AD is the easiest part of the exam and getting 40 points off the bat can be a massive morale boost. This is an assumed breach scenario and you will be provided domain credentials. You can't query the DC immediately as it is on a different subnet but I am positive that the domain creds can get you a shell on the system be it via rdp or win-rm. From there, run your domain enumeration tools.

3) IMO, AD boxes in OSCP are a bunch of windows boxes that happen to be in AD environment. You will still need to use the Windows PE methods to own the domain.

4) Just as in OSCP A/B/C, you need to pwn the first AD machine, then move on to pwn the second one and then from there pwn the DC. 

5) Try and get the AD within four hours or so. I suggest taking a quick 15 min break once you pwn the first AD and taking a longer 30 minute break after getting the whole AD.

6) If all goes well, after taking the AD, it is probably lunch time. I would say get a good lunch and maybe a shower to refresh your mind. also might as well redo a full scan for all the three standalones while you are having lunch/showering.

7) Now concentrate on getting a foothold on any of the three machines and root it if you can. 

8) By dinner time, if you have zero progress, i suggest taking a nap. This is because at this stage, you are mentally and physcially tired. You still have 12+ hours. Take a nap, a hot shower, a quick bite and coffee and go after the boxes.

**Most importantly, stay calm. You run out of things to do before you run out of time. Sometimes, you have to just treat some artefacts as deliberate rabbit holes no matter how tempting they look. If they just dont work, it is likely a rabbit hole.**

