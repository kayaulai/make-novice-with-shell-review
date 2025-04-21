---
title: Setup
---

## Files

You need to download some files to follow this lesson:

1. Download [make-lesson.zip][zip-file].

2. Move `make-lesson.zip` into a directory which you can access via your bash shell.

3. Open a Bash shell window.

4. Navigate to the directory where you downloaded the file.

5. Unpack `make-lesson.zip`:
  
  ```source
  $ unzip make-lesson.zip
  ```

6. Change into the `make-lesson` directory:
  
  ```source
  $ cd make-lesson
  ```

## Software

You also need to have the following software installed on your computer to
follow this lesson:

## Install software

If you do not already have the shell software installed, you will need to
[download and install][install_shell] it.

## Open a new shell

After installing the software

3. Open a terminal.
  If you're not sure how to open a terminal on your operating system, see the instructions below.
4. In the terminal type `cd` then press the <kbd>Return</kbd> key.
  This step will make sure you start with your home folder as your working directory.

In the lesson, you will find out how to access the data files in this folder.

:::::::::::::::::::::::::::::::::::::::::  callout

## Where to type commands: How to open a new shell

The shell is a program that enables us to send commands to the computer and receive output.
It is also referred to as the terminal or command line.

Some computers include a default Unix Shell program.
The steps below describe some methods for identifying and opening
a Unix Shell program if you already have one installed.
There are also options for identifying and downloading a Unix Shell program,
a Linux/UNIX emulator, or a program to access a Unix Shell on a server.

If none of the options below address your circumstances,
try an online search for: Unix shell [your computer model] [your operating system].


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::: solution

### Windows

Computers with Windows operating systems do not automatically have a Unix Shell program
installed.
In this lesson, we encourage you to use an emulator included in [Git for Windows][install_shell],
which gives you access to both Bash shell commands and Git.

Once installed, you can open a terminal by running the program Git Bash from the Windows start
menu.

**For advanced users:**

As an alternative to Git for Windows you may wish to [Install the Windows Subsystem for Linux][wsl]
which gives access to a Bash shell command-line tool in Windows 10 and above.

Please note that commands in the Windows Subsystem for Linux (WSL) may differ slightly
from those shown in the lesson or presented in the workshop.

::::::::::::

:::::::::::: solution

#### MacOS

For a Mac computer running macOS Mojave or earlier releases, the default Unix Shell is Bash.
For a Mac computer running macOS Catalina or later releases, the default Unix Shell is Zsh.
Your default shell is available via the Terminal program within your Utilities folder.

To open Terminal, try one or both of the following:

- In Finder, select the Go menu, then select Utilities.
  Locate Terminal in the Utilities folder and open it.
- Use the Mac 'Spotlight' computer search function.
  Search for: `Terminal` and press <kbd>Return</kbd>.

To check if your machine is set up to use something other than Bash,
type `echo $SHELL` in your terminal window.

If your machine is set up to use something other than Bash,
you can run it by opening a terminal and typing `bash`.

[How to Use Terminal on a Mac][mac-terminal]

::::::::::::

:::::::::::: solution

#### Linux

The default Unix Shell for Linux operating systems is usually Bash.
On most versions of Linux, it is accessible by running the
[Gnome Terminal][gnome-terminal] or [KDE Konsole][kde-konsole] or [xterm],
which can be found via the applications menu or the search bar.
If your machine is set up to use something other than Bash,
you can run it by opening a terminal and typing `bash`.

::::::::::::

[zip-file]: data/shell-lesson-data.zip
[install_shell]: https://carpentries.github.io/workshop-template/install_instructions/#shell
[wsl]: https://learn.microsoft.com/en-us/windows/wsl/install
[mac-terminal]: https://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/
[gnome-terminal]: https://help.gnome.org/users/gnome-terminal/stable/
[kde-konsole]: https://konsole.kde.org/
[xterm]: https://en.wikipedia.org/wiki/Xterm


### GNU Make

#### Linux

Make is a standard tool on most Linux systems and should already be available.
Check if you already have Make installed by typing `make -v` into a terminal.

One exception is Debian, and you should install Make from the terminal using
`sudo apt-get install make`.

#### OSX

You will need to have Xcode installed (download from the
[Apple website](https://developer.apple.com/xcode/)).
Check if you already have Make installed by typing `make -v` into a terminal.

#### Windows

Use the Software Carpentry
[Windows installer](https://github.com/swcarpentry/windows-installer).

### Python

Python2 or Python3, Numpy and Matplotlib are required.
They can be installed separately, but the easiest approach is to install
[Anaconda](https://www.anaconda.com/distribution/) which includes all of the
necessary python software.

[zip-file]: files/make-lesson.zip



