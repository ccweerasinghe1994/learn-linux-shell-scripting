- [Technical requirements](#technical-requirements)
- [Using graphical editors for shell scripting](#using-graphical-editors-for-shell-scripting)
  - [Atom](#atom)
    - [Atom installation and configuration](#atom-installation-and-configuration)
  - [Notepad++](#notepad)
- [Using command-line editors](#using-command-line-editors)
  - [Vim](#vim)

### Technical requirements
You will need the virtual machine we created in the previous
chapter when using Vim or nano. If you want to use Notepad++,
you will need a Windows host machine. For Atom, the host
machine can run either Linux, macOS, or Windows.

### Using graphical editors for shell scripting
Tooling has come a long way since the first Unix and Unix-like
distributions. In the earliest days, writing shell scripts was
significantly harder than today: the shells were less powerful,
text editors were command-line only and things such as syntax
highlighting and autocomplete were non-existent. Today, we
have very powerful GUI editors that will help us in our scripting
adventures. Why would we want to wait until we run a script to
find an error, when a GUI editor could have already shown us
the error in advance? Today, using an advanced editor for shell
scripting is almost a necessity that we wouldn't want to live
without.

We'll describe two text editors in the coming pages: Atom and
Notepad++. Both are GUI-based, which we can use for efficient
shell scripting. If you have a preference for either already, pick
that one. If you're unsure, we would recommend using Atom.

#### Atom
The first graphical editor we will consider is Atom, made by
GitHub. It's described as A hackable text editor for the 21st
Century. Hackable, in this sense, means that while the default
installation of Atom is as complete as any text editor, this
application really shines because it is very configurable and
extensible. Anything that has not been integrated by GitHub can
be written as an extension package. By using these extensions,
you can make your Atom installation fully your own; if you do
not like something, change it. If it can't be changed out of the
box, find a package that does it. And even if there's not a package
that does what you're hoping, you still have the option to create
your own package!

Another nice feature of Atom is the default integration with Git
and GitHub. Git is currently the most popular version control
system. Version control systems are used when writing code or
scripts. They ensure that history of files is preserved, and make it
possible for multiple, even many, contributors to work on the
same files at the same time, without getting burdened down by
conflict management. GitHub, as the name suggests, is currently
the most prominent web-based Git provider for open source
software.

The last great thing about Atom we want to mention is that, by
default, it supports many scripting and programming languages.
When we say supports, we mean that it can recognize file types
by their extensions, and offer syntax highlighting (which makes
for much easier scripting!). This functionality is provided
through core packages, which work the same way as normal

packages but are included from the start. For our purposes, the
core package, language-shellscript, will help us in our shell
scripting endeavors.

##### Atom installation and configuration
Let's go ahead and install Atom. As long as you're running Linux,
macOS, or Windows, you can go to https://atom.io/ and grab the
installer. Run the installer and, if necessary, follow along with
the prompts until Atom is installed. Now, start Atom and you'll
be greeted by the welcome screen, which at the time of writing
looks like the following:

Be sure to look at all the screens Atom has to offer. When you
feel like you've explored enough, let's add a package to Atom that
will complement our shell scripting. If you have the Welcome
Guide screen still open, select Install a Package from there.
Otherwise, you can use the keyboard shortcut Ctrl + , to bring up
the Settings screen. You will see an Install option there, which
will take you to the Install Packages screen. Search for bash, and
you should see the following package:

Click the Install button and watch the installation. You might be
prompted to reboot Atom after the install; be sure to do so. If
you're not prompted but see errors of any kind, a reboot of Atom
is never a bad idea. After installing the package, you will now
have autocomplete functionality when writing shell scripts. This
means that you can start typing and Atom will try to predict what
you want, in the following manner:

On the right-hand side, you can see we started typing the
echo shell command, and after the first two letters, Atom
presented us with two options that contain those two letters.
Once it makes a suggestion, we can press Enter and the
command is inserted fully. While it will not save much time in
this instance, it can be great for two main reasons:

- If you're unsure what the command is called exactly, you
might be able to find it with autocomplete.
- Once you start writing conditionals and loops (in the
second part of this book), the autocomplete will span
multiple lines, saving you from typing many words and
remembering all the syntax.

Finally, let's look at how Atom looks when you've got a Git
project open and are working on files:

When working in Atom, the screen will mostly look like this. On
the left-hand side, you'll see the Tree View, which you can
toggle on/off by pressing Ctrl + \. The Tree View contains all the
files in your current project (which is the directory you've
opened). All these files can be opened by double-clicking them,
which causes them to appear in the middle: the Editor View.
This is where you'll spend most of your time, working on the
shell scripts. The Editor View will always be visible, even if there
are currently no files open.

By default, there is one last view, the Git View, located on the
right-hand side. This view can be toggled by pressing
Ctrl +Shift + 9. The code for this book is hosted on GitHub,
which you will download (or, as Git calls it, clone) once, without
the need to edit it on the remote server. Because of this, the Git
View is not needed in this book, but we mention it since you will
probably use it for other projects.

#### Notepad++
Where Atom is closer to an integrated development
environment (IDE) than a text editor, Notepad++ is pretty
much what the name implies: good old Notepad with some
added features. Some of these added features include being able
to have multiple files open at the same time, syntax highlighting,
and limited autocomplete. It was initially released in 2003 and
only works on Windows.

Notepad++ is characterized by its simplicity. If you are familiar
with any kind of Notepad software (who isn't?), Notepad++
should be instantly recognizable. While we recommend using
Atom for this book, using a simple solution such as Notepad++
will definitely not hold you back. However, in a business
environment, you would almost always create scripts in an
already existing version-controlled repository, which is where
the added features of Atom really shine.

If you would like to check out Notepad++, grab it from https://note
pad-plus-plus.org/download and run the installer (remember, only if
you're on Windows!). Keep the default options and run
Notepad++ after the installation. You should be greeted by the
following screen:

As you can see, when you open a file ending in .sh, you will see
syntax highlighting. This is because the .sh extension is reserved
for shell script files. This can help you immensely when writing
scripts. The example of a missing quote messing up your script
will become really apparent with color-based syntax
highlighting, possibly saving you many minutes of
troubleshooting.

Notepad++ has many other features that make it a great
enhanced Notepad. You can use macros to perform scripted
tasks, you can install plugins to extend functionality, and there
are many more unique features that make Notepad++ an
attractive option

### Using command-line editors
Being able to use command-line editors is a skill anyone working
with Linux should learn sooner or later. For Linux installations
with a GUI, this might be substituted with a GUI tool such as
Atom or the distribution's built-in variant on Notepad. However,
server installations will almost never have a GUI and you will
have to rely on command-line text editors. While this might
sound daunting, it's really not! To give you a small introduction
to command-line editors, we'll go over two of the most popular
applications that are present on most Linux distributions: Vim
and GNU nano.

#### Vim
The first command-line text editor we will discuss is perhaps the
most popular for Linux: Vim. Vim is derived from the term Vi
Improved, as it is an updated clone of the Unix editor Vi. It was
created and is still maintained by Bram Moolenaar, who first
released Vim publicly in 1991. Vim (or, on very old systems, Vi)
should be present on all Unix or Unix-like machines you will
encounter.

Vim is considered a hard-to-learn tool. This is mainly caused by
the fact it works very differently from text editors that most
people are used to. However, once the initial learning curve is
over, many agree that a lot of actions can be done in Vim much
more quickly than in a normal text editor (such as Microsoft's
Notepad++).

Let's jump in! Log in to your virtual machine:

```bash
$ ssh chamra@192.168.163.131 -p 22
```
Once logged in, open Vim to an empty file:
```bash
chamara@ubuntu22:~$ vim
```
You should be greeted by something looking approximately like
the following:

![image](./img/3.png)
Vim starts a new process that uses your entire Terminal (don't
worry, everything will still be right where you left it once you exit
Vim!). When you start up Vim, you will be placed in normal
mode. Vim has a number of modes, of which normal and insert
are the most interesting to explore. In normal mode, you can't
just start typing like you would in Notepad or Word. Since Vim
was designed to be used without a mouse, it needed a way to
manipulate text as well. Where some applications decided on
using modifiers for this (holding the Shift key in Notepad for
example), Vim decided on modes. Let's first enter insert mode so
we can start to type some text. Press the I key, and your screen
should switch to insert mode: