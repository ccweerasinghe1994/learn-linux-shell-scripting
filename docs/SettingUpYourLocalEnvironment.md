- [🔥 Technical requirements](#-technical-requirements)
- [☄️ Choosing between a virtual machine and a physical installation](#️-choosing-between-a-virtual-machine-and-a-physical-installation)
- [💧 Setting up VirtualBox](#-setting-up-virtualbox)
- [🫧 Creating an Ubuntu virtual machine](#-creating-an-ubuntu-virtual-machine)
  - [⚡ Creating the virtual machine in VirtualBox](#-creating-the-virtual-machine-in-virtualbox)
  - [⛄ Installing Ubuntu on the virtual machine](#-installing-ubuntu-on-the-virtual-machine)
  - [🍄 Accessing the virtual machine via SSH](#-accessing-the-virtual-machine-via-ssh)
- [⛄ Summary](#-summary)
- [🦀 Questions](#-questions)
- [🦑 Further reading](#-further-reading)
### 🔥 Technical requirements
To complete the exercises in this chapter (and the following
chapters), you will need either a PC or laptop with at least 2 GHz
of CPU power, 10 GB of hard disk space, and about 1 GB of RAM
to spare. Pretty much all hardware created in the last 5 years
should suffice.
### ☄️ Choosing between a virtual machine and a physical installation
A virtual machine is an emulation of a physical machine. This
means it runs inside a physical machine, as opposed to directly
on the hardware. A physical machine has direct access to all
hardware, such as the CPU, the RAM, and other devices such as
the mouse, the keyboard, and the monitor. It is, however,
impossible to share the CPU or the RAM between multiple
physical machines. Virtual machines do not directly get access to
hardware, but through an emulation layer, which means
resources can be shared between multiple virtual machines.

Because we're discussing Bash shell scripting in general, in
theory it does not matter what kind of an installation is
performed. As long as that installation runs a compatible Linux
operating system with Bash 4.4 or later, all exercises should
work. There are, however, many advantages to using a virtual
machine over a physical installation:

- There is no need to remove your current preferred
operating system, or set up a complicated dual-boot
configuration
- Virtual machines can be snapshotted, which allows
recovery from critical failures
- You are able to run (many) different operating systemson a single machine

Unfortunately, there are also drawbacks associated with virtual
machine use:

- Because you're running a virtual operating system within
an already running operating system, there is some
overhead from the virtualization (in comparison to
running a bare-metal installation)
- Since you're running multiple operating systems at the
same time, you will need more resources than with a
bare-metal installation

In our opinion, modern computers are fast enough to make the
drawbacks almost trivial, while the advantages provided by
running Linux in a virtual machine are very helpful. Because of
this, we will only be explaining a virtual machine setup in the
rest of this chapter. If you feel confident enough to run Linux as
a physical installation (or perhaps you already have Linux
running somewhere!), feel free to explore the rest of the book
with that machine.

```
You might have a Raspberry Pi or another single-board computer
running Linux in your house from a previous project. While these
machines are indeed running a Linux distribution (Raspbian), they are
probably running it on a different architecture: ARM instead of x86.
Because this can cause unexpected results, we recommend only using
x86 devices for this book.
```
If you want to be sure all examples and exercises work as seen in
this book, run an Ubuntu 18.04 LTS virtual machine in
VirtualBox with the recommended specifications of 1 CPU, 1 GB
RAM, and a 10 GB hard disk: this setup is described in the rest of

this chapter. Even if many other types of deployment should
work, you would not want to be banging your head against the
wall for hours when an exercise is not working, before
discovering it was caused by your setup
### 💧 Setting up VirtualBox
To use virtual machines, we need software called a hypervisor.
A hypervisor manages resources between the host machine and
the virtual machines, provides access to disks, and has an
interface to manage it all. There are two different types of
hypervisors: type-1 and type-2. Type-1 hypervisors are the socalled bare-metal hypervisors. These are installed instead of a
regular operating system such as Linux, macOS, or Windows,
directly on the hardware. These types of hypervisors are used for
corporate servers, cloud services, and so on. For this book, we
will use a type-2 hypervisor (also called hosted hypervisors):
these are installed within another operating system, as a piece of
software not much different than, for example, a browser.

There are many type-2 hypervisors. The most popular choices at
the time of writing are VirtualBox, VMware workstation player,
or OS-specific variants such as QEMU/KVM on Linux, Parallels
Desktop on macOS, and Hyper-V on Windows. Because we are
going to use a virtual machine throughout this book, we do not
assume anything about the host machine: you should work
comfortably with whatever operating system you prefer. Because
of this, we've chosen to use VirtualBox as our hypervisor, since it
runs on Linux, macOS, and Windows (and even others!).
Furthermore, VirtualBox is free and open source software, which
means you can just download and use it.

Presently, VirtualBox is owned by Oracle. You can download the
installer for VirtualBox from https://www.virtualbox.org/. Installation
should not be hard; follow the instructions by the installer.

`
After installing a type-2 hypervisor such as VirtualBox, be sure to restart your computer. Hypervisors often need some kernel modules loaded,
which is easiest to achieve by rebooting
`
### 🫧 Creating an Ubuntu virtual machine
In this book, we're scripting with Bash, which means we do not
need a GUI for our Linux installation. We have chosen to use
Ubuntu Server 18.04 LTS as the virtual machine operating
system, for a number of reasons:
- Ubuntu is considered a beginner-friendly Linux
distribution
- The 18.04 is a Long-Term Support (LTS) release,
which means it will receive updates until April 2023
- Because an Ubuntu server offers a CLI-only installation,
it is easy on system resources and representative of reallife servers

At the time of writing, Ubuntu is maintained by Canonical. You
can download the ISO image from https://www.ubuntu.com/download/serve
r. Download the file now, and remember where you save this file,
since you'll need it soon.

`
Should the preceding download link no longer work, you can go to your
favorite search engine and search for Ubuntu Server 18.04 ISO download. You
should find a reference to the Ubuntu archives, which will have the
required ISO.
`
#### ⚡ Creating the virtual machine in VirtualBox
First, we will start with creating the virtual machine to host our
Ubuntu installation:

1. Open VirtualBox and choose Machine | New in the menu
toolbar.
2. For reference, we have circled the Machine entry in the
menu toolbar given in the following screenshot. Choose a
name for the virtual machine (this can be a different
name than the server name, but we like to keep it the
same for simplicity), set the Type to Linux, and Version
to Ubuntu (64-bit). Click Next:
3. On this screen, we determine memory settings. For most
servers, 1024 MB of RAM is a great start (and is
recommended by VirtualBox for virtual machines as
well). If you have beefy hardware, this can be set to 2048
MB, but 1024 MB should be fine. Make your selection
and press Next:
4. Once again, the recommended values by VirtualBox are
perfect for our needs. Press Create to start the creation of
the virtual hard disk:
5. Virtual hard disks can be many different types.
VirtualBox defaults to its own format, VDI, as opposed
to VMDK, which is the format used by VMware (another
popular virtualization provider). The last option is VHD
(Virtual Hard Disk), which is a more generic format
usable by multiple virtualization providers. Since we'll be
using VirtualBox exclusively in this book, keep the
selection on VDI (VirtualBox Disk Image) and press
Next:
6. We have two options on this screen: we can allocate the
full virtual hard disk on the physical hard disk right away,
or we can use dynamic allocation, which does not reserve
the full size of the virtual disk, but only what is used.
The difference between these options is often most
relevant in situations where many virtual machines are
running on a single host. Creating a total of disks larger
than what is physically available, but assuming not all
disks will be fully used, allows us to place more virtual
machines on a single machine. This is called
overprovisioning, and will only work if not all disks are
filled up (because we can never have more virtual disk
space than we have physical disk space). For us, this
distinction does not matter since we'll be running a single
virtual machine; we keep the default of Dynamically allocated and go to the next screen:
7. On this screen, we can do three things: name the virtual
disk file, select the location, and specify the size. If you
care about the location (it defaults to somewhere in your
home/user directory), you can press the circled icon in the
following screenshot. For the name, we like to keep it the
same as the virtual machine name. Lastly, a size of 10 GB
is sufficient for the exercises in this book. After you've set
up the three values, press Create. Congratulations, you've
just created your first virtual machine, as demonstratedin following 
screenshot:
8. However, before we can start the installation of Ubuntuon our virtual machine, we need to do two more things:
point the virtual machine to the installation ISO, and set
up the networking. Select the newly created virtual
machine and click Settings. Navigate to the Storage
section:
You should see a disk icon with the word Empty (in the
location circled on the left in the preceding screenshot).
Select it and mount an ISO file by clicking the select disk
icon (circled on the right), choose virtual optical disk file,
and then select the Ubuntu ISO that you downloaded
earlier. If you do this correctly, your screen should
resemble the preceding screenshot: you no longer see the
word Empty next to the disk icon and the Information
section should be filled in.
9. Once you have verified this, go to the Network section.
10. The configuration should default to the NAT type. If not,
set it to NAT now. NAT stands for Network Address
Translation. In this mode, the host machine acts as a
router for the virtual machines. Finally, we're going to set
up some port forwarding so we can use SSH tooling later
on. Click on the Port Forwarding button:
11. Set up the SSH rule just as we have done. This means that
port 22 on the guest (which is the virtual machine) is
exposed as port 2222 on the host (which is, surprise, the
host machine). We've chosen port 2222 for two reasons:
ports lower than 1024 require root/administrator
permissions, which we might not have. Secondly, there is
a chance an SSH process is already listening on the host
machine, which would mean VirtualBox would not be
able to use that port:
With this step, we've finished setting up the virtual machine!
#### ⛄ Installing Ubuntu on the virtual machine
Now you can start your virtual machine from the VirtualBox
main screen. Right click on the machine, select Start followed
by Normal Start. If all goes well, a new window will pop up,
showing you the virtual machine console. After a while, you
should see the Ubuntu server installation screen in that window:
1. On the screen shown in the following screenshot, select
your favorite language using the arrow keys (we're using
English, so if you're not sure, English is a good choice)
and press Enter:
2. Select the keyboard layout that you're using. If you're
unsure, you can use the interactive Identify keyboard
option to determine which layout is best for you. Once
the proper layout is set, move the focus to Done and press
Enter:
3. We now choose the type of installation. Because we're
using the server ISO, we do not see any options related to
the GUI. In the preceding screenshot, select Install
Ubuntu (both other options use Canonical's Metal-AsA-Server (MAAS) cloud offering, which is not relevant
for us) and press Enter:
4. You will see the Network connections screen. The
installer should default to using DHCP on the default
network interface that was created with the virtual
machine. Verify that an IP has been allocated to this
interface, and press Enter:
5. The Configure proxy screen is not relevant to us (unless
you're running with a proxy setup, but there's a good
chance you do not need our assistance with the
installation in that case!). Leave the Proxy address blank
and press Enter:
6. Sometimes it's helpful to manually partition your Linux
disk to fit specific needs. In our case, the default value of
Use An Entire Disk is a great fit, so press Enter:
7. After having selected that we want to use an entire disk,
we need to specify which disk to use. Since we only
created one disk when we configured the virtual machine,
select it and press Enter.
8. Now you will encounter a warning about performing a
destructive action. Because we are using an entire
(virtual!) disk, all information present on that disk will be
erased. We created this disk when we created the virtual
machine, so it does not contain any data. We can safely
perform this action, so select Continue and press Enter:
9. For the Filesystem setup, once again the default values
are perfect for our needs. Verify that we have at least 10
GB of hard disk space (it might be a little less, like
9.997 GB in the following example: this is fine) and press
Enter:
10. The Ubuntu server should now start installing to the
virtual disk. In this step, we'll be setting the server name
and creating an administrative user. We've chosen the
server name ubuntu, the username reader, and the
password password. Note that this is a very weak password
that we will only use on this server for simplicity. This is
acceptable because the server will only ever be accessible
from our host machine. When configuring a server that
accepts incoming traffic from the internet, never use a
password as weak as this! Choose anything you like, as
long as you can remember it. If you're unsure, we'd
advise using the same values of ubuntu, reader, and password:
Now that you've chosen a server name and configured an
administrative user, press Enter to finalize the
installation.
11. Depending on how long it took to complete the previous
screen and how fast the host machine is, Ubuntu will
either still be installing or will have finished already. If
you still see text moving around the screen, the
installation is still running. Once the installation is
completely finished, you will see the Reboot Now buttonappear. Press Enter:
12. After a few seconds, a message stating Please remove theinstallation medium, then press Enter should appear. Follow the
instructions and, if all goes well, you should be greeted
with a terminal login prompt:
Normally, VirtualBox is intelligent enough to try a second boot from the
hard disk instead of the ISO. If, after the previous steps, a reboot sends
you back to the installation menu, power down the virtual machine from
the VirtualBox main screen. Right-click on the machine, select Close
followed by Power Off. After it's fully powered down, edit the machine
and remove the ISO. This should force VirtualBox to boot from the disk,
which contains your Ubuntu Server 18.04 LTS installation.
13. Now the moment of truth: try to log in with your created
username and password. If successful, you should see a
screen similar to the following:

Give yourself a pat on the back: you have just created a virtual
machine, installed Ubuntu Server 18.04 LTS, and logged in via
the Terminal console. Well done! To exit, type exit or logout and
press Enter.
#### 🍄 Accessing the virtual machine via SSH
We have successfully connected to the Terminal console
provided to us by VirtualBox. However, this Terminal connection
is really basic: for example, we can't scroll up, we can't paste
copied text, and we do not have colored syntax highlighting.
Fortunately for us, we have a nice alternative: the Secure Shell
(SSH) protocol. SSH is used to connect to the running shell on
the virtual machine. Normally, this would be done over the
network: this is how enterprises maintain their Linux servers. In
our setup, we can actually use SSH within our host machine,
using the power forwarding we set up earlier.

If you followed the installation guide, port 2222 on the host
machine should be redirected to port 22 on the virtual machine,
the port where the SSH process is running. From a Linux or
macOS host machine, we can connect using the following
command (substitute the username or port number if
necessary):

```bash
🔥 -> ~ ssh reader@localhost -p 2222
```

However, there is a good chance you're running Windows. In
that case, you will probably not have access to a native SSH
client application within Command Prompt. Luckily, there are
many good (and free!) SSH clients. The simplest and most wellknown client is PuTTY. PuTTY was created in 1999 and, while
it's definitely a very stable client, its age is starting to show. Wewould recommend some newer SSH client software, such as
MobaXterm. This provides you with more session
management, a better GUI, and even a local Command Prompt!

Whichever software you decide on, be sure to use the following
values (again, change the port or username if you deviated from
the installation guide):

`If you're using SSH to connect to your virtual machine, you can start it
headless. When you do this, VirtualBox will not create a new window
with the Terminal console for you, but instead runs the virtual machine
in the background where you can still connect via SSH (just like what
happens on actual Linux servers). This option, Headless Start, is found
right below the earlier Normal Start, when right clicking on the
machine and selecting Start.`
### ⛄ Summary
In this chapter, we have started preparing our local machine for
the rest of the book. We now know about the differences between
virtual and physical machines, and why we prefer to use a virtual
machine for the remainder of this book. We've learned about the
two different types of hypervisors. We have installed and
configured VirtualBox with a virtual machine, on which we have
installed the Ubuntu 18.04 operating system. Finally, we have
connected to our running virtual machine using SSH instead of
the VirtualBox terminal, which affords better usability and
options.

The following command was introduced in this chapter: ssh and
exit.

In the next chapter, we will finish setting up our local machine
by looking at some different tools we can use that will help us
with bash scripting, both on the GUI and on the virtual machine
CLI.

### 🦀 Questions
1. What are some of the reasons running a virtual machine
would be preferable to a bare-metal installation?
2. What are some of the downsides of running a virtual
machine as opposed to a bare-metal installation?
3. What is the difference between a type-1 and type-2
hypervisor?
4. In which two ways can we start a virtual machine on
VirtualBox?
5. What makes an Ubuntu LTS version special?
6. What should we do if, after the Ubuntu installation, the
virtual machine boots to the Ubuntu installation screens
again?
7. What should we do if we accidentally reboot during
installation, and we never end up at the Ubuntu
installation (but instead see an error)?
8. Why did we set up NAT forwarding for the virtual
machine?
### 🦑 Further reading
Further reading
The following resources might be interesting if you'd like to go
deeper into the subjects of this chapter:
- Getting Started with Oracle VM VirtualBox by
Pradyumna Dash, Packt: https://www.packtpub.com/virtualization
-and-cloud/getting-started-oracle-vm-virtualbox
- Mastering Ubuntu Server – Second Edition by Jay
LaCroix, Packt: https://www.packtpub.com/networking-and-servers/mas
tering-ubuntu-server-second-edition