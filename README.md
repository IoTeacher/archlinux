# archlinux
117 pasos para instalar ArchLinux basado en tutorial para MacOS parallels 2023


```bash


Installing Arch Linux on Mac in Parallels
In January of 2020, I had need to set up an Arch Linux GUI environment on my MacBook Pro in Parallels. I knew virtually nothing about Linux, or operating systems in general for that matter. Every computer I had ever used came with an OS. That was pretty much all I knew about them. It was so ridiculously difficult that I figured other people out there may be struggling with the same problem, and they may benefit from my own experience.

I just followed your 177-step Arch install guide... and I give you praise because it's the only one that worked for me.. but now I think I need to combine tylenol, ibuprofen and a 30 minute break. Mike

Below you'll find a comprehensive, step-by-step guide for exactly how I set up Arch Linux on my MacBook Pro in Parallels. There won't be much in the way of explanation of what any of it means. Just the steps laid out. This is mostly because I don't really know what some of it means. But at the foot of this page you can find some good resources that will tell you what a lot of it means if that interests you. I myself just wanted it up and running, with a GUI, so if you're in those same shoes, just follow this step-by-step, word-for-word, and hopefully it will work for you as well.

Instructions

Download the latest Arch ISO. This guide is based on the following image:
https://mirror.rackspace.com/archlinux/iso/2020.01.01/
Open Parallels Control Center.
Select File > New.
Choose “Install Windows or another OS from a DVD or image file.”
Continue.
Choose Manually.
Drag the ISO you just downloaded onto the “Drag the image file here” spot. It will say “Unable to detect operating system.”
Continue.
Select “More Linux” > “Other Linux.”
Ok.
Choose a name for your new virtual device, like “Arch.”
Check the “Customize settings before installation” box.
Create.
In the configuration box, select “Options.”
Select “Sharing” in the top menu.
Change “Home folder only” to “All disks.”
Check the “Shared Profile” box.
Select “Hardware” in the top menu.
Move the Memory slider to 4GB.
Close the Configuration window. Can we be rational if there is no God?
If there is no reason behind your own mental processes, then there is no reason behind your own conclusions. So whose reason is behind your own mental processes?
Continue.
When the VM boot window displays, instead of selecting any of the options, hit the TAB button on your keyboard.
Hit the spacebar once and then type: “cow_spacesize=10G”
Enter.
The command prompt should display. Type in the following (without the hashtag at the front):
# nano /etc/pacman.d/mirrorlist

Follow the nano editor’s usage directions (which should display at the bottom of the screen) in order to move about five local mirrors (mirrors that are local to your location) to the top of the list. This way, when stuff downloads to your machine, it won't download all the way from the other side of the universe, which would take a really long time. Arrows move your cursor, CTRL-k cuts a line, CTRL-u pastes it. When you’re done, CTRL-x attempts to close the nano editor. Hit SHIFT-Y to say that you would like to save, and ENTER finishes.
Now we're going to partition the hard drive:
# gdisk /dev/sda
# p
 
# n
[ENTER]
[ENTER]
# +1G
[ENTER]
 
# n
[ENTER]
[ENTER]
# +5G
# 8200
 
# n
[ENTER]
[ENTER]
# +20G
[ENTER]
 
# n
[ENTER]
[ENTER]
[ENTER]
[ENTER]
 
# w
# Y
 
# mkfs -t ext4 /dev/sda1
# mkfs -t ext4 /dev/sda3
# mkfs -t ext4 /dev/sda4
 
# mkswap /dev/sda2
# swapon /dev/sda2
 
# mount /dev/sda3 /mnt
# cd /mnt
# mkdir boot home
# mount /dev/sda1 boot
# mount /dev/sda4 home
 
# nano /etc/resolv.conf

Add these nameservers to the top of the nameserver list:
nameserver 8.8.8.8
nameserver 8.8.4.4

CTRL-x. SHIFT-Y. Enter. Does the Problem of Evil prove that God isn't real?
The Problem of Evil argument cannot be presented without engaging in some kind of logical fallacy. The Palpatine Theodicy displays one... problem... with the Problem of Evil.
Now look up a thing that we're going to need to reference:
# ip link

Using the second set of data displayed, find the correct service domain and put it into the following line.
# sudo systemctl enable dhcpcd@enp0s5.service
# ping google.com

You should get a response from Google. If you don’t, you can try these three commands:
# sudo systemctl start dhcpcd
# sudo systemctl enable dhcpcd
# sudo dhcpcd

Once you are able to get a response from Google when pinging, continue.
# pacstrap -i /mnt base base-devel
# genfstab -p /mnt >> /mnt/etc/fstab
# more /mnt/etc/fstab
# pacstrap -i /mnt syslinux gptfdisk linux linux-headers nano networkmanager linux-firmware dhcpcd
# arch-chroot /mnt
# nano /etc/locale.conf

Type the following into the nano editor.
LANG="en_US.UTF-8"

CTRL-x. SHIFT-Y. Enter. Now make sure it knows what language you speak.
# nano /etc/locale.gen

Uncomment the following lines by removing the hashtags in front of them (because of course you live in the US and speak English):
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1

CTRL-x. SHIFT-Y. Enter. Now set your time clock. If you don't live on the US east coast, stop typing after "zoneinfo/" and hit TAB a few times and see what happens:
# locale-gen
# ln -s /usr/share/zoneinfo/America/New_York /etc/localtime
# nano /etc/hostname

Type in the one-word, lowercase name of your VM. It can be absolutely anything you want, but you should probably make it something meaningful. Maybe “archlinux”. I’m going to assume you used “archlinux” and show you how to set up your hosts file if you did. If you didn’t, set up the hosts file with the name you actually used.
CTRL-x. SHIFT-Y. Enter. How could time itself start?
The Author Analogy easily explains many difficult questions about life, the universe, theology, religion, philosophy, epistemology, and the reality we find ourselves in.
Now change the hosts file.
# nano /etc/hosts

Type in the following with your VM name to the nano editor.
127.0.0.1 localhost
127.0.1.1 archlinux.localdomain archlinux

CTRL-x. SHIFT-Y. Enter. Now update some things and change some stuff:
# syslinux-install_update -i -a -m
# cd /boot/syslinux
# cp /usr/lib/syslinux/bios/menu.c32 .
# cp /usr/lib/syslinux/bios/vesamenu.c32 .
# cp /usr/lib/syslinux/bios/chain.c32 .
# cp /usr/lib/syslinux/bios/hdt.c32 .
# cp /usr/lib/syslinux/bios/reboot.c32 .
# cp /usr/lib/syslinux/bios/poweroff.c32 .
# cp /usr/lib/syslinux/bios/libutil.c32 .
# cp /usr/lib/syslinux/bios/libcom32.c32 .
# mkinitcpio -p linux
# passwd

Type in the new password for the root account (not your regular user account) for the new Arch Linux VM. Then we'll do more cool stuff:
# exit
# cd /
# umount /mnt/boot
# umount /mnt/home
# swapoff /dev/sda2
# umount /mnt
# sgdisk /dev/sda --attributes=1:set:2
# reboot

Let the system reboot. When it’s complete, it will ask you to log in. Log in as "root."
# root

Type in the root password that you created a couple of steps ago. Now update your mirrorlist again:
# nano /etc/pacman.d/mirrorlist

Move the local mirrors to the top of the list again.
CTRL-x. SHIFT-Y. Enter. Then edit your nameservers again:
# nano /etc/resolv.conf

Add these nameservers to the top of the nameserver list:
nameserver 8.8.8.8
nameserver 8.8.4.4

CTRL-x. SHIFT-Y. Enter. Then look up that thing again:
# ip link

Using the second set of data displayed from that command above, find the correct service domain and put it into the following line.
# sudo systemctl enable dhcpcd@enp0s5.service
# ping google.com
# dhcpcd
# ping google.com

Trouble staying pure online?
Accountable2You can help you find peace of mind and rebuild trust with detailed, real-time accountability software. Available for Arch Linux as well as other major operating systems.
Hopefully, you’re receiving data from Google. Next, create your own user account. We'll pretend your name is Josh because... why wouldn't it be?
# useradd --home-dir /home/josh --create-home josh
# passwd josh
# nano /etc/sudoers

Once the nano editor opens, find the line that reads:
root ALL=(ALL) ALL

Under that line, add the following line just like it:
josh ALL=(ALL) ALL

CTRL-x. SHIFT-Y. Enter. Now do some more stuff:
# exit
# ip link show
# sudo systemctl enable dhcpcd@enp0s5.service
# sudo nano /etc/resolv.conf.head

Add these nameservers to the empty text file:
nameserver 8.8.8.8
nameserver 8.8.4.4

CTRL-x. SHIFT-Y. Enter. Now install a bunch more stuff and then reboot:
# sudo pacman -S xorg-server xorg-xinit xorg-apps
# sudo pacman -S xorg-iceauth xorg-sessreg xorg-xcmsdb xorg-xbacklight xorg-xgamma xorg-xhost xorg-xinput xorg-xmodmap xorg-xrandr xorg-xrdb xorg-xrefresh xorg-xset xorg-xsetroot mesa-libgl xterm
# sudo pacman -S xf86-video-vesa
# sudo pacman -S xfce4 xfce4-goodies sddm
# sudo reboot
# ping google.com

Hopefully, that last ping -- the one after reboot -- just worked right off-the-bat and you're getting data from google.com. Install some more stuff and then test GUI functionality:
# sudo pacman -S xorg-twm xorg-xclock
# startx

Here you can see a very simplistic GUI. In the first terminal, type the following:
# exit

You should be out of the GUI again now. Install some more stuff and restart:
# sudo pacman -S ttf-liberation noto-fonts ttf-roboto ttf-anonymous-pro
# sudo pacman -S ttf-hack ttf-inconsolata noto-fonts-emoji powerline-fonts
# sudo pacman -S adobe-source-code-pro-fonts ttf-fira-mono ttf-fira-code
# sudo pacman -S ttf-ubuntu-font-family ttf-dejavu ttf-freefont
# sudo pacman -S ttf-droid terminus-font ttf-font-awesome
# sudo pacman -S gnome gnome-extra
# sudo systemctl enable gdm
# sudo reboot

The new GUI should automatically boot up. This should happen from now on. Open terminal again manually in your GUI. Install some important apps:
# sudo pacman -S chromium
# sudo pacman -S firefox
# sudo pacman -S opera
# sudo pacman -S flashplugin
# gsettings set org.gnome.desktop.wm.preferences button-layout ":minimize,maximize,close"

Still in the terminal, clean up some font cache problems:
# sudo rm /var/cache/fontconfig/*
# rm ~/.cache/fontconfig/*

Now install Parallels Tools so you can copy between your operating systems and so you can share directories and stuff. Parallels Tools just worked right for me first try, so I will just link to another source for instructions for installing Parallels Tools.
In the future, to run a full system update, do this in the terminal:
# pacman -Syu


```
