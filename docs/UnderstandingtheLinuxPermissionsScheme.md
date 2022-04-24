- [Technical requirements](#technical-requirements)
- [Read, write, and execute](#read-write-and-execute)
  - [RWX](#rwx)
- [Users, groups, and others](#users-groups-and-others)
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

|Permission|On normal files|On directories|
|:---|:---|:---|
|Read|Allows the user to list the contents of the directory using the `ls` command.This will even list files in the directory on which the user has no other privileges!|Allows the user to list the contents of the directory using the `ls` command.This will even list files in directory on which the user has no other privileges!|
|Write|   Allows the user to make changes to the file.|Allows the user to replace or delete files within the directory, even if the user has no direct permissions on that file. However, this does not include read permissions on all files within the directory!|
Execute|Allows the user to execute the file. This is only relevant when the file is something that is supposed to be executed, such as a binary or script; otherwise, this attribute does nothing.|Allows the user to traverse into the directory by using cd. This is a separate permission from the listing of contents, but they are almost always used together; being able to list without being able to navigate into it (and vice versa) is mostly an ineffective configuration.|

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