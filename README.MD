# ✨Welcome to my OSCP Tips and Tricks!✨

I believe in sharing information and tips to help others excel on their journey to OSCP. 
There are plenty of charlatans out there who charges for information that are open-source (or stolen from another person) and that is a mighty shame.
Part of what drew me to this field was the generous information and experience sharing from other professionals so this is my way of contributing back to that deep pool of expertise.

## A Quick Warning

Depending on how much knowledge you already have and whether you are working a full time job or have to dedicate time for family matters,
you are looking at anywhere from a 100 to 300+ hour investment to study for the OSCP.

If you have not gotten your PWK course, I strongly suggest getting the **LearnOne One Year Subcription** instead of the three months package.

I had no choice since it was company sponsored and I was lucky to have some background, free time off and a supportive partner to put this much time into OSCP so three months were sufficient. More details are available below!

## 📚How to use this guide

Everything is in .md so you can just download them or clone them and you can use it in obsidian or notion. Feel free to modify for your own needs!

I will personally start with preparing the environment and then just go down the list.

I placed links below for easier navigation:

1) [Preparing for the Exam](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/00-Pre-OSCP%20Preparation.MD)

2) [Mindset and Strategy](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/02%20-%20Mindset_Strategy.md)

3) [How to Nmap](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/01-Nmap.MD)

4) [Service Enumeration and What to do](https://github.com/OoStellarnightoO/OSCP_Notes/tree/main/03%20-%20Service_Enumeration)

5) [Dealing with Web Services](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/03%20-%20Service_Enumeration/80_443-HTTP_HTTPS.md)

6) Pivoting - [Ligolo-NG](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/05%20-%20Pivoting/Pivoting%20with%20Ligolo-ng.md), [Chisel](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/05%20-%20Pivoting/Pivoting%20with%20Chisel.md)

7) [Windows Host Enumeration](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/06-Windows%20Enumeration.md)

8) [Linux Host Enumeration](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/07-%20Linux%20Enumeration.md)

9) [Windows PrivEsc]()

10) [Linux PrivEsc]()

11) [Cracking Passwords](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/11-Cracking.md)

12) [Active Directory Enum, Lateral Movement and PrivEsc](https://github.com/OoStellarnightoO/OSCP_Notes/blob/main/12-ActiveDirectory.md)

13) [Other Random Stuff]

----------------

## Quick Background

![alt text](/Assets/mega64_hacking.gif)

I would love to say that I was pwning Microsoft 98 boxes (yea I am old enough to have used diskettes and MS98) at 6 years old and solving leetcode at 12.

But no. I was a non tech person for most of my life and the hardest technical thing I have done before the last two years was doing up a static HTML page.

In 2022, I came into contact with Cyber Threat Intel folks in my workplace and I was mesmerised by their briefs about APTs and Ransomware groups.
I decided to do some online research to find out more (skipping the part where I found CEH first) and found TCM Security affordable but yet comprehensive course in mid 2022. It was pre-subscription model days and I now have lifetime access to the courses I got.

I failed my first PNPT in October 2022 and deservedly so because I did not even know what is the difference between an external and an internal network. My basics were really weak. I thought perhaps I was not fated for this and i put my cyber stuff on hiatus as my busy worklife took over my free time.

In December 2023, I decided to approach PNPT with a fresh perspective, redid all the courses and boom I completed the exam in 1.5 days and was then certified on 26 December 2023. Perhaps I can thrive in this field. Did a career transition in mid 2024 to join a cybersecurity transition programme to *git gud*.

Did cyber things day in day out, learn about Blue and Red Team stuff on TCM Academy, THM and HTB. Got my GCIH in November 2024 and now OSCP+ in Jan 2025.

I only got 70 points for OSCP+ though so I am sure you can do better than me!

## Am I ready for the test?

This seems like a perpetual question on the OSCP subreddit and also no doubt on every test taker's mind. I myself had doubts about my readiness prior to the test.

TLDR? You are ready when you can start a box and you can run through your enumeration like a well-oiled machine and identify potential vectors for investigation. When you run into difficulties, you look through your notes, make sure you didn't miss anything out and you go out and google. 

When you get a shell, you don't blank out. You start manual enumeration and then run the tools. You come out with hypotheses on what to try out and execute without hesistation. You can only reach this state if you do boxes over and over again until it is second nature.

Think about it. How did you learn to cycle (just stay with me if you don't know how to cycle)? 

The first few times, you fall over. 

The next few times, you figured out how to peddle and you get somewhere but you dont really know how to turn and you crash when the route gets narrow. 

The next few times, you adjust your body weight subtly and you turn well but you have difficulty climbing hills. 

Then you realise that you can change the gears on your bicycle to make it easier to climb.

The same applies to the OSCP. It sucks at the start but take good notes, learn from your failures, try new things, note if it works (or dont), and keep going until it becomes second nature.

---------------------

Let me share my own experience so that perhaps in some manner, you could relate to it.

I was basically given full time off for about six weeks from my work to prepare for the OSCP. This was my rough schedule:

### Timeline

**Last week of November 2024:** Activated my three month PEN-200 access

**1st Week of December 2024:** Went on one week holiday (haha)

**Rest of December:** Drill PEN-200 materials. I did not complete the whole course and speed run the enumeration, and AD stuff as TCM's materials were more than sufficient. I left AV-Evasion, and the AWS mods untouched as these were not within the scope of OSCP+ as of January 2025. Subscribed to one month PG on 29 December 2024.

**1st Week of January 2025**: Completed Secura, Relia, Medtech, OSCP A/B/C. Started doing PG boxes from Lain's list

Started Full time work here

**2nd Week of January 2025**: Completed most of Skylark. Half of Lain's recommended PG boxes

**3rd Week of Janurary 2025**: Completed all Lain's boxes. Redid OSCP A/B/C without any hints. Also did some TJNull PG boxes

If you can OSCP A/B/C without much difficulty, I would say you can probably go for it. To increase your chances, I would suggest doing TJNull and Lain's recommended PG Boxes just to have the chance to practice your metholodolgy and increase your exposure to different attack vectors.

----------------------

### FAQ 

**Q:** Was I a uber wizard that did not need to see hints, walkthroughs or seek help from OffSec Discord?

**A:** No! In fact I struggled greatly when doing Medtech, Relia and OSCP A and B. My methodology wasn't well established and I was poor with manual eumeration. So rather than get stuck and "TRYING HARDER"🤮, I checked for hints to move forward. In fact, I had to check the hints so many times that I wonder whether I will ever make it out of Challenge Labs.

As I reached OSCP C, I realised the frequency of hints I checked dropped dramatically. I was able to solve most of the boxes without hints. But of course the hint seeking came back when doing the PG boxes but as I did more, I started to be able to solve Medium boxes without hints and hard/very hard boxes with some hints here and there.

As you do more boxes, you start to intuitively understand how OSCP design its boxes and you start to shape your methodology towards it.

The key takeaway here is to take the hints, update your methodology and notes. If you run into a similar situation, you want to be in a state where you don't need the hints anymore because it is in your notes and you understand the problem at heart. **So it is fine to resort to hints and walkthroughs as long as you learn and NOTE the tool, method and the thinking behind it! Also do more boxes if you can**

-------------------------------

**Q:** Is the course PWK enough for me to pass the OSCP?

**A:** From my perspective? Sad to say that it is a resounding no especially if you have no background. Go google and search for OSCP tips and you will see that a majority of the advice revolve around additional materials from TryHackMe, HacktheBox, and TCM Security. Now I caveat, I actually think the Challenge Labs and the PG boxes (you might need to pay for this if your subscription doesnt cover) are the best part of it and the most relevant to OSCP preparation.

You will see defenders of OSCP saying that it is all about the Trying Harder🤮 mindset and googling for things that are not covered in the course. This avoids the big elephant in the room. Why it is acceptable for a course costing 2k USD to be not comprehensive in its materials?

Does the PWK material suck? No! It is actually decent stuff. My problem with it is that it does not prepare you for the exam sufficiently and the ridiculous price point. Yea I am pretty hung up over the cost if you can't tell by now. Why? 

1) It does not instil the methodology and the thinking when confronting a box. Example, if you are doing AD, the first thing you should absolutely do is to go after the low-hanging fruits which is kerberoasting, as-rep and password spraying. This is not evident from the course material. 

- On this, OffSec obviously can do this job. Look at S1ren's videos. That is methodology. Why can't OffSec include a module on her metholodogy? She is not even referenced in the PWK materials. (you can find her videos on youtube.)

2) There are a lot of useful but not relevant materials for the OSCP exam. Yes, learning how to live off the land and use native binaries are great for red-teaming but for a 23hour 45 minutes exam? You are going to be using tools. Once you start PWK, you realised that the material does not respect your time. You will be asked to create enumeration and exploitation scripts manually and then after wasting one hour or more of your time (per lesson), they tell you hey here is this (sometimes outdated) tool that does all the above and more. As a working adult, I just don't have the tolerence for this kind of nonsense. 

3) Why teach SQLMap, Nessus, Metasploit when they are either outright banned or heavily restricted. Come on. And again if you are here for the knowledge, there are way better sources for these stuff (and cheaper). Want to learn SQLi and Web App pentest? BurpSuite Academy has a free course. Want to use Metasploit well, HTB has a fairly comprehensive section on it.

4) They have the gall to create OSCP+ and then not bother updating their modules and challenge labs. Some of the tools taught are already deprecated like crackmapexec, legacy bloodhound. There is no mention of tools such as ligolo-ng, netexec when these has proliferated for at least two to three years and widely used by people taking the test. The materials does not account for the assumed breach scenario. I can accept not updating materials timely if your course is cheap but a course that cost 2k USD and markets itself as THE PENTEST cert? There is really no good excuse. **Update Feb 2025** OffSec has FINALLY updated its OSCP A/B/C to an assumed breach scenario but wait for it! The AD boxes are exactly the same only that they give you a valid pair of creds for you to immediately get a foothold. That is incredibly dissapointing but kind of expected to be honest.

5) OffSec discord support and its Student Mentors are wildly inconsistent in terms of reliability. Some Student Mentors are quite nice and provide good advice. Some are literally TryHarder folks. I even see Student Mentors contradicting each other on the intended path for the labs. I see a Student Mentor telling someone who had difficulty setting up ligolo to use chisel instead because ligolo is not taught in the course. Another Student Mentor was more than happy to address ligolo issues. These Student Mentors are paid employees of OffSec and your contact point when you get stuck. So why the inconsistency? For 2k+ USD? This is again not acceptable. 

But hey, lets create a cringe song about Trying Harder. (The below is really a song from OffSec) 

https://www.youtube.com/watch?v=t-bgRQfeW64


--------------------------------------

**Q:** What Challenge Labs should I be doing?

**A:** Minimally OSCP A/B/C. If you have the time, do Secura, Relia, Medtech and maybe parts of Skylark. OSCP A/B/C somewhat resembles the actual exam though with the assumed breach scenario, the AD set is not entirely accurate but you get a good sense of the exam (they give you creds now). Zeus and Posiedon are generally thought of as out of scope for the exam.

----------------------------------
**Q:** How difficult is the exam compared to the challenge labs?

**A:** There is no good answer because difficulty is subjective and complicated by the fact that there are multiple exam sets from what I have heard. I am sure you would have heard of this so called nightmare Jenkins AD set (which certainly did exist in the previous edition of the OSCP given its noteriety but I am not so sure about the new OSCP+). My AD Set was stupidly easy with the difficulty on par or even slightly easier than OSCP A/B/C and I definitely lucked out there. That said, my standalones felt like PG Very Hard.

IMO, if you fail OSCP, it may not be because you are not prepared, but it may be because you were just presented with a set of very hard boxes or had terrible luck with the exam environment. If you passed OSCP, it is not necesssary that you are certified hax0r. Maybe you got lucky with the boxes. So stay humble even if you have OSCP and don't be too hard on yourself if you failed.

----------------------------------------
**Q:** Lain or TJNull?

**A:** Why not both? Instead of trying to "optimise", just do boxes with emphasis on completing the PEN-200 Challenge labs first before doing the PG Practice and Play boxes recommended in both lists. I strongly recommend doing the "Administrator" box from HTB as it is a assumed breach scenario and allows you to practice ACL abuse.  **For me what worked was the sheer amount of boxes I have done over the past month. So much so that I can quickly identify an attack vector when scanning and enumerating**

----------------------------------
**Q:** How do you notetake?

**A:** Different strokes for different people but I love Obsidian because of its community plugins. I sync my notes using Google Cloud (you could pay for Obsidian Sync if you want) and I used the Templater plugin to instantly plant my enumeration template when i note take for a box.

------------------------------


## Other Awesome Resources!

1) This is your best friend in the exam > (https://book.hacktricks.wiki/en/index.html)

2) Reverse Shell Generator > (https://www.revshells.com/)

3) **Offsec's S1ren**> (https://www.youtube.com/watch?v=h1Br5umYxwc)

Special Mention for S1ren. I like her methodology and have adopted her way of approaching a box. Please give her a watch and understand the methodology she is using.

4) LainKusanagi's List of Boxes to do for OSCP > (https://docs.google.com/spreadsheets/d/18weuz_Eeynr6sXFQ87Cd5F0slOj9Z6rt/edit?gid=487240997#gid=487240997)

5) TJNull's List of Boxes to do for OSCP > (https://docs.google.com/spreadsheets/u/1/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview)

