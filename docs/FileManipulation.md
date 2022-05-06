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

As you might expect, as with many things under Linux, the cp
command is also very versatile. We can indeed copy files not
owned by us; we do not need to be in the same directory as the
file, and we can also copy directories! Let's try a few of these
things out: