- [🐻‍❄️ Introduction](#️-introduction)
  - [🐼 What is Linux?](#-what-is-linux)
  - [🐨 What is Bash?](#-what-is-bash)
  - [🐻 Summary](#-summary)

## 🐻‍❄️ Introduction
### 🐼 What is Linux?
****
Linux is a generic term that refers to different open source
operating systems that are based on the Linux kernel. The Linux
kernel was originally created by Linus Torvalds in 1991, and
open sourced in 1996. A kernel is a piece of software that is
designed to act as an intermediate layer between low-level
hardware (such as the processor, memory, and input/output
devices) and high-level software, such as an operating system.
Apart from the Linux kernel, most Linux operating systems rely
heavily on ```GNU``` project utilities; for example, the Bash shell is a
```GNU``` program. Because of this, Linux operating systems are
referred to by some as ```GNU```/Linux. The ```GNU``` project, where
```GNU``` stands for ```GNU```'s `Not Unix!` (a recursive acronym), is a
collection of free software, a lot of which is found in most Linux
distributions. This collection includes many tools, but also an
alternative kernel called ```GNU``` HURD (which is not as
widespread as the Linux kernel).

Why do we need a kernel? Since a kernel sits between hardware
and the operating system, it provides an abstraction for
interacting with hardware. This is why the Linux ecosystem has
grown so large: the kernel can be used freely, and it handles a lot
of low-level operations on many types of hardware. Creators of
an operating system can therefore spend their time making an
easy-to-use, beautiful experience for their users, instead of
having to worry about how the users' pictures are going to be
written to the physical disk(s) attached to the system.

The Linux kernel is so-called `Unix-like` software. As you might
suspect, this implies that it is similar to the original Unix kernel,which was created between 1971 and 1973 at Bell Labs, by Ken
Thompson and Dennis Ritchie. However, the Linux kernel is
only based on Unix principles and does not share code with Unix
systems. Famous Unix systems include the BSDs (FreeBSD,
OpenBSD, and so on) and macOS.

Linux operating systems are broadly used for one of two
purposes: as a desktop or as a server. As a desktop, Linux can
serve as a replacement for the more commonly used Microsoft
Windows or macOS. However, most Linux usage is accounted for
the server landscape. At the time of writing, it is estimated that
around 70% of all servers on the internet use a Unix or `Unix-like`
operating system. The next time you're browsing the news,
reading your mail, or are scrolling through your favorite social
media website, remember that there's a big chance the pages you
are being shown have been processed by one or more Linux
servers.

There are many distributions, or flavors, of Linux. Most Linux
operating systems fall within distribution families. A distribution
family is based on a common ancestor, and often use the same
package management tools. One of the more well-known Linux
distributions, `Ubuntu`, is based on the `Debian` distribution
family. Another prominent Linux distribution, `Fedora`, is based
on the `Red Hat` family. Other notable distribution families
include `SUSE, Gentoo,` and `Arch`.

![](./img/2.png)
[original diagram](https://whimsical.com/linux-9F2aeMv7fBPsQnsR5WoBrg@LUSUr8hW4UQFJJAr6P)

Not many people realize how many devices run the Linux kernel.
For example, the most common smartphone operating system in
use today, Android (with a market share of around 85%), uses a
modified version of the Linux kernel. The same goes for many
smart TVs, routers, modems, and various other (embedded)
devices. If we were to include Unix and other `Unix-like` software,
we can safely say that most of the devices in the world run on
these kernels!

### 🐨 What is Bash?
****
The most commonly used shell in Linux systems is the `Bourne again shell`, or Bash. The Bash shell is based on the Bourne
`shell`, known as `sh`. But what is a `shell`?

A shell is, in essence, a user interface. Most often, it is used to
refer to a text-based interface, also called a `command-line
interface (CLI)`. However, it is called a shell because it can be
seen as a shell around the kernel; this means that it applies not
just to CLIs, but just as well to `graphical user interfaces`
`(GUIs)`. When we refer to a shell in this book, we are talking
about a CLI, and unless stating differently, we're talking about
the Bash shell.

The purpose of a shell, both CLI and GUI, is to allow the user to
interact with the system. After all, a system that does not offer
interaction would be hard to justify, not too mention hard to use!
Interaction in this context means many things: typing on your
keyboard will make letters appear on your screen, moving your
mouse will change the location of the cursor on screen, giving the
command to delete a file (either with a CLI or GUI) will remove
the bytes from the disk, and so on.

In the earliest days of Unix and computers, no GUIs were
available, so all work was performed via a CLI. To connect to the
shell on a running machine, a `video terminal` was used: often
this would be a very simple monitor combined with a keyboard,
which was connected with, for example, a RS-232 serial cable.
Commands entered on this `video terminal` were processed by the
shell running on the Unix machine.

Luckily for us, things have changed quite a bit since the first
computers. Today, we no longer use dedicated hardware to
connect to shells. A piece of software running in a GUI, a
`terminal emulator`, is used for interaction with the shell. Lets
take a quick look at how connecting to a Bash shell with a
`terminal emulator` can look:

![ubuntu server login screen](./img/1.png)

In the preceding screenshot, we're connected to a Linux virtual
machine (we'll be setting this up in the next chapter), using a
`terminal emulator` (GNOME Terminal) via the `Secure Shell
(SSH)` protocol. A few interesting things to note:

- We're on a CLI interface; we do not have access to, nor do
we need, a mouse
- We're connected to an Ubuntu machine, but we're
running this within another operating system (Arch
Linux, in this case)
- We're presented with a welcome message by Ubuntu
18.04, showing some general information about the
system

Besides using the Bash shell for direct system interaction, it
provides another important functionality: the ability to execute
multiple commands sequentially, with or without user
interaction, tailored to a specific goal. This might sound
complicated, but it's actually pretty simple: we're talking about
`Bash scripts`, the subject of this book!

### 🐻 Summary
****
In this chapter, you've read about the GNU/Linux operating
systems and the Linux kernel, what a kernel really is, and how
big an impact Linux distributions have on daily life. You've also
learned what a shell is, and that the most common Linux shell,
Bash, can be both used to interact with a Linux system, and is
also utilized to write shell scripts.
In the next chapter, we'll set up a local environment which we
will use throughout the rest of the book, in both the examples
and exercises.
