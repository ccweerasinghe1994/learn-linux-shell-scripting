- [Technical requirements](#technical-requirements)
- [Read, write, and execute](#read-write-and-execute)
  - [RWX](#rwx)
- [Users, groups, and others](#users-groups-and-others)
- [Manipulating file permissions and ownership](#manipulating-file-permissions-and-ownership)
  - [chmod, umask](#chmod-umask)
  - [sudo, chown, and chgrp](#sudo-chown-and-chgrp)
    - [sudo](#sudo)
    - [chown, chgrp](#chown-chgrp)
### Technical requirements
We will explore the Linux permissions scheme using the virtual
machine we created in Chapter 2, Setting Up Your Local
Environment. During this chapter, we will add new users to this
system, but only having access as the first user (which has
administrative, or root privileges) is sufficient at this point.

### Read, write, and execute
In the previous chapter, we discussed the Linux filesystem and
the different types with which Linux implements the everything
is a file philosophy. However, we did not look at `permissions` on
those files. As you might have guessed, in a `multi-user` system
such as a `Linux server`, it is not a particularly great idea that
users can `access` files which are owned by other users. Where
would the `privacy` be in that?

The Linux permissions scheme is actually at the heart of the
Linux experience, as far as we are concerned. Just as (almost)
everything is handled as a `file` in `Linux`, all of those files have a
distinct set of permissions accompanying them. While exploring
the file system in the previous chapter, we limited ourselves to
files that were viewable by either everyone or by the `currently`
`logged` in `user`. However, there are many files that are only
`viewable` or `writable` by the `root` `user`: often, these are sensitive
files such as `/etc/shadow` `(which contains the hashed passwords for
all users)`, or files which are used when `starting the system`, such
as `/etc/fstab` `(which determines which file systems are mounted at
boot)`. If everyone could edit those files, it could result in an
`unbootable` system very quickly!

#### RWX
File `permissions` under Linux are handled by three attributes:
`read`, `write`, and `execute`, or `RWX`. While there are other
permissions (some of which we will discuss later in this chapter),
most interactions with regards to permissions will be handled by
these three. Even though the names seem to speak for
themselves, they behave differently with regards to `(normal)` files
and `directories`. The following table should illustrate this:

Allows the user to see the contents of the file with any command
that supports this, such as `vim`, `nano`, `less`, `cat`, and so on.

| Permission | On normal files                                                                                                                                                                            | On directories                                                                                                                                                                                                                                                                       |
| :--------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Read       | Allows the user to list the contents of the directory using the `ls` command.This will even list files in the directory on which the user has no other privileges!                         | Allows the user to list the contents of the directory using the `ls` command.This will even list files in directory on which the user has no other privileges!                                                                                                                       |
| Write      | Allows the user to make changes to the file.                                                                                                                                               | Allows the user to replace or delete files within the directory, even if the user has no direct permissions on that file. However, this does not include read permissions on all files within the directory!                                                                         |
| Execute    | Allows the user to execute the file. This is only relevant when the file is something that is supposed to be executed, such as a binary or script; otherwise, this attribute does nothing. | Allows the user to traverse into the directory by using cd. This is a separate permission from the listing of contents, but they are almost always used together; being able to list without being able to navigate into it (and vice versa) is mostly an ineffective configuration. |

This overview should provide a basis for the three different
permissions. Please take a good look and see whether you can
fully understand what is presented there.

Now, it's about to get a little more complicated. While these
permissions on both files and directories show what can and
cannot be done for a `user`, how does Linux deal with multiple
users? How does Linux keep track of `file` `ownership`, and how are
files shared by `multiple` `users`?

### Users, groups, and others

Under Linux, every file is `owned` by exactly `one` `user` and `one`
`group`. Every user has an `identifying` `number`, the `User ID`
`(UID)`. The same applies for a `group`: it is resolved by a `Group
ID` `(GID)`. Every `user` has exactly `one` `UID` and one `primary`
`GID`; however, users can be members of `multiple groups`. In that
case, the user will have one or more supplementary GIDs. You
can see this for yourself by running the id command on your
Ubuntu machine:

```bash
➜  ~ id
uid=1000(chamara) gid=1000(chamara) groups=1000(chamara),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd)
```
In the preceding output, we can see the following things:
- The `uid` for the chamara `user` is 1000; Linux typically starts
numbering `normal` `users` at `1000`
- The `gid` is 1004, which corresponds to the chamara `group`; by
`default`, Linux creates a `group` with the `same` `name` as the
`user` `(unless told specifically not to)`
- Other groups include adm, sudo, and others.

What does this mean? The `current logged-in user has a uid of 1000`,
a `primary gid of 1004`, and a few `supplementary groups`, which
makes sure that it has `other privileges`. For example, under
`Ubuntu`, the `cdrom` group allows the user to have `access to the disk
drive`. The `sudo` group allows the user to perform `administrative
commands`, and the `adm group` allows the user to `read
administrative files`.

*While we typically refer to users and groups by name, this is just a
representation for the `UIDs` and `GIDs` that Linux provides us with. On a
system level, only the `UID` and `GIDs` are `important` for `permissions`. This
makes it possible, for example, to have `two users with the same`
username but `different UIDs`: the permissions for those users will not be
the same. `The other way around is also possible: two different
usernames with the same UID`—this causes the permissions for both
users to be the same, at least on the `UID level`. However, both situations
are terribly confusing and should not be used! As we'll see later on, using
groups to share permissions is by far the best solution for sharing files
and directories*.

*Another thing to keep in mind is that `UIDs and GIDs` are `local` to the
`machine`. So if I have a user named `bob` with UID 1000 on `machine A`,
and UID 1000 is mapped to user `alice` on `machine B`, transferring bob's
files from machine A to machine B would result in the files being owned
by alice on system B!*

The `RWX` permissions explained previously relate to the users
and groups we're discussing now. In essence, every file (or
directory, which is just a different type of file), has the following
attributes:

- The file is owned by a user, which has (part of) the `RWX`
permissions
- The file is also owned by a `group`, which again, has (part
of) the `RWX` permissions
- The file finally has `RWX` permissions for others, which
means all different users that don't share the group

To determine if a user can read, write, or execute a file or
directory, we need to look at the following attributes (not necessarily in this order):

- Is the user the `owner` of the file? What `RWX` permissions
does the owner have?
- Is the user part of the `group` that owns the file? What
`RWX` permissions have been set for the group?
- Does the file have enough `permissions` on the `others`
`attribute`?

Let's look at some simple examples before it gets too abstract. On
your virtual machine, follow along with the following commands:

```bash
➜  ~ pwd
/home/chamara
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 22 03:12 nano-test.txt
-rw-rw-r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ touch nano-test.txt  
➜  ~ 
```

First, we ensure that we are in the home directory for the chamara
user. If not, we can move back there by using the `cd
/home/chamara` command or, `alternatively`, by just entering `cd` `(without
an argument`, `cd defaults to the user's home directory!`). We proceed
by listing the contents of the directory in the `long format`, using
`ls -l`, which shows us one file: `nano-test.txt`, from Chapter 2, Setting
Up Your Local Environment (don't worry if you didn't follow
along there and do not have the file; we'll be creating and
manipulating files in a little bit). We use a new command, `touch`,
to `create an empty file`. The argument we specify for `touch` is
interpreted as the `file name`, as we can see when we list the files
again:


```bash
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rw-rw-r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ 
```

You'll see the permission followed by two names: the `username`
and the` group name` (in that order!). For our `testfile`, the user
chamara and members of the chamara group can both read and write to
the file, but cannot execute (on the position of the x, there is
instead a -, indicating an absence of that permission). All other
users, such as those that are neither chamaras nor part of the
chamara group (which, in this case, is really all other users), can
only read the file due to the permission of others. This is also
described in the following table:

![](./img/19.png)

If a file had `full permissions` for everyone, it would look like this:
`-rwxrwxrwx`. For files that have all permissions for the `owner` and the
`group`, but none for others, it would be `-rwxrwx---`. `Directories`
with `full permissions on user and group`, but `none for others`, are
represented as `drwxrwx---`.

Let's look at another example:

```bash
 ls -l /
drwx------   4 root root  4096 Apr 10 00:52 root
dr-xr-xr-x 381 root root     0 Apr 23 03:59 proc
drwxr-xr-x  30 root root   880 Apr 24 01:16 run
➜  ~ 
```

The home directory for the systems' superuser is `/root/`. We can see
from the first character on the line that it is a `d`, for `directory`. It
has `RWX` (one last time: read, write, execute) permissions for the
owner root, and no permissions for the group (also root), nor for
others (as denoted by ---). These permissions can only mean one
thing: `only the user root can enter or manipulate this
directory`! Let's see if our assumption is correct. Remember,
`entering a directory requires the x permission`, while `listing the
directory contents the r permission`. We should not be able to do
either, since we're neither the root user or in the root group. In
this case, the permissions of others will be applied, this being `---:`

```bash
➜  ~ cd /root/
cd: permission denied: /root/
➜  ~ ls /root/
ls: cannot open directory '/root/': Permission denied
➜  ~ 
```
### Manipulating file permissions and ownership

After reading the first part of this chapter, you should have a
decent understanding of Linux file permissions, and how read,
write, and executed are used on a user, group, and other levels to
ensure that files are exposed exactly as required. However, up
until this point, we've been dealing with static permissions.
When administering a Linux system, you will most likely spend a
fair bit of time adjusting and troubleshooting permissions. In
this part of the book, we'll be exploring the commands we can
use to `manipulate` the `permissions` on `files`.

#### chmod, umask
Let's circle back to our `testfile`. It has the following permissions: `-
rw-rw----`. `Read/writable by user and group`, `readable by others`.
While these permissions might be fine for most files, they are
definitely not a great fit for all files. `What about private files?
You would not want those to be readable by everyone, perhaps
not even by group members`.

The Linux command to `change permissions` on a `file or directory`
is `chmod`, which we like to read as change file mode. `chmod` has two
operating modes: `symbolic mode` and `numeric/octal` mode. We
will begin by explaining `symbolic mode (which is easier to
understand)`, before we move to `octal mode (which is faster to
use)`.

*Something we have not yet introduced is the command to view manuals
for commands. The command is simply `man`, followed by the command for
which you'd like to see the manual of. In this case, `man chmod` will place us
into the `chmod manual pager`, which uses the same navigation controls as
you learned for Vim. Remember, quitting is done by entering `:q`. In this
case, just `q` is enough. Take a look at the chmod manual now and read at
least the description header; it will make the explanation that follows
clearer.*

`Symbolic mode` uses the `RWX` construct we saw before with the
`UGOA` letters. This might seem new, but it actually isn't! `Users`,
`Groups`, `Others`, and `All` are used to denote which permissions
we're changing.

To add permissions, we tell `chmod` who (`users`, `groups`, `others`, or
`all`) we are doing this for, followed by the permission we want to
add. `chmod u+x <filename>`, for example, will add the `execute`
permission for the `user`. Similarly, removing permissions with chmod is done as follows: `chmod g-rwx <filename>`. Notice that we use the
`+` sign to `add` permissions and the `-` sign to `remove` permissions.
If we do `not` specify `user, group, others, or all`, `all` is used by
`default`. Let's try this out on our Ubuntu machine:

```bash
➜  ~ cd
➜  ~ pwd
/home/chamara
➜  ~ ls -l 
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rw-rw-r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ chmod u+x test.txt 
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rwxrw-r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ chmod g-rwx test.txt 
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rwx---r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ chmod -r test.txt 
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
--wx------ 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ 
```
First, we added the `execute` permission for the `user` to the `testfile`.
Next, we `removed` `read`, `write`, and `execute` from the `group`,
resulting in `-rwx---r--`. In this scenario, group members are still
able to read the file, however, because everyone can still read
the file. Not the perfect permissions for privacy, to say the least.
Lastly, we do not specify anything before the `-r`, which effectively
removes read access for the user, group, and others, causing the
file to end up as `--wx-----`-.

Being able to `write` and `execute` a `file` you can't `read` is a bit weird.Let's fix it and look at how `octal permissions` work! We can use
the `verbose` option on `chmod` to make it print more information by
using the `-v` flag:
    
```bash
➜  ~ chmod -v u+rwx test.txt 
mode of 'test.txt' changed from 0300 (-wx------) to 0700 (rwx------)
➜  ~
```

As you can see, we now get output from `chmod`! Specifically, we can
see the `octal mode`. Before we changed the file, the mode was
`0300`, and after adding `read` for the `user`, it jumped up to `0700`. What
do these numbers mean?

It all has to do with the `binary implementation of the
permission`. For all three levels `(user, group, others)`, there are `8
different possible permissions when combining read, write, and
execute`, as follows:

| Symbolic | Octal |
| -------- | ----- |
| ---      | 0     |
| --x      | 1     |
| -w-      | 2     |
| -wx      | 3     |
| r--      | 4     |
| r-x      | 5     |
| rw-      | 6     |
| rwx      | 7     |

Basically, the `octal` value is between `0 and 7`, for a total of `8`
values. This is the reason it's called `octal`: from the `Latin/Greek`
representation of `8`, `octo`. The `read` permission is given the value
of `4`, `write` permission the value of `2`, and the `execute` permission
the value of `1`.

By using this system, the value of `0 to 7` can always be uniquely
related to an `RWX` value. `RWX` is `4+2+1 = 7`, `RX is 4+1 = 5`, and
so on.

Now that we know how octal representations work, we can use
them to modify the file permissions with chmod. Let's give the test
file full permissions (RWX or 7) for user, group, and others in a
single command:

```bash
➜  ~ chmod -v 0777 test.txt 
mode of 'test.txt' changed from 0700 (rwx------) to 0777 (rwxrwxrwx)
➜  ~
```
In this case, `chmod` accepts four numbers as the argument. The `first
number` is in regards to a special type of permission called the
`sticky bit`; we won't be discussing this, but we have included
material in the Further reading section for those interested. In
these examples, it is always set to `0`, so no `special bits` are set. The
`second number` maps to the `user permissions`, the `third` to `group`
permissions, and the `fourth`, unsurprisingly, to the `others`
permissions.

If we wanted to do this using `symbolic` representation, we could
have used the `chmod a+rwx` command. So, why is `octal faster` than, as
we said earlier on? Let's see what happens if we want to have different permissions for each level, for example, `-rwxr-xr--`. If we
want to do this with symbolic representation, we'd need to use
either three commands or one `chained` command (another
function of chmod):

```bash
➜  ~ chmod 0000 test.txt
➜  ~ ls -l              
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
---------- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ chmod u+rwx,g+rw,o+r test.txt 
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rwxrw-r-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ 
```

As you can see from the `chmod u+rwx,g+rx,o+r` testfile command,
things have gotten a bit complicated. Using `octal` notation,
however, the command is much `simpler`:

```bash
➜  ~ chmod 0000 test.txt 
➜  ~ ls -l
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
---------- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ chmod 0754 test.txt
➜  ~ ls -l              
total 8
-rw-rw-r-- 1 chamara chamara 29 Apr 24 02:07 nano-test.txt
-rwxr-xr-- 1 chamara chamara 81 Apr 23 01:55 test.txt
➜  ~ 
```
Basically, the difference is mainly using `imperative` notation
`(add or remove permissions)` versus `declarative` notation `(set it
to these values)`. In our experience, `declarative is almost always
the better/safer option.` With `imperative, we need to first check
the current permissions and mutate them; with declarative`, we can just specify in a single command exactly what we want.

*It might be obvious by now, but we `prefer` to use the `octal` notation.
Besides the benefits from shorter, simpler commands that are handled
`declaratively`, another benefit is that most examples you will find online
use the `octal` notation as well. To fully understand these examples, you
will need to at least understand `octals`. And, if you need to understand
them anyway, nothing beats using them in your day to day life!*.

Earlier, when we used the `touch` command, we ended up with a file
that could be `read` and `written` to by both the `user and group`, and
was `readable` to others. These seem to be default permissions,
but where do they come from? And how can we manipulate
them? Let's meet `umask`:

```bash
➜  ~ umask
002
➜  ~ 
```

The `umask session` is used to determine the file permissions for
`newly created files and directories`. For files, the following is
done: take the `maximum octal` value for files, `0666`, and `subtract`
the `umask (in this case, 0002)`, which gives us `0664`. This would mean
that newly created files are `-rw-rwr--`, which is exactly what we saw
for our `testfile`. Why do we take `0666` and not `0777`, `you might ask?`
This is a `protection that Linux provides`; if we were to use `0777`,
most files would be created as `executable`. `Executable files can be
dangerous`, and the design decision was made that files should
only be executable when explicitly set that way. So, with the
current implementation, there is no such thing as `accidentally
creating an executable file`. `For directories, the normal octal
value of 0777 is used`, which means that `directories` are created
with `0775, -rwxrwxr-x` permissions. We can check this out by creating
a new directory with the `mkdir` command:


```bash
➜  ~ mkdir testdir
➜  ~ ls -l
total 12
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
```

Because the execute permission on a directory is much less
dangerous `(remember, it is used to determine if you can move
into the directory)`, this implementation differs from files.

We have one last trick we'd like to showcase with regards to `umask`.
In specific cases, we'd like to determine default values for files
and directories ourselves. We can also do this using the umask
command:

```bash 
➜  ~ umask
002
➜  ~ umask 0007
➜  ~ umask
007
➜  ~ touch umaskfile
➜  ~ mkdir umaskdir
➜  ~ ls -l
total 16
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara chamara    0 Apr 24 04:27 umaskfile
➜  ~
```

In the preceding example, you can see that running the
`umask` command without arguments prints the current umask.
Running it with a valid umask value as an argument changes

umask to that value, which is then used when creating new files
and directories. Compare umaskfile and umaskdir with the earlier
testfile and testdir in the preceding output. This is very useful if
we want to create files that are private by default!
#### sudo, chown, and chgrp
So far, we have seen how we can manipulate the (basic)
permissions for files and directories. However, we haven't dealt
with changing either the `owner` or the `group` for a file. It would
be a little impractical to always have to work with users and
groups as they were at `creation time`. For Linux, we can use two
tools to `change the owner and group`: `change owner (chown)`
and `change group (chgrp)`. However, there is one very important
thing to note: these commands can only be executed for someone
with `root permissions (which will, typically, be the root user)`. So,
before we introduce you to `chown and chgrp`, let's look at `sudo`!
##### sudo
The `sudo` command was originally named for `superuser do`,
which, as the name implies, gives you a chance to perform an
action as the `root superuser`. The `sudo` command uses the
`/etc/sudoers file to determine if users are allowed to elevate to
superuser permissions`. Let's see how it works!

```bash
➜  ~ cat /etc/sudoers
cat: /etc/sudoers: Permission denied
➜  ~ ls -l /etc/sudoers
-r--r----- 1 root root 1671 Feb  8 08:41 /etc/sudoers
➜  ~ sudo cat /etc/sudoers
[sudo] password for chamara: 
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty

# This preserves proxy settings from user environments of root
# equivalent users (group sudo)
#Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"

# This allows running arbitrary commands, but so does ALL, and it means
# different sudoers have their choice of editor respected.
#Defaults:%sudo env_keep += "EDITOR"

# Completely harmless preservation of a user preference.
#Defaults:%sudo env_keep += "GREP_COLOR"

# While you shouldn't normally run git as root, you need to with etckeeper
#Defaults:%sudo env_keep += "GIT_AUTHOR_* GIT_COMMITTER_*"

# Per-user preferences; root won't have sensible values for them.
#Defaults:%sudo env_keep += "EMAIL DEBEMAIL DEBFULLNAME"

# "sudo scp" or "sudo rsync" should be able to use your SSH agent.
#Defaults:%sudo env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"

# Ditto for GPG agent
#Defaults:%sudo env_keep += "GPG_AGENT_INFO"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

@includedir /etc/sudoers.d
➜  ~ 
```

We first try to look at the contents of `/etc/sudoers` as a `normal user`.
When that gives us a Permission denied error, we look at the
permissions on the file. From the `-r--r----- 1 root root` line, it
becomes obvious that only the root user or members of the root
group can read the file. To elevate to root privileges, we use the
`sudo` command in front of the command we want to run, which is
`cat /etc/sudoers`. `For verification, Linux will always ask the user for
their password. This password is then kept in memory for about
5 minutes by default, so you do not have to type your password
every time if you've recently entered it.`

After entering the password, the `/etc/sudoers` file is printed for us!
It seems that sudo did indeed provide us with `superuser
permissions`. How that works is also explained by the `/etc/sudoers`
file. The # Allow members of group sudo to execute any command line is a
comment (since it starts with a #; more on this later) and tells us
that the line below gives all users of the sudo group permissions
for any commands. `On Ubuntu, the default created user is
considered an administrator and is a member of this group`. Use
the `id` command to verify this:

```bash
➜  ~ id
uid=1000(chamara) gid=1000(chamara) groups=1000(chamara),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd)
➜  ~  
```
The `sudo` command has another excellent use: switching to the `root
user`! For this, use the `--login` flag, or its shorthand, `-i`:

```bash
➜  ~ sudo -i
root@ubuntu22:~#
```

In the prompt, you will see that the `username` has changed from
`reader to root`. Furthermore, the last character in your prompt is now a `#` instead of a `$`. This is also used to denote the current
elevated permissions. You can `exit` this elevated position by
using the `built-in exit shell`:

```bash
➜  ~ sudo -i
root@ubuntu22:~# exit
logout
➜  ~ 
```

*`Remember, the root user is the superuser of the system that can do
everything. And with everything, we really mean everything! Unlike
other operating systems, if you tell Linux to delete the root file system
and everything below it, it will happily oblige (right up until the point it
has destroyed too much to work properly anymore). Do not expect an Are
you sure? prompt either. Be very, very careful with sudo commands or
anything in a root prompt.`*

##### chown, chgrp
After the little `sudo` detour, we can get back to `file permissions`:
how do we change the ownership of files? Let's start with
changing the `group using chgrp`. The syntax is as follows: `chgrp
<groupname> <filename>:`

```bash
➜  ~ chgrp games umaskfile 
chgrp: changing group of 'umaskfile': Operation not permitted
➜  ~ sudo chgrp games umaskfile 
➜  ~ ls -l
total 16
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara games      0 Apr 24 04:27 umaskfile
➜ 
```

First, we list the contents using `ls`. Next, we try to use `chgrp` to
change the group of the umaskfile file to games. However, since
this is a `privileged operation` and we did not start the command
with `sudo`, it fails with the Operation not permitted error message. Next,
we use the correct `sudo chgrp games umaskfile` command, which does
not give us feedback; generally, this is a good sign in Linux. We
list the files again to make sure that this is the case, and we can
see that the group has changed to `games` for the `umaskfile`!

Let's do the same, but now for the `user`, by using the `chown`
command. The syntax is the same as `chgrp`: `chown <username> <filename>:`

```bash
➜  ~ sudo chown pollinate umaskfile 
➜  ~ ls -l\
> 
total 16
-rw-rw-r-- 1 chamara   chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara   chamara 4096 Apr 24 04:24 testdir
-rwxr-xr-- 1 chamara   chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara   chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 pollinate games      0 Apr 24 04:27 umaskfile
➜  ~
```
As we can see, we have now changed the file ownership from
`reader:reader to pollinate:games`. However, there is one little `trick`
that's so convenient that we'd like to show you it right away! You
can actually `use chown to change both users and groups` by using
the following `syntax: chown <username>:<groupname> <filename>`. Let's see if
this can restore the umaskfile to its original ownership:

```bash
➜  ~ sudo chown chamara:chamara umaskfile 
➜  ~ ls -l
total 16
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara chamara    0 Apr 24 04:27 umaskfile
➜  ~
```

We used `random users and groups` in the preceding examples. If you
want to see which groups are present on the system, inspect the `/etc/group`
file. For users, the same information can be found in `/etc/passwd`.

```bash
➜  ~ less /etc/group
➜  ~ less /etc/passwd
```