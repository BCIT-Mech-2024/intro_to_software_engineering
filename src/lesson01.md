# Preparing your Environment
In order to write software, we first need to prepare an environment with in which to 
develop.  A software development environment is different for every language, operating
system, and machine architecture.  The steps here will outline the general process for
setting up an environment, though it will specifically look at getting your environment
ready for C development.

## What is a development environment?
As a simple analogy, developing software can be considered similarly to cooking. While it
is possible to cook food on an open flame with a stick, your ability to make complex or
interesting food is limited by your lack of tools.  To cook more complex dishes, you 
require a "cooking environment"; a space full of tools that unlock more powerful
cooking technique: tools of the trade.

## Note on terminal applications
Up until now, I suspect many of you have only ever used Graphical User Interfaces (GUI) and
the world of Terminal User Interfaces (TUI) can be a bit intimidating. While it takes some
getting use to, I'd recommend embracing the terminal whole-heartily. UI and UX design
are unique and challenging discipline and, sometimes, the people making software aren't
exactly the most aesthetically gifted.  Great software to solve many problems are available
in the terminal if you're willing to overcome the user interface.

### Terminal navigation
The main thing your computer does is maintain and update a file system.  Your Documents, 
Music, Videos, PDFs, etc. all live within a "file system"; a structured way of grouping
files into "directories" and assigning "permissions".  The most basic tasks that you will
need to complete on a regular basis are creating, renaming, moving, and deleting files.
As such, you should be well familiar with the basics of navigating the file system in 
the terminal.  Unfortunately, this is where there is a rather noticeable difference
between Windows and OSX.  The commands for navigation and interacting with your file
system differ between the two major OSes.  Additionally, as of Windows 10, Microsoft has 
put in an effort to be more friendly to software developer who usually work on POSIX OSes
(Linux, OSX, BSD, etc.) so Windows has a bit of duplication depending on if you are using
PowerShell or CMD.  I recommend using PowerShell

| Description                         | OSX                        | Windows                      |
| -----                               | ----                       | ---                          |
| Print your current directory        | `pwd`                      | `cd`                         |
| List files in the current directory | `ls`                       | `dir`                        |
| Change to a directory               | `cd ${dir}`                | `cd ${dir}                   |
| Move/rename a file                  | `mv ${oldname} ${newname}` | `move ${oldname} ${newname}` |
| Copy a file                         | `cp ${file} ${newfile}`    | `copy ${file} ${newfile}`    |
| Delete a file                       | `rm ${filename}`           | `del`                        |
| Delete an empty folder              | `rmdir ${dirname}`         | `rmdir ${dirname}`           |
| Create a directory                  | `mkdir ${dirname}`         | `mkdir ${dirname}`           |
| Clear the terminals                 | `clear`                    | `cls`                        |

## Setup
To setup a development environment you need to:
1. Get the toolchain for your target language, operating system, and architecture
	1. Compiler
	2. Standard Library
	3. Debugger
	4. Documentation
	5. Build tools
2. Install the toolchains need to be installed within your OSes `PATH` so that the
		OS can find the binaries when they are called.  
3. Install a code editor
4. Configured editor plugins
	1. Syntax highlighting
	2. Autocomplete
	3. Test runners
5. Setup version control
6. Setup authentication

For this program, we'll be using the following:
1. C language with the `gcc` compiler
2. Detailed below
3. VSCode
4. Detailed below
5. Git
6. N/A

### Determining OS and Architecture
Some of the step below will be different based on your OS and Architecture.  To find out
those details
* Determine whether you have a Microsoft or Apple computer.  Your OS is either 
	 Windows or OSX respectively.
* **For Windows Users:** Open up your "Settings" and go to "About this PC" and 
	 check whether your computer is an x86_32, or x86_64 bit.  It is most likely 64 bit,
	 but double check that this is the case
* **For OSX Users:** Click the Apple logo in the upper left most portion of your screen 
	and go to the "About This Computer" section.  Your computer is either an Intel or
	an Apple Silicon.  This is AMD or ARM architecture respectively

### 1. Toolchain
#### Windows
MinGW Installation Manager:
| package name         | versions   | type          |
| ----                 | ----       | ----          |
| mingw32-binutils     | 2.28-1     | bin, man      |
| mingw32-gcc          | 6.3.0-1    | bin           |
| mingw32-gcc-deps     | 6.3.0-1    | dll           |
| mingw32-gcc-g++      | 6.3.0-1    | bin           |
| mingw32-gdb          | 7.6.1-1    | bin, man      |
| mingw32-libatomic    | 6.3.0-1    | dll           |
| mingw32-libgcc       | 6.3.0-1    | dll           |
| mingw32-libgmp       | 6.1.2-2    | dll           |
| mingw32-libgomp      | 6.3.0-1    | dll           |
| mingw32-libgomp-deps | 5.3.0      | dll           |
| mingw32-libiconv     | 1.14-3     | dll           |
| mingw32-libintl      | 0.18.3.2-2 | dll           |
| mingw32-libisl       | 0.18.1     | dll           |
| mingw32-libmingwex   | 5.0.2      | dll           |
| mingw32-libmpc       | 1.0.3-1    | dll           |
| mingw32-libpfr       | 3.1.5-2    | dll           |
| mingw32-libthreadgc  | 2.10-pre   | dll           |
| mingw32-libquadmath  | 6.3.0-1    | dll           |
| mingw32-libssp       | 6.3.0-1    | dll           |
| mingw32-libstdc++    | 6.3.0-1    | dll           |
| mingw32-libz         | 1.2.8-1    | dll           |
| mingw32-make         | 3.82.90    | bin           |
| mingw32-mingw-get    | 0.6.2-beta | bin, gui, lic |
| mingw32-mingwrt      | 5.0.2      | dev, dll      |
| mingw32-w32api       | 5.0.2      | dev           |

#### OSX
To install all the necessary support files, run 
```sh
xcode-select --install
```
XCode is the
native development environment on OSX, but the "gcc" binary it installs is actually
the compiler "clang" in disguise.  To fix this issue, you'll need to install the package
manager `brew`. (In a later talk, I'll go deeper into the concept of a package manager)
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After you've finished installing `brew`
```sh
brew install gcc
```

**WARNING!** **WARNING!**

> As a general rule, you should _NOT_ trust a random bash command that uses `curl` like this.
What the above command says is "Take the file at the URL and run the code".  This is a 
way to get malware on your computer.  Do _NOT_ run arbitrary bash commands like without
some degree of trust in the author of the command

**WARNING!** **WARNING!**

### 2. `PATH`
`gcc` is a standard enough toolchain that the installers you just used will have 
appropriately updated your `PATH` variable to make sure the binaries are accessible to
the OS.  To validate that this is true, check that you can find the binaries with
the following commands:
* **Window:** `Get-Command gcc` 
* **OSX:** `which gcc` 

### 3. Code Editor
Go to the [VS Code](https://code.visualstudio.com/download) website and download the
version of the application that matches your OS and Architecture.  Follow through with
the standard installation process for your machine

### 4. Configure Plugins
In VSCode, there is an Extensions tab in the left hand column. Install the following:
* C/C++ , by Microsoft
* CMake Tools, by Microsoft
* Makefile Tools, by Microsoft

### 5. Version Control
#### Windows
Download the appropriate [**Standalone Installer**](https://git-scm.com/download/win)

Once downloaded, run the installer, the numbers below will outline which settings you
should have on the various menus:
1. **Next** to accept the GNU General Public License
2. Leave as default
3. Hit the dropdown and select **Use Notepad as Git's default editor**.  You'll need 
	 to scroll down somewhat as you'll first see **Notepad++**.  That is also an acceptable
	 option so long as you have Notepad++ installed.
4. **Override the default** and have the name in the box by **main**
5. **Use Git and Optional Unix Tools from the Command Prompt**
6. **Use bundled OpenSSH**
7. **Use the OpenSSL library**
8. **Checkout Windows-style, commit Unix-style line endings**
9. **Use MinTTY**
10. **Fast-forward or merge**
11. **Git Credential Manager**
12. Neither option is required here, but **Enable file system caching** can be helpful
13. Leave both these options unchecked

#### OSX
Simply,
```
brew install git
```

#### Configuring Git
Every commit in Git is associated to a person.  As such, git won't let you make any commits
unless it has a way to tie your name to it.  Run the following commands to setup your user
data for git commits.
```sh
git config --global user.name "${Your full name}"
git config --global user.email "${Your email}"
```

#### Git GUI
Git is an extremely powerful tool, but one with a pretty steep learning curve.  There is a
Desktop GUI application that connects `git` with GitHub and makes file management a bit 
easier to handle.  I'm not a fan of this tool as it hides some of the much more powerful
features of `git` and comes with a certain amount of "vendor lock-in" in an attempt to make
you think GitHub is the only place on the internet to share source code.  While I
recommend that everyone learn to use `git` in the terminal, I understand that not everyone
has the time to do so and provide the following link to the 
[GitHub Desktop Client](https://desktop.github.com/download/)

### 6. Authentication
Authentication is the process of setting up your device with the appropriate passwords,
private keys, and associated other networking information to allow you to access protected
resources.  While no explicit authentication looks like it will be necessary for the
upcoming C course, I would recommend everyone setup a GitHub account.

After establishing an account, you'll need to generate a public/private key pair and 
give GitHub your private key so that you can easily push and pull resources from your 
personal repository.

```sh
ssh-keygen
```
This will take a moment and ask if it can place a key someplace like this 
`/home/${your name}/.ssh/`.  Hit `Y` to approve.  You'll be asked for a password and I
recommend that you _do not_ create a password.  There's nothing particularly wrong with
having a password created, but it means you'll have to type it in every time you
push a commit.  Not a problem, but pretty annoying.  For interested parties, 

```sh
cat ~/.ssh/*.pub
```
Copy the output of this command (a large block of random letter and numbers).  Go to
(https://github.com/settings/keys)[https://github.com/settings/keys] 
and click `New SSH Key`.  Paste the output of the `cat` command and name it something.


## Testing out the environment
To make sure that your environment is working, I've create a simple "hello world"
project that should immediately compile and run without issue.  Clone the following
repository using
```
git clone git@github.com:Bekreth/basic_c_test.git
```
Once the project has been cloned, follow the directions of the README.md. I've
intentionally written the README.md to match a lot of what these types of documents
(sadly) usually look like.  For Windows users, you'll have more difficulty as these
documents are typically written for POSIX machines with an understand set of programs
that may not be available to Windows machines by default.


## Additional learning resources
The goal of this document is to get you setup for development, so we've installed a bunch
of tools.  There hasn't been the time necessary to learn how to use all these tools, or
even what most of them do.  In following meetings, I'll go into detail about these, how and
when you'd want to use them, but in the mean time, here is a link to a learning resource
for the most powerful tool currently in your arsenal: Git

[https://learngitbranching.js.org/](https://learngitbranching.js.org/)

This online game teaches you the basic commands of `git` and I'd recommend doing at 
least lessons 1, 2, 3 of Intro in Main and 1-6 of Push-Pull in Remote.  This will 
give you the basic tools to understand the most used `git` operations and there is 
a wonderful graphic to show what these various commands are doing.
