# Introduction

The OSCP may be 23 hours 45 minutes but you have a lot longer than that to prepare your environment for the exam. Proper Preparation is half of the battle for OSCP. The last thing you want is to scramble for working tools on the exam itself.

For me, my trusty Kali VM decided to throw tantrum regarding my netexec dependencies after I installed penelope to give it a try.

So the day before my OSCP, I prepared a completely fresh Kali VM. This is how I did it and what worked for me.

Feel free to modify to your own needs. I also added some videos that I thought were useful.

-------------------------------------------------------------

## Steps

Now you have installed your Kali. What's next?

First of all, I would like to introduce this wonderful script by Dewalt that would help install 95% of the tools you need for OSCP:

This includes things like nxc, seclists (i am surprised that this does not come native with kali), enum4linux-ng etc

https://github.com/Dewalt-arch/pimpmykali

Do this first. Read more here: https://tcm-sec.com/pimpmykali-pmk-v-2-0-release/

_That aside, I am always an advocate for TCM Security's affordable courses so give them a look if you wish!_

```bash
# Remove existing pimpmykali folder
rm -rf pimpmykali/

# Clone pimpmykali repository & enter the folder
git clone https://github.com/Dewalt-arch/pimpmykali

cd pimpmykali

# Execute the script - For a new Kali VM, run menu option 'N'
# (The script must be run with root privileges)
sudo ./pimpmykali.sh
```

