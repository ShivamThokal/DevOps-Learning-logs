# Linux Process Management

## 1. Process Monitoring

### 1.1 `top` Command

The `top` command displays running processes and system resource
utilisation in real time.

``` bash
top
```

The output is organised into system summary information and a process
list.

### 1.2 System Uptime and Load Average

Example format:

``` text
top - 10:03:24 up 175 days, 3:26, 0 users, load average: 0.51, 0.46, 0.42
```

  -----------------------------------------------------------------------
  Field                               Meaning
  ----------------------------------- -----------------------------------
  Current time                        Current system time

  Uptime                              How long the system has been
                                      running

  Users                               Number of logged-in users

  Load average                        Average system load over the last
                                      1, 5, and 15 minutes
  -----------------------------------------------------------------------

### 1.3 Tasks / Process Summary

The task summary shows the total number of tasks and their states.

  -----------------------------------------------------------------------
  State                               Meaning
  ----------------------------------- -----------------------------------
  Total                               Total number of tasks

  Running                             Tasks currently running or runnable

  Sleeping                            Tasks waiting for an event or
                                      resource

  Stopped                             Tasks that have been stopped

  Zombie                              Tasks that have completed but whose
                                      exit status has not yet been
                                      collected by the parent process
  -----------------------------------------------------------------------

### 1.4 CPU Utilisation

The CPU summary displays how CPU time is being used.

  Field   Meaning
  ------- -------------------------------------------
  `us`    CPU time spent on user processes
  `sy`    CPU time spent on system/kernel processes
  `ni`    CPU time spent on niced processes
  `id`    CPU idle time
  `wa`    CPU time waiting for I/O
  `hi`    Time spent handling hardware interrupts
  `si`    Time spent handling software interrupts
  `st`    CPU time stolen by the hypervisor

### 1.5 Memory and Swap Utilisation

The `top` output includes memory and swap summaries.

  Memory field   Meaning
  -------------- -----------------------------------
  Total          Total available memory
  Free           Memory currently unused
  Used           Memory currently in use
  Buff/cache     Memory used for buffers and cache

  -----------------------------------------------------------------------
  Swap field                          Meaning
  ----------------------------------- -----------------------------------
  Total                               Total swap space

  Free                                Unused swap space

  Used                                Swap space currently in use

  Available                           Memory estimate available for
                                      starting new applications without
                                      swapping
  -----------------------------------------------------------------------

### 1.6 Columns in the `top` Process List

  Column   Meaning
  -------- -----------------------------------------------
  `PID`    Process ID
  `USER`   User who owns the process
  `PR`     Process priority
  `NI`     Nice value
  `VIRT`   Virtual memory used by the process
  `RES`    Resident memory (physical RAM currently used)
  `SHR`    Shared memory
  `S`      Process status/state
  `%CPU`   CPU utilisation percentage
  `%MEM`   Memory utilisation percentage

------------------------------------------------------------------------

## 2. Process States

A process can be in different states during its lifetime. The `top`
command displays a process state in the `S` column.

  State   Meaning
  ------- --------------------------------------
  `D`     Uninterruptible sleep
  `I`     Idle
  `R`     Running or runnable
  `S`     Sleeping
  `T`     Stopped by a job-control signal
  `t`     Stopped by a debugger during tracing
  `Z`     Zombie

------------------------------------------------------------------------

## 3. Managing Processes with Signals

### 3.1 `kill` Command

The `kill` command sends a signal to a process. The process is
identified using its PID.

``` bash
kill -SIGNAL PID
```

Examples from the class notes:

  Command          Signal      Purpose
  ---------------- ----------- ---------------------------------
  `kill -9 PID`    `SIGKILL`   Forcefully terminates a process
  `kill -15 PID`   `SIGTERM`   Requests graceful termination
  `kill -19 PID`   `SIGSTOP`   Stops a process
  `kill -18 PID`   `SIGCONT`   Resumes a stopped process

**Note:** Replace `PID` with the actual process ID. `SIGTERM` gives a
process an opportunity to shut down cleanly; `SIGKILL` does not.

### 3.2 `htop` Command

``` bash
htop
```

`htop` is an interactive process-monitoring tool. It provides a
user-friendly view of running processes and system resource usage.

------------------------------------------------------------------------

## 4. Differences to Study

The following comparisons are study questions listed in the class notes.
The tables provide the key differences in a concise format.

### 4.1 Difference Between `ps` and `top`

  -----------------------------------------------------------------------
  Basis                   `ps`                    `top`
  ----------------------- ----------------------- -----------------------
  Purpose                 Displays information    Monitors processes and
                          about processes         system resource usage

  Output                  Snapshot of process     Continuously refreshed
                          information when the    process and system
                          command is executed     information

  Updating                Does not refresh        Refreshes periodically
                          automatically           

  Resource usage          Can show process        Shows CPU and memory
                          details; output depends utilisation along with
                          on the selected options process information

  Interaction             Generally used as a     Interactive; supports
                          command that prints     keyboard controls
                          output                  

  Best used for           Checking process        Live monitoring of
                          details at a particular processes and system
                          moment                  performance
  -----------------------------------------------------------------------

### 4.2 Difference Between `top` and `htop`

  -----------------------------------------------------------------------
  Basis                   `top`                   `htop`
  ----------------------- ----------------------- -----------------------
  Interface               Traditional text-based  More user-friendly,
                          interface               interactive text
                                                  interface

  Display                 Displays process and    Displays process
                          system information in a information in a more
                          compact layout          visual and navigable
                                                  layout

  Navigation              Primarily               Easier navigation,
                          keyboard-based          including arrow-key
                                                  process selection

  Process management      Supports process        Provides convenient
                          actions through         interactive
                          keyboard commands       process-management
                                                  controls

  Resource display        Displays CPU, memory,   Displays CPU, memory,
                          swap, and process       swap, and process
                          information             information in a more
                                                  visual way

  Availability            Commonly available by   May need to be
                          default on Linux        installed separately
                          systems                 

  Best used for           Quick process and       Interactive process
                          system monitoring       monitoring and
                                                  management
  -----------------------------------------------------------------------

### 4.3 CPU Utilisation

CPU utilisation is the amount of CPU processing time being used by
processes and the operating system. In `top`, the CPU summary helps
identify whether CPU time is being spent on user work, system work, I/O
wait, or idle time.

------------------------------------------------------------------------

## 5. Skeleton Files

### 5.1 What Is a Skeleton File?

A skeleton file is a default configuration file used when creating a new
user's home directory.

Skeleton files act as a layout or configuration blueprint. Their
contents are copied into the home directory of a newly created user.

### 5.2 Skeleton Directory

``` bash
/etc/skel
```

`/etc/skel` is the default skeleton directory on many Linux
distributions.

-   It contains default files and directories.
-   Its contents are used to initialise a new user's home directory.
-   It helps provide a consistent starting environment for new users.

### 5.3 View the Contents of `/etc/skel`

``` bash
ls -la /etc/skel
```

### 5.4 Create a User with a Home Directory

``` bash
useradd -m username
```

The `-m` option creates the user's home directory and populates it using
the skeleton directory.

------------------------------------------------------------------------

## 6. Quick Revision

  Topic                                    Command / Key point
  ---------------------------------------- ------------------------------
  Live process monitoring                  `top`
  Interactive process monitoring           `htop`
  Process information                      `ps`
  View process states                      Check the `S` / `STAT` field
  Forcefully terminate a process           `kill -9 PID`
  Gracefully request process termination   `kill -15 PID`
  Stop a process                           `kill -19 PID`
  Resume a stopped process                 `kill -18 PID`
  Skeleton directory                       `/etc/skel`
  List skeleton directory contents         `ls -la /etc/skel`
  Create a user with a home directory      `useradd -m username`

------------------------------------------------------------------------



Dated : 4 November 2026