---
title: "Xrandr Blog Post"
author: "Erik Andersen"
date: "11 May, 2022"
output: 
  html_document:
    toc: true
    toc_float: true
    keep_md: yes
---



## Intro

This blog is going to focus on the important issues. You know, the real down to earth stuff that affects everyone. And not just first world issues either, we will practice inclusiveness. To demonstrate my commitment, this first post will be all about making sure your dual monitor setup runs smoothly when you switch to linux. 

If you're anything like me (and I assume you are), you took a machine learning class in college, and realized windows doesn't let you run code in parallel as UNIX based systems, so you *very* sneakily convinced you dad you needed a new 500 GB ssd to dual boot linux on. Then as soon as you booted it up, the OS decided that your secondary monitor should really be the main one: *very* frustrating. Power user that you are (you swapped to linux after all), you used all of your skills built up from years of using windows, booted up the [Nvidia menu](https://forum.manjaro.org/t/how-do-you-use-nvidia-x-server-settings-exactly/96678), toggled the switch that swaps your default monitor, then calmly went about your business. Now your monitors were working perfectly, and you were all ready to be a new linux hacker until... you restarted your computer and it swapped back. This was when you first learned that in linux many settings aren't persistent like they are on windows, and *gasp* this is a feature!

I have two goals for this post. Nominally, it is about how to use Nvidia settings correctly on Ubuntu 20.04 and above, but the more important message I hope to convey is how to how to go about trouble shooting a linux system when you first come from windows. There is extensive documentation on just about everything you can do on a linux system online, but for a brand new user, its not very helpful. You have to know what you want to do, how to look it up, and then what to do with that knowledge. In the long-run this will be a much more useful skill than knowing the [xrandr](https://www.x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html)^[Note: Since the first draft, I have found a different (possibly easier) way to solve this for Nvidia cards. The solution is linked [here](https://askubuntu.com/questions/379483/nvidia-x-server-settings-lost-on-every-reboot)] command is the correct one to swap your display settings. Next time, you (and I) won't have my blog post available with the perfect answer available^[I definitely will be SEO boosting this thing, so it will appear over Stack overflow for anyone trying to solve this issue], so knowing the basics of how to problem solve in linux will be necessary. Hadley Wickham preaches having a [scientific mindset](https://adv-r.hadley.nz/introduction.html#meta-techniques) when doing data science; you should do the same when working in linux.

### Solution

I don't want to be accused on my very first blog post of being on of those cooking blogs where they bury the recipe after their whole life story (ignore the three paragraphs above), so I will skip to the punchline and start out telling you the commands to make your monitors work correctly. 

The command we'll be using is **xrandr**. Xrandr stands for *resize, rotate, and reflect extension*. This is a powerful command that lets you tinker with pretty much any setting you could think of for your monitors. Let's look at the output first to see what we're dealing with.^[I don't think there's any vulnerable info here from my pc, but if there is please don't hack me]


```bash
xrandr -q
```

```
## Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
## DP-0 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
##    1920x1080     60.00 + 143.98*  119.98   119.93    59.94    50.00  
##    1680x1050    119.99    59.95  
##    1440x900     119.85    59.89  
##    1280x1024    119.96    75.02    60.02  
##    1280x720      59.94    50.00  
##    1024x768      75.03    70.07    60.00  
##    800x600       75.00    72.19    60.32    56.25  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    72.81    59.94    59.93  
## DP-1 disconnected (normal left inverted right x axis y axis)
## HDMI-0 connected 1920x1080+1920+0 (normal left inverted right x axis y axis) 598mm x 336mm
##    1920x1080     60.00*+  59.94    50.00  
##    1280x1024     75.02    60.02  
##    1280x720      60.00    59.94    50.00  
##    1152x864      75.00  
##    1024x768      75.03    60.00  
##    800x600       75.00    60.32  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    59.94    59.93  
## DP-2 disconnected (normal left inverted right x axis y axis)
## DP-3 disconnected (normal left inverted right x axis y axis)
## DP-4 disconnected (normal left inverted right x axis y axis)
## DP-5 disconnected (normal left inverted right x axis y axis)
## USB-C-0 disconnected (normal left inverted right x axis y axis)
```

It looks confusing, but don't worry, there's only one bit of information we need from here: the monitor names.^[You can also use the following command to extract the names directly, but I find regular expressions a bit confusing, so I currently prefer to do it manually. xrandr | grep -e " connected [^(]" | sed -e "s/\\([A-Z0-9]\\+) connected.*/\1/"] The monitor names are the strings on the left called DP-0 or HDMI-0. I have a lot of options, but only those two are connected. As you can see my HDMI-0 is the primary monitor, which is wrong. I want it to be DP-0. To switch it I run the following command.


```bash
xrandr --output DP-0 --primary
```


```
## Screen 0: minimum 8 x 8, current 3840 x 1080, maximum 32767 x 32767
## DP-0 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 597mm x 336mm
##    1920x1080     60.00 + 143.98*  119.98   119.93    59.94    50.00  
##    1680x1050    119.99    59.95  
##    1440x900     119.85    59.89  
##    1280x1024    119.96    75.02    60.02  
##    1280x720      59.94    50.00  
##    1024x768      75.03    70.07    60.00  
##    800x600       75.00    72.19    60.32    56.25  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    72.81    59.94    59.93  
## DP-1 disconnected (normal left inverted right x axis y axis)
## HDMI-0 connected 1920x1080+1920+0 (normal left inverted right x axis y axis) 598mm x 336mm
##    1920x1080     60.00*+  59.94    50.00  
##    1280x1024     75.02    60.02  
##    1280x720      60.00    59.94    50.00  
##    1152x864      75.00  
##    1024x768      75.03    60.00  
##    800x600       75.00    60.32  
##    720x576       50.00  
##    720x480       59.94  
##    640x480       75.00    59.94    59.93  
## DP-2 disconnected (normal left inverted right x axis y axis)
## DP-3 disconnected (normal left inverted right x axis y axis)
## DP-4 disconnected (normal left inverted right x axis y axis)
## DP-5 disconnected (normal left inverted right x axis y axis)
## USB-C-0 disconnected (normal left inverted right x axis y axis)
```

It swapped! For many of you, this is all the info you need, but for beginning linux users (like me mostly), this still isn't perfect. We know how to swap the primary monitor, but we still have to run this script on everytime on start up. There are a number of this to try that I'll discuss later on, but to start out with I'll show you the option I settled for. We're going to make our command into a script, then put that script in our **[.bashrc](https://www.journaldev.com/41479/bashrc-file-in-linux)** file.

We could just put the command direclty into the .bashrc file, which would probably be fine since it's only a single line, but it is better practice^[Source my dad, a 60 year old senior dev.] to make script files, then have .bashrc call the scripts. So, let's make the script. 

We'll start by creating a text file that contains the command from above. 


```bash
# Go to the demonstration directory

cd ~/Documents/EC510/Projects/Blog-Post/playground

# This line assigns assigns the command from above to the file monitor.txt. I have to echo it and put it in quotes, or bash will put the output from the command into the text file. 

echo "xrandr --output DP-0 --primary" > monitor.txt

# Now we print the text file to the screen to check it worked

cat monitor.txt
```

```
## xrandr --output DP-0 --primary
```

Great! Now we have a text file containing our command, but its not exacutable yet, so it won't work. We have to work with linux file permissions to make it work. 




















