# What Is a Process?

A **process** is an instance of a running program.

## Process Visibility

Processes can run in two ways:

-   **Foreground process** -- runs in the terminal, and the terminal
    waits for it to finish.
-   **Background process** -- runs without keeping the terminal
    occupied.

To run a command in the background, add `&` at the end:

``` bash
sleep 200 &
```

To bring a background job to the foreground:

``` bash
fg %<job_id>
```

> Note: `fg` generally uses the shell's **job ID**, not the process ID
> (PID). Use `jobs` to view job IDs.

## Types of Processes

-   Parent process
-   Child process
-   Daemon process
-   Zombie process

## Viewing Processes

``` bash
ps -elf | less
```

-   `|` (pipe) sends the output of one command as input to another
    command.
-   `ps` -- process status
-   `-e` -- displays all processes
-   `-l` -- long format
-   `-f` -- full-format listing
-   `less` -- displays output one screen at a time; press `q` to quit.

## Fields of `ps -elf`

Example output:

``` text
F S UID          PID    PPID  C PRI  NI ADDR SZ WCHAN  STIME TTY          TIME CMD
4 S root           1       0  0  80   0 -  6374 -      11:23 ?        00:00:13 /usr/lib/systemd/systemd --switched-root --system --deserialize=51
1 S root           2       0  0  80   0 -     0 -      11:23 ?        00:00:00 [kthreadd]
1 S root           3       2  0  80   0 -     0 -      11:23 ?        00:00:00 [pool_workqueue_release]
1 I root           4       2  0  60 -20 -     0 -      11:23 ?        00:00:00 [kworker/R-rcu_gp]
1 I root           5       2  0  60 -20 -     0 -      11:23 ?        00:00:00 [kworker/R-sync_wq]
1 I root           6       2  0  60 -20 -     0 -      11:23 ?        00:00:00 [kworker/R-kvfree_rcu_reclaim]
```

  Field     Meaning
  --------- ------------------------------------------------------
  `F`       Flags
  `S`       Process state/status
  `UID`     User ID / owner of the process
  `PID`     Process ID
  `PPID`    Parent process ID
  `C`       CPU utilization (scheduler-related CPU usage field)
  `PRI`     Process priority
  `NI`      Nice value
  `ADDR`    Memory address (often shown as `-`)
  `SZ`      Process size
  `WCHAN`   Kernel function/channel where the process is waiting
  `STIME`   Start time of the process
  `TTY`     Controlling terminal
  `TIME`    Cumulative CPU time used by the process
  `CMD`     Command used to start the process

## Find a Process by Name

``` bash
pgrep <processName>
```

This displays the process ID(s) of processes whose names match the given
name.

## Display Processes in Tree Format

``` bash
pstree
```

This displays the parent-child relationship between processes in a tree
structure.

## `/etc/skel` Directory

The `/etc/skel` directory contains default files and directories that
are copied into a newly created user's home directory.

Common files may include:

-   `.bash_logout`
-   `.bashrc`
-   `.profile`

The exact files depend on the Linux distribution.

## What Does `sleep 200 &` Mean?

``` bash
sleep 200 &
```

-   `sleep 200` pauses for 200 seconds.
-   `&` runs the command in the background.
-   The shell returns control to you immediately, so you can enter other
    commands while the process is running.

## Homework (H-W)

Understand the difference between:

``` bash
ps -elf
ps -a
ps -aux
```

**Quick note:** `ps -aux` is commonly seen in practice, but the usual
BSD-style syntax is `ps aux` (without the hyphen). Compare the options
and the processes displayed on your Linux system.
