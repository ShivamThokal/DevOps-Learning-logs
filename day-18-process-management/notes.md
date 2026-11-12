## 1. Process Monitoring

### 1.1 `top` Command

The `top` command displays running processes and system resource utilisation in real time.

```bash
top
```

Its output is organised into system summary information and a process list.

### 1.2 System Uptime and Load Average

Example:

```text
top - 10:03:24 up 175 days, 3:26, 0 users, load average: 0.51, 0.46, 0.42
```

| Field | Meaning |
|---|---|
| Current time | Current system time (`10:03:24`) |
| Uptime | How long the system has been running (`175 days, 3:26`) |
| Users | Number of currently logged-in users (`0 users`) |
| Load average | Average system load over the last 1, 5, and 15 minutes (`0.51, 0.46, 0.42`) |

**Note:** Load average represents the average number of tasks that are runnable or waiting for uninterruptible I/O. Whether a value indicates high load depends partly on the number of CPU cores.

### 1.3 Tasks / Process Summary

The task summary displays the total number of tasks along with their current states.

| State | Meaning |
|---|---|
| Total | Total number of tasks reported by `top` |
| Running | Tasks currently running or ready to run |
| Sleeping | Tasks waiting for an event or resource |
| Stopped | Tasks that have been stopped |
| Zombie | Tasks that have completed execution but whose exit status has not yet been collected by the parent process |

### 1.4 CPU Utilisation

The CPU summary displays how CPU time is being used.

| Field | Meaning |
|---|---|
| `us` | CPU time spent running user-space processes |
| `sy` | CPU time spent running kernel/system code |
| `ni` | CPU time spent running processes with adjusted nice values |
| `id` | CPU idle time |
| `wa` | CPU time spent waiting for I/O to complete |
| `hi` | Time spent handling hardware interrupts |
| `si` | Time spent handling software interrupts |
| `st` | CPU time taken by the hypervisor from a virtual machine |

### 1.5 Memory and Swap Utilisation

The `top` output includes memory and swap summaries. The exact labels and layout can vary by Linux distribution and `top` version.

**Memory fields**

| Field | Meaning |
|---|---|
| Total | Total physical memory (RAM) |
| Free | Memory currently unused |
| Used | Memory currently in use, according to the displayed memory accounting |
| Buff/cache | Memory used by buffers and cache |
| Available | Estimate of memory available for starting new applications without swapping |

**Swap fields**

| Field | Meaning |
|---|---|
| Total | Total swap space |
| Free | Unused swap space |
| Used | Swap space currently in use |

**Note:** `Available` is a memory estimate, not a swap field. It is generally shown in the memory summary.

### 1.6 Columns in the `top` Process List

| Column | Meaning |
|---|---|
| `PID` | Process ID |
| `USER` | User who owns the process |
| `PR` | Process priority |
| `NI` | Nice value |
| `VIRT` | Total virtual memory used by the process |
| `RES` | Resident memory (physical RAM currently used by the process) |
| `SHR` | Shared memory |
| `S` | Process status/state |
| `%CPU` | CPU utilisation percentage |
| `%MEM` | Memory utilisation percentage |

---

## 2. Process States

A process can be in different states during its lifetime. The `top` command displays a process state in the `S` column.

| State | Meaning |
|---|---|
| `D` | Uninterruptible sleep, usually while waiting for I/O |
| `I` | Idle kernel thread |
| `R` | Running or runnable |
| `S` | Interruptible sleep (waiting for an event or signal) |
| `T` | Stopped by a job-control signal |
| `t` | Stopped by a debugger during tracing |
| `Z` | Zombie: the process has exited, but its parent has not yet collected its exit status |

---

## 3. Managing Processes with Signals

### 3.1 `kill` Command

The `kill` command sends a signal to a process. The process is identified using its PID.

```bash
kill -SIGNAL PID
```

Examples:

| Command | Signal | Purpose |
|---|---|---|
| `kill -9 PID` | `SIGKILL` | Forcefully terminates a process |
| `kill -15 PID` | `SIGTERM` | Requests graceful termination |
| `kill -19 PID` | `SIGSTOP` | Stops a process |
| `kill -18 PID` | `SIGCONT` | Resumes a stopped process |

**Notes:**
- Replace `PID` with the actual process ID.
- `SIGTERM` requests that a process terminate, allowing it to perform cleanup.
- `SIGKILL` terminates a process immediately; the process cannot catch or ignore it.
- `SIGSTOP` stops a process and cannot be caught or ignored.
- `SIGCONT` resumes a stopped process.
- Signal numbers shown above are the common Linux values on widely used architectures; symbolic signal names are clearer and more portable.

Examples using signal names:

```bash
kill -SIGTERM PID
kill -SIGKILL PID
kill -SIGSTOP PID
kill -SIGCONT PID
```

### 3.2 `htop` Command

```bash
htop
```

`htop` is an interactive process-monitoring tool. It provides a user-friendly view of running processes and system resource usage.

It may need to be installed separately, depending on the Linux distribution.

---

## 4. Differences to Study

### 4.1 Difference Between `ps` and `top`

| Basis | `ps` | `top` |
|---|---|---|
| Purpose | Displays information about processes | Monitors processes and system resource usage |
| Output | Provides a snapshot when the command is executed | Continuously refreshes process and system information |
| Updating | Does not refresh automatically | Refreshes periodically |
| Resource usage | Displays process details; fields depend on the selected options | Shows CPU and memory utilisation along with process information |
| Interaction | Usually prints output and returns to the shell | Interactive; supports keyboard controls |
| Best used for | Checking process details at a particular moment | Live monitoring of processes and system performance |

### 4.2 Difference Between `top` and `htop`

| Basis | `top` | `htop` |
|---|---|---|
| Interface | Traditional text-based interface | More user-friendly, interactive text interface |
| Display | Displays process and system information in a compact layout | Displays process information in a more visual and navigable layout |
| Navigation | Primarily keyboard-based | Supports convenient navigation, including arrow-key process selection |
| Process management | Supports process actions through keyboard commands | Provides convenient interactive process-management controls |
| Resource display | Displays CPU, memory, swap, and process information | Displays CPU, memory, swap, and process information in a more visual way |
| Availability | Commonly installed by default on many Linux systems | May need to be installed separately |
| Best used for | Quick process and system monitoring | Interactive process monitoring and management |

### 4.3 CPU Utilisation

CPU utilisation indicates how CPU processing time is being used by processes and the operating system. In `top`, the CPU summary helps identify whether CPU time is being spent on user work, system work, I/O wait, or idle time.

---

## 5. Skeleton Files

### 5.1 What Is a Skeleton File?

A skeleton file is a default configuration file used when creating a new user's home directory.

Skeleton files act as a layout or configuration blueprint. Their contents are copied into the home directory of a newly created user.

### 5.2 Skeleton Directory

```bash
/etc/skel
```

`/etc/skel` is the default skeleton directory on many Linux distributions.

- It contains default files and directories.
- Its contents are used to initialise a new user's home directory.
- It helps provide a consistent starting environment for new users.

### 5.3 View the Contents of `/etc/skel`

```bash
ls -la /etc/skel
```

### 5.4 Create a User with a Home Directory

```bash
useradd -m username
```

The `-m` option creates the user's home directory and populates it using the skeleton directory (subject to the system's user-creation configuration).

---

## 6. Quick Revision

| Topic | Command / Key point |
|---|---|
| Live process monitoring | `top` |
| Interactive process monitoring | `htop` |
| Process information | `ps` |
| View process states | Check the `S` / `STAT` field |
| Forcefully terminate a process | `kill -9 PID` |
| Gracefully request process termination | `kill -15 PID` |
| Stop a process | `kill -19 PID` |
| Resume a stopped process | `kill -18 PID` |
| Skeleton directory | `/etc/skel` |
| List skeleton directory contents | `ls -la /etc/skel` |
| Create a user with a home directory | `useradd -m username` |

---
**Date:** 4 November 2026

output_path.write_text(content, encoding="utf-8")
print(f"Created {output_path} ({output_path.stat().st_size:,} bytes)")
