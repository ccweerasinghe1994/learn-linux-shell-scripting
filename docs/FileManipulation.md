- [File Manipulation](#file-manipulation)
  - [Technical requirements](#technical-requirements)
  - [Common file operations](#common-file-operations)
  - [Copying](#copying)
### File Manipulation
This chapter is dedicated to `file manipulation`. As in `everything`
is a `filesystem`, file manipulation is considered one of the most
`important` aspects of working with `Linux`. We will start by
exploring `common` `file` `operations`, such as `creating`, `copying`, and
`deleting` files. We will continue with a bit on `archiving`, another
important tool when working on the command line. The last part
of this chapter will be devoted to `finding` `files` on the file system,
another important skill in the `tool set` of a shell `scripter`.

The following commands will be introduced in this chapter: `cp`, `rm`,
`mv`, `ln`, `tar`, `locate`, and `find`.

The following topics will be covered in this chapter:
-   Common file operations
-   Archiving
-   Finding files

#### Technical requirements
We will practice file manipulation using the virtual machine we
created in Chapter 2, Setting Up Your Local Environment. No
further resources are needed at this point.

#### Common file operations
So far, we have mainly introduced commands related to
`navigation` on the Linux `filesystem`. In earlier chapters, we
already saw that we can use `mkdir` and `touch` to create `directories`
and `empty files`, respectively. If we want to give a file some
meaningful (text) content, we use `vim` or `nano`. However, we have
not yet talked about `removing` `files` or `directories`, or `copying`,
`renaming`, or `creating shortcuts`. Let's start with `copying files`.

#### Copying
In essence, `copying` a file on Linux is really simple: use the `cp`
command, followed by the `filename-to-be-copied` to the
`filename-to-copy-to`. It looks something like this:

```bash
🔥 ➜  ~ ls -l
total 16
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rw-rw-r-- 1 chamara chamara    0 May  6 01:31 testfile
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara chamara    0 Apr 24 04:27 umaskfile
🔥 ➜  ~ cp testfile testfilecopy
🔥 ➜  ~ ls -l
total 16
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rw-rw-r-- 1 chamara chamara    0 May  6 01:31 testfile
-rw-rw-r-- 1 chamara chamara    0 May  6 01:32 testfilecopy
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara chamara    0 Apr 24 04:27 umaskfile
```

-   Do we always need to be in the `same directory` as the
`source` and `destination` file?
-   What about the `permissions` of the file?
-   Can we also `copy` `directories` with `cp`?

As you might expect, as with many things under Linux, the `cp`
command is also very `versatile`. We can indeed `copy files not
owned by us`; we do `not` `need` to be in the same `directory` as the
`file`, and we `can` also `copy` `directories`! Let's try a few of these
things out:

```bash
🔥 ➜  ~ cd /var/log
🔥 ➜  log ls -l
total 5712
-rw-r--r--  1 root      root                  0 May  3 14:46 alternatives.log
-rw-r--r--  1 root      root              35403 Apr 29 02:21 alternatives.log.1
drwxr-xr-x  2 root      root               4096 May  5 02:13 apt
-rw-r-----  1 syslog    adm                3357 May  6 01:30 auth.log
-rw-r-----  1 syslog    adm               27631 May  3 14:46 auth.log.1
-rw-r-----  1 syslog    adm                2052 Apr 23 19:17 auth.log.2.gz
-rw-r-----  1 syslog    adm                2011 Apr 22 01:01 auth.log.3.gz
-rw-r--r--  1 root      root              64966 Apr  5 09:55 bootstrap.log
-rw-rw----  1 root      utmp                  0 May  3 14:46 btmp
-rw-rw----  1 root      utmp               1152 Apr 29 02:11 btmp.1
-rw-r--r--  1 syslog    adm             1138640 May  6 01:30 cloud-init.log
-rw-r-----  1 root      adm               47907 May  6 01:30 cloud-init-output.log
drwxr-xr-x  2 root      root               4096 Mar 31 21:23 dist-upgrade
-rw-r-----  1 root      adm              156049 May  6 01:30 dmesg
-rw-r-----  1 root      adm              156322 May  5 02:12 dmesg.0
-rw-r-----  1 root      adm                   0 May  3 14:46 dmesg.1.gz
-rw-r-----  1 root      adm               27028 Apr 29 22:48 dmesg.2.gz
-rw-r-----  1 root      adm               26920 Apr 29 02:41 dmesg.3.gz
-rw-r-----  1 root      adm               27064 Apr 29 02:10 dmesg.4.gz
-rw-r--r--  1 root      root               2722 May  5 02:13 dpkg.log
-rw-r--r--  1 root      root             570739 Apr 29 02:21 dpkg.log.1
-rw-r--r--  1 root      root              32128 Apr 24 06:35 faillog
drwxr-x---  3 root      adm                4096 Apr 10 00:49 installer
drwxr-sr-x+ 3 root      systemd-journal    4096 Apr 10 00:52 journal
-rw-r-----  1 syslog    adm              397616 May  6 01:30 kern.log
-rw-r-----  1 syslog    adm              791189 May  3 14:46 kern.log.1
-rw-r-----  1 syslog    adm              108522 Apr 24 01:14 kern.log.2.gz
-rw-r-----  1 syslog    adm              156837 Apr 22 01:01 kern.log.3.gz
drwxr-xr-x  2 landscape landscape          4096 Apr 10 00:53 landscape
-rw-rw-r--  1 root      utmp             293168 May  6 01:30 lastlog
drwx------  2 root      root               4096 Apr  5 09:57 private
-rw-r-----  1 syslog    adm              513599 May  6 01:35 syslog
-rw-r-----  1 syslog    adm              975162 May  3 14:46 syslog.1
-rw-r-----  1 syslog    adm              154471 Apr 24 01:14 syslog.2.gz
-rw-r-----  1 syslog    adm              213220 Apr 22 01:01 syslog.3.gz
-rw-r--r--  1 root      root                  0 Apr  5 09:59 ubuntu-advantage.log
-rw-r--r--  1 root      root                157 May  3 14:52 ubuntu-advantage-timer.log
-rw-r--r--  1 root      root               1022 Apr 29 23:35 ubuntu-advantage-timer.log.1
drwxr-x---  2 root      adm                4096 May  3 14:46 unattended-upgrades
-rw-r--r--  1 root      root                193 May  5 02:12 vmware-network.1.log
-rw-r--r--  1 root      root                193 May  3 14:46 vmware-network.2.log
-rw-r--r--  1 root      root                195 Apr 29 22:48 vmware-network.3.log
-rw-r--r--  1 root      root                195 Apr 29 02:41 vmware-network.4.log
-rw-r--r--  1 root      root                195 Apr 29 02:10 vmware-network.5.log
-rw-r--r--  1 root      root                195 Apr 23 03:59 vmware-network.6.log
-rw-r--r--  1 root      root                195 Apr 23 01:39 vmware-network.7.log
-rw-r--r--  1 root      root                195 Apr 22 01:01 vmware-network.8.log
-rw-r--r--  1 root      root                195 Apr 11 00:59 vmware-network.9.log
-rw-r--r--  1 root      root                193 May  6 01:30 vmware-network.log
-rw-------  1 root      root                  0 May  5 02:13 vmware-vmsvc-root.1.log
-rw-------  1 root      root               2758 May  5 02:13 vmware-vmsvc-root.2.log
-rw-------  1 root      root               2897 May  5 02:13 vmware-vmsvc-root.3.log
-rw-------  1 root      root               1971 May  6 01:30 vmware-vmsvc-root.log
-rw-------  1 root      root               3890 May  6 01:30 vmware-vmtoolsd-root.log
-rw-rw-r--  1 root      utmp              53760 May  6 01:30 wtmp
🔥 ➜  log cp dpkg.log /home/chamara 
🔥 ➜  log ls -l /home/chamara 
total 20
-rw-r--r-- 1 chamara chamara 2722 May  6 01:43 dpkg.log
-rw-rw-r-- 1 chamara chamara   29 Apr 24 02:07 nano-test.txt
drwxrwxr-x 2 chamara chamara 4096 Apr 24 04:24 testdir
-rw-rw-r-- 1 chamara chamara    0 May  6 01:31 testfile
-rw-rw-r-- 1 chamara chamara    0 May  6 01:32 testfilecopy
-rwxr-xr-- 1 chamara chamara   81 Apr 23 01:55 test.txt
drwxrwx--- 2 chamara chamara 4096 Apr 24 04:27 umaskdir
-rw-rw---- 1 chamara chamara    0 Apr 24 04:27 umaskfile
🔥 ➜  log cp tallylog /home/chamara/
cp: cannot open 'tallylog' for reading: Permission denied

```

So, what happened? We used `cd` to change the directory to
`/var/log/`. We listed the files there using ls with the long option.
We copied a file with a relative path that we were able to read,
but that was owned by root:root, to the fully qualified /home/reader/
directory. When we listed /home/reader/ with the fully qualified
path, we saw that the copied file was now owned by reader:reader.
When we tried to do the same for the tallylog file, we got the error
cannot open 'tallylog' for reading: Permission denied. This should not be
unexpected, since we do not have any read permissions on that
file, so copying would be hard.