This is the first step of how to install Ubuntu and CUDA. Installation of CUDA is very stupid and I waste lots of time on how to find the right way and make all the path set. Hence if you want to use it in the future, just simply follow the steps I wrote. 

---

# Make a bootable USB stick

This is the begining of the story. Time now is July 2020. There might be other Ubuntu system in the future, at this moment, Ubuntu 18.04 is still the best one. I tried the Ubuntu 20.04. The interface is good, GUI is perfect, but there are lots of bugs there. Like the app store never updates, the `gcc --version` is too high for the CUDA installation (gcc version needs to be lower that 8). Hence if you plan to start the Ubuntu travel, I highly recommend you to use Ubuntu 18.04. Here is how to install it.

[Official Guidance on how to make a bootable USB stick.](https://ubuntu.com/tutorials/create-a-usb-stick-on-macos#1-overview)

Actually all you need to do is simply follow it step by step. I just want to mention a few things that I may forget in the future:

1. This bootable steps in based on Mac OS, can I install it on other kinds of machine?

- A: Of course you can. This is just a way to make the installation system into your steak. You can install it in any machine you like. By the way, if you plan to use the `balenaEtcher` app as the steps says, you may fail at your first attempt. Don't worry and try this again. You'll be fine at second time. After you finish, it will say "The system cannot read this file". DON NOT CLICK INITALIZE! Just eject it. Otherwise you need to start it from begining again.

2. I cannot find the right way the erease the disk/USB?

- This is because you didn't choose the right options of your USB. I mean, you are under the child disk of it, and you need to go to the root folder of it. Be careful when you install it. You can see the pickture on the official website, there are two dick on the left side on the picture. Choose the parents one. (If it doesn't display there, you just need to change some settings of your mac. Google it.)

# Install Nvidia Driver

(You may be asked to insert boot device. This is because you change the secure boot sequence. Just reboot the  PC, press F12 or F10, go to the menu, go to the settings and check the secure boot optiond. You will find the way to tell the disk start with Ubuntu. Or you can just simply call Ubuntu by F12 everytime the system awake)

After you insert the USB into your new computer, press F12 or F10 to boot from USB, and follow the steps to install the Ubuntu, the first thing you need to do is install/update the driver. Especially when you are using the Graphic Card from Nvidia. There are many guidances on the Internet telling you how to install it. But here I only recommend one way.

1. Check the driver series number for your driver.

You can check it from [Nvidia official website](https://www.nvidia.com/Download/index.aspx), but Never Never download it here. Remeber the series number, like 440, you just need to do it.

2. Go to the `Software Updated` in your menu of Ubuntu. Click it, any choose the option of `Addition Driver` (You may need to click the `Live Pitch` to get there). You'll find the driver with the series number as you just remember from Nvidia website. Choose it and apply. There are few reasons that why you need to install here, like it can avoid the mistakes you may make when you install it manually, or it will hurt you system settings. I just skip the reasons. Just do it.

3. Reboot your computer. Then you can try `nvidia-smi` to check.

4. You may find a interesting thing: You will see a CUDA version in your `nvidia-smi`. And it is very possible with the CUDA you may install later. Don't worry about it. This is just the driver CUDA version and it has nothing to do with the CUDA you will use with your deep learning packages. For further details, just check it here: [Answer](https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi).

# Install CUDA

This is the most annoying part. Nowadays using the library from Github is so necessary and most of the library need you to specify the path of CUDA. The methods I introduce is the only way I think can figure out this question. You may hear many methods like using `sudo apt-get cuda` or something like `wget ...`. This could work, and it can be used by PyTroch and Tensorflow. However, the thing is that it will mess up all the path, and you will find the folder of CUDA is empty. This is because the system set all these paths into several other folders. [You can check my previous question here](https://askubuntu.com/questions/1256378/question-about-installing-cuda-10-1-on-ubuntu-20-04-the-root-folder-is-empty) Therefore, the methods I recommend here can solve this problem.

1. Download the file from CUDA official website.Like for CUDA 10.1, you can just Google it can find the [site.](https://developer.nvidia.com/cuda-10.1-download-archive-base) Download the file. (Usually end with the `.run`)

2. Don't hurry to run it. It won't work becuase of the most annoying problem of `nouveau`. You need to block it. However, don't follow the steps that you can find from StackOverflow. They will broke your system and make your Ubuntu run very slowly. The only thing you need to find is the official website: [Official Guidance](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html). Go to 4.1.5.2 for Ubuntu part.

3. OK now you just follow the steps. Be careful the `conf` is `blacklist-nouveau.conf`, don't use other name otherwise it won't work. 

For step 1: (Always remember this is 07/2020 version. Make change with official guidance)
```
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nouveau.conf"

# use cat to check it
cat /etc/modprobe.d/blacklist-nouveau.conf

# then update it
sudo update-initramfs -u
```

You may find the step 2 is confusing: `runlevel 3 by temporarily `. What's that?

Well, you just need to run the following steps:
```
# go to the folder
cd /etc/default
ls

# you can find a grub here, open it
sudo gedit grub

# change the line
grub_cmdline_linux_default = 'XXXXX(What ever it is) 3 nomodeset'
# simply add it to the end, then save and exit

# update it
sudo update-grub
```

You may meet some miskate when you save the file with gedit. Don't worry is just a warning. You won't meet these problems with `vim` (i for insert and `esc then :x` for quit). Now you can install the `.run` without problem. If `sudo sh ..` can not find the file, then go to the root folder. (Usually the download file)
```
sudo sh cuda_10.1.105_418.39_linux.run --silent
sudo nvidia-xconfig
```

After it is finished, check whether it is on the right path:
```
# update the path in the system
sudo updatedb

# find all cuda path
locate cuda | grep /cuda$
```
Feel free to go inside it can check the files.

OH! You may forget an important thing! Do you remember we TEMPOTARILY use `nomodeset`. Don't forget to go to Grub again and delete it after you finished your installation. Otherwise you will lost the GUI when you reboot. (Of course you can use command line to reach to grub and edit it, then reboot. Don't forget to add `sudo`!)

4. Set the path

You can follow the step as the official website says:
```
export PATH=/usr/local/cuda-11.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
But it make no sense if you just follow it. When you input `nvcc -V`, it won't show you the right version. How to do it? Go to edit the `.bashr`.
```
# open it
nano /home/zihan/.bashrc

# add the path above to it(Change if your CUDA file has different name)
export PATH=/usr/local/cuda-11.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

# update
source .bashrc

# now you can check it
nvcc --version
```

Now we reach to the end of Step 1. I'm not sure whether things will change in the future. Always follow the official guidance. Official ones are the best!
