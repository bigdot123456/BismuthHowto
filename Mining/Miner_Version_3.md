# EggminerGpu3

Current version is v3.012

Link to [Miner V3 Release](https://github.com/EggPool/EggMinerGpu/releases/tag/3.0.1.2)

See README.txt from the archive, changes in -i management: don't use -i 50 for nvidias anymore!


# Changelog

* 3.012Lin and Win
* New gen
* More Hashrate, specially on NVidias (up to +20% on nvidias)
* *No more need of -i 50 for nvidias.*
* Optimization and benchmarking procedure
* Less CPU Usage
* New metric, FTS
* More Eggs
* See README.txt from the archive, changes in -i management: don't use -i 50 for nvidias anymore!

# Optimization

You can stop the miner, then run the optimization procedure.
Windows: "optimize.bat".  
Linux: add `--action=optimize` or run ./optimize.sh

It'll take a while but will find the best params for your rig, then save them for the miner.  
(file optis.json)

If you physically change your gpus, you'll need to delete optis.json or re-run the optimization.

## What does the optimization do?
It makes a reference measure, then scans all potentially reasonable settings to find the best one.
Does not play with clocks, only algo params.
It stops when it tried all, and save its finding in "optis.json" file.

If you run it with -i 100,0,0,0 for instance, it will only optimize the first gpu.  
(can be usefull if you have 8 identical gpus, its faster, and then you edit optis.json to duplicate found settings)

This optimization not only finds the best raw hashrate, but compensate for gpu run time. It measures a "mhu" (Useful MH/s).  
The longer the gpu runs, the more hash you usually get.  
But the more you risk to give stalled shares.  
This measure takes that into account to optimize a meaningfull setting, not just the best displayed hash rate, but the one that will give you the more shares.

# Benchmarking

add `--action=benchmark`  
If you run it with -i 100,0,0,0 for instance, it will only benchmark the first gpu.  
Reports hashrate and mhu for the current setting.  
Usefull if you xant to try different clocks settings from a script to find the best one.


# Optionnal Command Line params
run `./EggMinerGpuLin3 -h` to see available switches.
  
  * -a ADDRESS, --address ADDRESS  
  Mine to that BIS address
  * -n NAME, --name NAME  
  Use a specific worker name
  * -v                 
  Verbose (for debug)
  * -c, --clear           
  Use clear hostname as Worker name
  * -d DELAY, --delay DELAY                         
  Ms to wait between rounds. Lower hash but easier on GPU (not used)
  * -R MIN, --relaunch MIN  
  close cleanly after MIN minutes, to allow for a clean restart  
  720 or 1440 is safe.
  * -i INTENSITY, --intensity INTENSITY  
  GPU intensity, from 0 to 100 (you can list for each gpu)
  * -f damping_factor. 0 to 90  
  Nvidia damping factor. Can be 0 for AMD only. Default 80 if there is at least a Nvidia GPU
  * -0, --gpu0           
  Go Easier on GPU#0 (display - not needed)
  * -l --lock  
  Add some locks to prevent crashes (it's on by default)

# What to use for damping factor?
- AMD Only: use 0 or 50, it is ignored.
- Nvidia: begin with default of 80. If your cpu is really overloaded, raise to 85 or 90.  
  If your CPU still has free cycles, you can try to lower a bit (70 or 60 min) and see if it gives you more hash (but it will eat more cpu).
  
# What is the FTS?

It's a custom metrics, that gives an indication of how your cpu is able to keep up with the gpus.  
0 is good and should be the target. Greater than 1 is definitely an issue and means high risks of crashing.  
See the 'windows crash' section for tips on how to reduce FTS and crash frequency.

If you have FTS but no crash, there's nothing to solve. FTS are not a problem by themselve, just an indirect indicator.
  
# What about more hash for AMD?

I focused on nvidia first. I thought it would benefit to AMD too, but it's the opposite.  
what makes nvidia faster slows AMD down. So I'll do it in 2 steps. There is still room for improvement.

# Known bugs

-i 100 doesn’t work, still sets i to 50
I had to specify the comma separated list 100,100,100 etc to get the GPUs to 100 intensit


# Windows Crash on nvidia

Try to raise the damping factor from 80 to 90  
just add `-f 90` to the command line in the .bat

Lower overclock or powerlimit (the more you lower power, the more you can OC and keep stable)

use -l (lowercase L)

Don't do other things while you mine

Optimize Windows for minning: https://github.com/DeadManWalkingTO/Windows10MiningTweaksDmW

Once optimized, edit optis.json and try to raise the 'nmh' param.  
Some report great luck with 64 or 96 instead of 32. Keep it a multiple of 16.  
Only do that if your rig crashes.

# Some features from experimental branch are missing

* Missing the colored lines of 2.3.4
* no json status
* no alternate directory

# Stability reports from version 3.001
(it's better now!)

## Linux OK
Ubuntu 16.04, nvidia 387.34 (stable for 2 days +)  
Ubuntu, nvidia 390.25 (stable for 2 days)  
Ubuntu 16.04.03 lts. Driver 387.34 (Works flawlessly)

## Windows OK
Win 10 Pro x64 build 16299, NVidia driver 390.77  
Win 8.1 Version 6.3.9600 - 391.01 drivers - 6xgtx 1070 (about one crash per hour)  
Win 10 16299, Nvidia 391.01, GPU +150 on 1060s and +120 on 1080tis, PLs at 90  
Win 10 Pro x64 build 16299.248, NVidia driver 388.13 (23.21.13.8813) - 6xmsi 1060 gaming 6 gb, OC +100/+0 (stable for 4 days)  
Win 10 x64 1709 Build 16299.309 - AMD Driver Radeon 18.3.2 - 8 AMD RX580 gpus all OC ~+200 - Up 4 days no issues  
Win 10 16299, Nvidia 391.01, 1070 and 970, no OC, stable for 10 hours and still running

## Windows Ko
Win 10 Pro build 16299, NVidia driver 390.65 - Gtx1060 oc +150/0 - crash twice per hour  
WIN 10 PRO build 16299.309 nvidia driver 390.65 - no oc - crashes every 20 min

# HR Reports

You can check and fill in your details here, will help for stats and giving hints for best OC params (thanks freaq!).  
https://docs.google.com/spreadsheets/d/1--SNuCPBRblrlhSbU6QNoE-Hwkw1GWvnggvV633P92g/edit?usp=sharing

# Some FAQs

## If I choose any intensity (except for -i 0, which does completely disable GPU0), the intensity stays at full and the hash rate doesn't change

Indeed: with the current v3 Miner, -i has no more effect.  
It will be re-added in the next version, available soon.

In the mean time, you could run a v2 miner on the first gpu with -i 50,0  
https://github.com/EggPool/EggMinerGpu/releases/tag/2.3.4  
https://github.com/EggPool/BismuthHowto/blob/master/Mining/Experimental.md  
and the v3 miner on the second gpu, with 0,100

Sorry for the trouble!

