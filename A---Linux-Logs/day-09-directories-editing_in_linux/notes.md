# Main Linux Filesystem Directories

- `/bin` - Essential command binaries(commands for local users)
- `/boot` - Boot loader files
- `/dev` - Device files
- `/etc` - System configuration files
- `/home` - User home directories or working directories for local users
- `/lib` - Essential system libraries
- `/libx32` - 32-bit libraries on 64-bit systems
- `/media` - the automatic parking spot for removable drives, managed by the desktop, cleaned up when you eject.
- `/mnt` - Mount point for temporary filesystems i.e. when you mount a disk or a network share manually.
- `/opt` - Optional application software packages or add-on software packages
- `/proc` - Process information pseudo-filesystem
- `/root` - Home directory for the root user
- `/run` - Runtime data for running processes or processes metadata
- `/sbin` - System administration binaries (commands for root user)
- `/srv` - Service-specific data
- `/sys` - System information pseudo-filesystem
- `/tmp` - Temporary files
- `/usr` - User binaries and applications
- `/var` - Variable data like logs and spool files


# Editing Files in Linux:
Means to modify the contents of a file.

Types of Editors:
- vi --> A screen-oriented text editor originally created for the Unix operating system.
- pico --> A simple, user-friendly text editor for Unix-like computing systems or operating environments using a command line interface.
- vim --> Vi IMproved, a text editor that is an improved version of the vi editor distributed with most UNIX systems.
- nano --> A simple, user-friendly text editor for Unix-like computing systems or operating environments using a command line interface.

### Modes of vim editor:
- Normal mode: This is the default mode when you open a file in vim
- Insert mode: This mode allows you to insert text into the file. You can enter this mode by pressing `i` in normal mode.
- Command mode: This mode allows you to execute commands in vim. You can enter this mode by pressing `:` in normal mode.
- Visual mode: This mode allows you to select text in the file. You can enter this mode by pressing `v` in normal mode.
- executable mode: This mode allows you to execute shell commands from within vim. You can enter this mode by pressing `!` in normal mode.


### Basic usage of vim editor:
- To open a file: `vim filename`
- To save changes: `:w`
- To save changes and exit: `:wq`
- To exit without saving changes: `:q!`


Dated: 22 October 2026
