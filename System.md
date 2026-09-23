## 09:09:26(Day1) - 08:30 -- 09:02(break) -- 09:34 -- 10:50(112min)

### Part 1) What an OS does
#### first of all how does a command even work

When you type "ls" the computer can not understand what it means it first need to be go through 4 levels
1) You
2) shell
3) kernel
4) hardware

### 1) You 
type a command like ls

### 2) The shell
interpret the command(understand it)
common shells
* bash
* zsh
* fish
its job is to:
* read what you typed
* find the program
* ask kernel to run it

### 3) kernel
It controls
* CPU scheduling
* memory allocation
* Security permissions
* devices
* files
* networking

### 4) Hardware
* CPU
* RAM
* SSD

## What happens when you type `ls`?

Let's trace the journey.

![700](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22360%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20720%20360%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2240%22%20y%3D%2230%22%20width%3D%22640%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%2255%22%20font-size%3D%2218%22%20font-family%3D%22Arial%22%3EYou%20type%3A%20ls%3C%2Ftext%3E%3Cpath%20d%3D%22M360%2070%20V95%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22360%2C105%20354%2C93%20366%2C93%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22105%22%20width%3D%22640%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22130%22%20font-size%3D%2218%22%20font-family%3D%22Arial%22%3EShell%20searches%20PATH%20for%20%2Fbin%2Fls%20or%20%2Fusr%2Fbin%2Fls%3C%2Ftext%3E%3Cpath%20d%3D%22M360%20145%20V170%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22360%2C180%20354%2C168%20366%2C168%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22180%22%20width%3D%22640%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22205%22%20font-size%3D%2218%22%20font-family%3D%22Arial%22%3EKernel%20creates%20a%20new%20process%20and%20loads%20the%20program%20into%20memory%3C%2Ftext%3E%3Cpath%20d%3D%22M360%20220%20V245%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22360%2C255%20354%2C243%20366%2C243%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22255%22%20width%3D%22640%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22280%22%20font-size%3D%2218%22%20font-family%3D%22Arial%22%3EProgram%20asks%20kernel%20to%20read%20the%20current%20directory%3C%2Ftext%3E%3Cpath%20d%3D%22M360%20295%20V320%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22360%2C330%20354%2C318%20366%2C318%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22330%22%20width%3D%22640%22%20height%3D%2225%22%20rx%3D%228%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22347%22%20font-size%3D%2218%22%20font-family%3D%22Arial%22%3ENames%20are%20printed%20back%20to%20your%20terminal%3C%2Ftext%3E%3C%2Fsvg%3E)

Notice something important.

`ls` never reads the disk directly.

Every file operation goes through the kernel.

That rule will become crucial later when we study Docker and Kubernetes.

## Program vs Process

This distinction matters throughout systems engineering.

|Program|Process|
|---|---|
|A file stored on disk|A running instance of that file|
|Passive|Active|
|`/bin/ls`|The `ls` currently executing|

Think of it like this:

- Program = recipe
    
- Process = meal being cooked
    

Multiple processes can run from the same program.

### In my words - when you run a command the shell first search the path to the program /bin/ls then kernel starts the new process and loads the program into memory here the new process means that the passive inactive program is not active and running the process ls ask the kernel to read the current directory and names are printed back to the terminal

Applications do not own the computer.

The kernel is the referee.

Every request passes through it.

> Read a file? Ask the kernel. Use the CPU? Ask the kernel. Send data over Wi-Fi? Ask the kernel.

That single idea explains why operating systems can safely run thousands of programs at once.


## Part 2) The Linux Filesystem
This is the first concept that makes Linux feel different from Windows.

## One tree, one root

Unlike Windows (`C:`, `D:`), Linux starts from a single root:

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22380%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20260%20380%22%20width%3D%22260%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20font-family%3D%22monospace%22%20font-size%3D%2218%22%20fill%3D%22currentColor%22%3E%3Ctext%20x%3D%22120%22%20y%3D%2224%22%20text-anchor%3D%22middle%22%3E%2F%3C%2Ftext%3E%3Cpath%20d%3D%22M120%2030%20V48%22%20stroke%3D%22currentColor%22%2F%3E%3Cpath%20d%3D%22M120%2048%20H50%20M120%2048%20H190%22%2F%3E%3Ctext%20x%3D%2220%22%20y%3D%2272%22%3Ehome%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%2272%22%3Ebin%3C%2Ftext%3E%3Cpath%20d%3D%22M120%2096%20H50%20M120%2096%20H190%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2224%22%20y%3D%22120%22%3Eetc%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%22120%22%3Eusr%3C%2Ftext%3E%3Cpath%20d%3D%22M120%20144%20H50%20M120%20144%20H190%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2224%22%20y%3D%22168%22%3Evar%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%22168%22%3Etmp%3C%2Ftext%3E%3Cpath%20d%3D%22M120%20192%20H50%20M120%20192%20H190%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2224%22%20y%3D%22216%22%3Edev%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%22216%22%3Eproc%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Every file, folder, and disk eventually appears somewhere under `/`.

Even USB drives become folders inside this tree.

## The eight directories that matter most

- ![Files | LinuxPhoneApps.org](https://images.openai.com/static-rsc-4/vNnilDMvSnuyZhkSiwDhXtx3Y01nVADSVYLkDX2R_AOeKg9cEt8rgpuiT5KFVrIntUIZlUFcd2Z9TqAXx4xLUb3Hzpl9B7nvhaOz7doa8YnxfS3k2apEbmWcTyHTli-0aMq8U8rBDq92sUq9-GkcjOgwzJm5RbfsNIf-KrA6MW0?purpose=inline)
    
    `/home` stores user files.
    
    Example: `/home/tanishq/Documents/project`.
    
- ![Entendendo a estrutura de pastas do Linux e Android | Android](https://images.openai.com/static-rsc-4/qYCae4lb4eiEtD0-vu-aKwG1Op9GIg8qbb-PIMOP5aU2naohDhg9w0lpD4z5lxo2n21-XTg6nsnGAzHcNyxDBJ0egEW6AknBFYeTMrU7ghQhvSfNEX7Ji-A8q-mrI0OOvCzyh7BuMfSGAU87LPhOUIGy6w4c3Iw5NsCHm_xAv9Q?purpose=inline)
    
    `/bin` contains essential programs like `ls`, `cp`, `mv`, and `cat`.
    
- ![Every Directory Has a Story. The Linux File Hierarchy | by Aadityakumar | Medium](https://images.openai.com/static-rsc-4/z-RB9QZeP_1YuysI_9xvgFVRbSdyugBAzYJ2nLt5LyD5u42CXyde5K4Y3m2dvoPkkIj6Y7uuSokI2JkvKTCeRxmTsAJhL5qT0N9MlEeMPRf3WFX3tIi6uo5MGmqAlwcpVd5gbCy5VbsPQ64xWxwSv8MfuqveYwjmO1J4YKnjOg4?purpose=inline)
    
    `/etc` stores system configuration files.
    
- ![How To Check Installed Software In Ubuntu Terminal](https://images.openai.com/static-rsc-4/GC4i2Z1taow9HP3KNaKprVtovBIlUxX4UDe3gXim0Tr-oH8axx6o2dSxfIJ3PWLTi1jVFUEEktp7ym6T9kpyV_7kU_QCY9CU4H05ClqICInDz3cfk5q21YD0EeW5-L6W1FSjLYM10nI3O8BG3yLONRb2yjZBgFtmf92hWVoJbEE?purpose=inline)
    
    `/usr` contains most installed software and libraries.
    
- ![ThinkEdge SE70: Preload OS Recovery Image, and Restore to Factory Reset process - Lenovo Support LV](https://images.openai.com/static-rsc-4/EPhDEKvoZTIZUqyn01Jqu6v_oh3WlAwgJojjfih_YDdWTFDpmBL9_fpnXEgzuDSU477lQVstHxeDn5Ff0XB6ut-2OEmXz9E5q4CgYiFYATnNFqow0glsdz-aS9iciNuJCyk3SL6D7S4vwhqRc5QDq0RPyz-hRzclQzEkIc8VSog?purpose=inline)
    
    `/var` stores changing data such as logs and caches.
    
- ![nautilus - How can I navigate to /tmp? - Ask Ubuntu](https://images.openai.com/static-rsc-4/9IjiRdQL-bP0MMvhJ-kIFtgoDjZyVmV_3YNqG3sIaOFM7FCzKZgt0PH7iEyrjwemJfzlFeNlR-HN0NPnXSRif2yu8HbNiFSkzG5qv4M5G1tOi-AXsSdf8EAEmfTFHQPQgSaBwz6EeCp9ymaC8SlFg5SHNQ26trpjTRKrUWFipSY?purpose=inline)
    
    `/tmp` holds temporary files that can disappear after a reboot.
    
- ![Linux File System - DEV Community](https://images.openai.com/static-rsc-4/jWPxl0SPR_lwFXZxuTIN90p9S_40obZRdMueyuMRlWmhNNvliOam7EcQHV3UGicl2oy8O4X4rtYjAimcnXGduY7LMMLTfUU6GWwh8DgAHR7nslBl_mJgxGcVJ3JNypPQjVL0a8DMXFTMd1ALMzlqQw0NuGjhd5iVqCe-uv5un6A?purpose=inline)
    
    `/dev` represents devices as files.
    
- ![How to use /proc filesystem in Linux for system monitoring | Dan Nanni posted on the topic | LinkedIn](https://images.openai.com/static-rsc-4/d2cS-KlTnF3d_hZ6GTDDzx65e7Fvo9uQV9R3VFBZLR2qzIA_kHFDIOm2nbCg7OFhdRxvqMt8jUSmaKcojwtcT84XR38kXkxubj3DsjaqPH2L7FlfbUcLx3rtw3d_1p_jsg-AK798lWlfH-CdZbYI2vYRxk1peBgeAOt3SBjgjxE?purpose=inline)
    
    `/proc` is a virtual filesystem exposing kernel and process information.
    

## The two "magic" folders

These surprise almost everyone.

### `/dev` (Devices are files)

Your SSD, keyboard, and USB drive appear as files.

Examples:

- `/dev/sda` → hard drive
    
- `/dev/null` → black hole file
    
- `/dev/tty` → terminal
    

This "everything is a file" philosophy makes Linux very powerful.

### `/proc` (The kernel talking to you)

`/proc` isn't stored on disk.

The kernel creates it dynamically.

Example:

```
cat /proc/cpuinfo
```

You're not opening a real file.

You're asking the kernel:

> "Tell me about the CPU."

The kernel generates the answer on demand.

## Visual memory trick

|Folder|Think of it as|
|---|---|
|`/home`|Your room|
|`/bin`|Toolbox|
|`/etc`|Settings|
|`/usr`|Shopping mall|
|`/var`|Diary that keeps changing|
|`/tmp`|Scratch paper|
|`/dev`|Device control panel|
|`/proc`|Live dashboard|
### In my words - Linux file system is a tree structure which means all the files come under one root file "/" all the files on the system comes under the root directory.
### the most important 8 directories are:
* /home - these is where user files are stored like documents, images, etc.
* /bin - contains all essential program like ls, pwd, cat, etc.
* /etc - stores all configuration files like settings .
* /usr - contains most installed software and libraries.
* /var - stores changing data such as logs and caches.
* /tmp - stores temporary files that gets removes when system reboots.
* /dev - represents devices as files.
* /proc - is a virtual filesystem expressing kernel and process information.

### all the commands are the program saved in the files that gets executed once the shell call from them kernel allocates memory and CPU to the process.  

## Part 3) mastering the shell
The objective is not memorizing commands. The objective is understanding that the shell is just another program that keeps track of your current working directory and launches other programs.

## The Current Working Directory (CWD)

Every shell process always has one location called the Current Working Directory.

Think of it as the room you're currently standing in.

```
/home/tanishq
```

When you type:

```
pwd
```

The shell asks the kernel, "Where am I?" and prints the answer.

## Relative vs Absolute Paths

This is one of the most important concepts in Linux.

### Absolute path

Starts from `/`.

```
/home/tanishq/project/main.py
```

No matter where you are, this points to the same file.

### Relative path

Starts from your current location.

If you're inside:

```
/home/tanishq
```

then:

```
cd project
```

means:

```
/home/tanishq/project
```

### Two special shortcuts

|Symbol|Meaning|
|---|---|
|`.`|Current directory|
|`..`|Parent directory|

Example:

```
cd ..
```

moves one level up.

## Essential Commands

We'll learn these through a mini project.

### Step 1: Create a workspace

```
mkdir systems-day1
cd systems-day1
```

`mkdir` creates a directory.

The shell asks the kernel to create a new directory entry.

### Step 2: Create files

```
touch notes.txt
touch todo.txt
```

`touch` creates an empty file.

Nothing magical happens.

The kernel simply creates a filesystem entry.

Check it:

```
ls
```

### Step 3: Read a file

```
cat notes.txt
```

Since it's empty, nothing appears.

Now add text:

```
echo "Linux Day 1" > notes.txt
cat notes.txt
```

What's happening?

- `echo` produces text.
    
- `>` redirects that text into a file.
    
- The shell handles the redirection.
    
- The kernel writes the bytes to disk.
    

### Step 4: Copy and Move

Copy:

```
cp notes.txt backup.txt
```

Move (or rename):

```
mv backup.txt archive.txt
```

Think carefully:

`mv` doesn't always move data.

If it's in the same folder, it often just changes the filename in the filesystem metadata.

Much faster.

### Step 5: Delete

```
rm archive.txt
```

Linux doesn't ask for confirmation.

The kernel removes the directory entry.

Recovery isn't guaranteed.

Treat `rm` with respect.

## Hidden Files

Run:

```
ls -a
```

You'll see files beginning with:

```
.
```

Example:

```
.bashrc
.gitignore
```

The dot means hidden, not encrypted.

They're usually configuration files.

## Command Chain

Here's the flow you've now built mentally:

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22120%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20720%20120%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cg%20font-family%3D%22Arial%22%20font-size%3D%2216%22%20fill%3D%22currentColor%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%3E%3Crect%20x%3D%2220%22%20y%3D%2230%22%20width%3D%2290%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%2265%22%20y%3D%2255%22%20text-anchor%3D%22middle%22%20stroke%3D%22none%22%3EYou%3C%2Ftext%3E%3Cpath%20d%3D%22M110%2050%20H150%22%2F%3E%3Cpolygon%20points%3D%22150%2C50%20138%2C44%20138%2C56%22%20fill%3D%22currentColor%22%20stroke%3D%22none%22%2F%3E%3Crect%20x%3D%22150%22%20y%3D%2230%22%20width%3D%22110%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22205%22%20y%3D%2255%22%20text-anchor%3D%22middle%22%20stroke%3D%22none%22%3EShell%3C%2Ftext%3E%3Cpath%20d%3D%22M260%2050%20H300%22%2F%3E%3Cpolygon%20points%3D%22300%2C50%20288%2C44%20288%2C56%22%20fill%3D%22currentColor%22%20stroke%3D%22none%22%2F%3E%3Crect%20x%3D%22300%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22360%22%20y%3D%2255%22%20text-anchor%3D%22middle%22%20stroke%3D%22none%22%3EKernel%3C%2Ftext%3E%3Cpath%20d%3D%22M420%2050%20H460%22%2F%3E%3Cpolygon%20points%3D%22460%2C50%20448%2C44%20448%2C56%22%20fill%3D%22currentColor%22%20stroke%3D%22none%22%2F%3E%3Crect%20x%3D%22460%22%20y%3D%2230%22%20width%3D%22120%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22520%22%20y%3D%2255%22%20text-anchor%3D%22middle%22%20stroke%3D%22none%22%3EDisk%3C%2Ftext%3E%3Cpath%20d%3D%22M580%2050%20H620%22%2F%3E%3Cpolygon%20points%3D%22620%2C50%20608%2C44%20608%2C56%22%20fill%3D%22currentColor%22%20stroke%3D%22none%22%2F%3E%3Crect%20x%3D%22620%22%20y%3D%2230%22%20width%3D%2280%22%20height%3D%2240%22%20rx%3D%228%22%20fill%3D%22none%22%2F%3E%3Ctext%20x%3D%22660%22%20y%3D%2255%22%20text-anchor%3D%22middle%22%20stroke%3D%22none%22%3EOutput%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Every file command follows this pattern.


### In my words - a shell is just another program that keeps track of the current working directory and the program that we need to run simultaneously. The current working directory is the directory that we are currently in and using there are 2 types of paths to the directory. 1 absolute which always starts with the root directory and till the directory that we are in right now 2 relative path that demonstrate the current location.
### some important commands 
* ls
* pwd
* cd
* mkdir
* touch
* cat
* echo
* cp
* mv
* rm


## 10:09:26 | 10:00 - 10:45 -- 11:15 - 11:50(80min)

Processes, PIDs, and Signals

Today's central question is:

> How can one CPU run Chrome, Spotify, VS Code, Docker, and dozens of terminal commands at the same time?

The answer is processes and the kernel scheduler.

By the end of this lesson you'll understand what a process actually is, how Linux creates one, how the CPU switches between them, and why `kill -9` is considered a last resort.

# 1. Program vs Process (5 min)

You already know:

- `/bin/ls` → Program (stored on disk)
    
- Running `ls` → Process (executing in memory)
    

Now let's go deeper.

## What changes when a program becomes a process?

Imagine `/bin/ls` sitting quietly on your SSD.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22220%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20700%20220%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2230%22%20y%3D%2250%22%20width%3D%22180%22%20height%3D%22120%22%20rx%3D%2215%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22120%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EProgram%3C%2Ftext%3E%3Ctext%20x%3D%22120%22%20y%3D%22120%22%20text-anchor%3D%22middle%22%20font-size%3D%2216%22%3E%2Fbin%2Fls%3C%2Ftext%3E%3Ctext%20x%3D%22120%22%20y%3D%22145%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3E\(SSD\)%3C%2Ftext%3E%3Cpath%20d%3D%22M230%20110%20H470%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22470%2C110%20455%2C102%20455%2C118%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%22490%22%20y%3D%2230%22%20width%3D%22180%22%20height%3D%22160%22%20rx%3D%2215%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22580%22%20y%3D%2260%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EProcess%3C%2Ftext%3E%3Ctext%20x%3D%22580%22%20y%3D%2290%22%20text-anchor%3D%22middle%22%20font-size%3D%2216%22%3EPID%3A%204321%3C%2Ftext%3E%3Ctext%20x%3D%22580%22%20y%3D%22115%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EMemory%20allocated%3C%2Ftext%3E%3Ctext%20x%3D%22580%22%20y%3D%22135%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3ECPU%20time%3C%2Ftext%3E%3Ctext%20x%3D%22580%22%20y%3D%22155%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EOpen%20files%3C%2Ftext%3E%3C%2Fsvg%3E)

The kernel creates a process control block (PCB), which is the process's identity card.

It contains information like:

|Field|Purpose|
|---|---|
|PID|Process ID|
|Memory|Where the program lives in RAM|
|Registers|CPU state|
|Open files|Files currently being used|
|Owner|Which user started it|
|State|Running, sleeping, stopped|

This PCB is how the kernel remembers what every process is doing.

# 2. What happens internally when you run a program? (5 min)

Let's trace:

```
sleep 60
```

Instead of just saying "it runs," here's what actually happens.

## Step-by-step

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22500%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20500%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2240%22%20y%3D%2220%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%2248%22%20font-size%3D%2218%22%3E1.%20You%20type%3A%20sleep%2060%3C%2Ftext%3E%3Cpath%20d%3D%22M380%2064%20V92%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22380%2C102%20372%2C90%20388%2C90%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22102%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22130%22%20font-size%3D%2218%22%3E2.%20Shell%20finds%20%2Fbin%2Fsleep%3C%2Ftext%3E%3Cpath%20d%3D%22M380%20146%20V174%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22380%2C184%20372%2C172%20388%2C172%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22184%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22212%22%20font-size%3D%2218%22%3E3.%20Shell%20calls%20the%20kernel%20using%20execve\(\)%3C%2Ftext%3E%3Cpath%20d%3D%22M380%20228%20V256%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22380%2C266%20372%2C254%20388%2C254%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22266%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22294%22%20font-size%3D%2218%22%3E4.%20Kernel%20creates%20a%20new%20process%20\(new%20PID\)%3C%2Ftext%3E%3Cpath%20d%3D%22M380%20310%20V338%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22380%2C348%20372%2C336%20388%2C336%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22348%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22376%22%20font-size%3D%2218%22%3E5.%20Program%20is%20loaded%20into%20RAM%3C%2Ftext%3E%3Cpath%20d%3D%22M380%20392%20V420%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22380%2C430%20372%2C418%20388%2C418%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22430%22%20width%3D%22680%22%20height%3D%2244%22%20rx%3D%2210%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%22458%22%20font-size%3D%2218%22%3E6.%20Scheduler%20gives%20it%20CPU%20time%20until%20it%20exits%3C%2Ftext%3E%3C%2Fsvg%3E)

Notice something interesting.

The shell does not become `sleep`.

Instead, the shell creates another process and waits for it.

That's why, after `sleep` finishes, your prompt comes back.

# 3. Every process gets a PID (5 min)

A PID is simply a unique number.

Example:

|PID|Process|
|---|---|
|1|systemd|
|245|sshd|
|1024|bash|
|4310|sleep|

Think of PIDs like employee IDs inside a company.

The kernel uses them to identify exactly which process you're referring to.

## Find your shell's PID

Run:

```
echo $$
```

Example output:

```
4312
```

`$$` is a shell variable containing the current shell's PID.

That's the process you're typing into right now.

# 4. Viewing running processes (7 min)

## `ps`

Run:

```
ps
```

Example:

```
PID   TTY      TIME CMD
4312  pts/0    00:00 bash
4398  pts/0    00:00 ps
```

Meaning:

|Column|Meaning|
|---|---|
|PID|Process ID|
|TTY|Terminal|
|TIME|CPU time used|
|CMD|Command|

Notice something funny.

`ps` appears in its own output.

Why?

Because it became a process while listing processes.

## `ps -ef`

Run:

```
ps -ef
```

Now you'll see hundreds of processes.

Important columns:

|Column|Meaning|
|---|---|
|UID|Owner|
|PID|Process|
|PPID|Parent Process|
|CMD|Command|

The PPID introduces a crucial idea.

# Parent and Child Processes

Example:

```
bash (PID 4312)
    |
    ├── ls
    ├── sleep
    └── python
```

Every process is usually created by another process.

This forms a process tree.

View it:

```
pstree
```

If unavailable:

```
ps -ef --forest
```

You'll literally see processes branching like a tree.

# 5. The Scheduler: One CPU, Many Programs (5 min)

This is one of the biggest misconceptions beginners have.

Your CPU usually isn't running everything simultaneously.

Instead, Linux rapidly switches between processes.

Imagine four processes:

- Chrome
    
- VS Code
    
- Spotify
    
- Terminal
    

The scheduler does something like this:

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22140%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20720%20140%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Ctext%20x%3D%2220%22%20y%3D%2230%22%20font-size%3D%2218%22%3ECPU%20timeline%3C%2Ftext%3E%3Cline%20x1%3D%2220%22%20y1%3D%2270%22%20x2%3D%22700%22%20y2%3D%2270%22%20stroke%3D%22currentColor%22%20stroke-width%3D%224%22%2F%3E%3Cg%20font-size%3D%2214%22%20text-anchor%3D%22middle%22%3E%3Crect%20x%3D%2220%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%2260%22%20y%3D%2275%22%3EChrome%3C%2Ftext%3E%3Crect%20x%3D%22100%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22140%22%20y%3D%2275%22%3EVS%20Code%3C%2Ftext%3E%3Crect%20x%3D%22180%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22220%22%20y%3D%2275%22%3ETerminal%3C%2Ftext%3E%3Crect%20x%3D%22260%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22300%22%20y%3D%2275%22%3ESpotify%3C%2Ftext%3E%3Crect%20x%3D%22340%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22380%22%20y%3D%2275%22%3EChrome%3C%2Ftext%3E%3Crect%20x%3D%22420%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22460%22%20y%3D%2275%22%3EVS%20Code%3C%2Ftext%3E%3Crect%20x%3D%22500%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22540%22%20y%3D%2275%22%3ETerminal%3C%2Ftext%3E%3Crect%20x%3D%22580%22%20y%3D%2250%22%20width%3D%2280%22%20height%3D%2240%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22620%22%20y%3D%2275%22%3ESpotify%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)

Each switch is called a context switch.

The kernel saves one process's CPU registers and restores another's.

This happens thousands of times every second.

That's why everything feels simultaneous.

# 6. Live Monitoring (3 min)

Run:

```
top
```

![Observing Processes and Threads in Linux | by Faiz Nabil | Operating System CSCI 3300 | Medium](https://images.openai.com/static-rsc-4/qRfGYL5bQTRyNViVkApQ_G1jkTzkv7WYyn8ch6EPotb_AYY2R134g4TMtrXY1sHUoSV4mCTbdfUmR8xx1uf6Yo6g0ko82siJ158CQmtyTgheCLOW0lUd_QxbBJ9Z-hJdHIlU7ODyK3n_E80YoTWq-Uh2laYjucrVcBQ_ya3SVJA?purpose=inline)

![Linux: Compiling software from source code | by Hanlly S. | Medium](https://images.openai.com/static-rsc-4/5CQT_x325T6dSAiJGRwhGDRK3j5CShShpi9RJwMPhayRbAyReG0k5hniEOwZEWGWXTCdBtgimowYrtbVtJaKHI4x9f1s2Lo-2b2uFJrtSENd_5RkLCHGDZBLZj2YMP6hcdaiIU8aYJeFo9qEFuJcdjGCeone8W_UKyezRXJGfhg?purpose=inline)

![5 Best Terminal-Based Linux Monitoring Tools](https://images.openai.com/static-rsc-4/Tir_c-PAAIlokaYQ4OylBtFSrOjC-8qTUwGIrzjrLMl7Ifno0oQGW05PGAUwbfR97Mpi73aKCY-tBNhQzD_9RSG-QaJEfqeJswXugF-bNYCxkireti68whSNZnk9LqqsBamKl0UG4FPUtRofHmg9VJ6TJ8kViJCdnT4u3durXmI?purpose=inline)

4

You'll see:

- CPU usage
    
- Memory usage
    
- Running processes
    
- Sleeping processes
    

Think of `top` as a live dashboard for your operating system.

Later we'll use `htop`, which is an improved version.

# 7. Signals: How Linux Stops Processes (5 min)

A process doesn't usually "die."

It receives a signal.

A signal is a message from the kernel.

Common signals:

|Signal|Number|Meaning|
|---|---|---|
|SIGTERM|15|Please exit cleanly|
|SIGKILL|9|Stop immediately|
|SIGINT|2|Ctrl+C|

## Example

Start:

```
sleep 100
```

In another terminal:

```
ps -ef | grep sleep
```

Find its PID.

Suppose it's:

```
5021
```

Now:

```
kill 5021
```

This sends SIGTERM.

The process gets a chance to clean up.

### Force kill

```
kill -9 5021
```

This sends SIGKILL.

The kernel immediately removes the process.

No cleanup.

No saving.

That's why experienced engineers avoid `kill -9` unless necessary.

# 8. Foreground vs Background Processes

Normal:

```
sleep 30
```

Terminal waits.

Background:

```
sleep 30 &
```

Example output:

```
[1] 5042
```

The shell immediately returns.

Useful commands:

|Command|Purpose|
|---|---|
|`jobs`|Show background jobs|
|`fg`|Bring a job back|
|`bg`|Resume a stopped job in background|

This becomes extremely useful when working over SSH.

# Mini Practical (10 minutes)

Run these commands in order:

```
echo $$
sleep 60 &
jobs
ps
ps -ef | grep sleep
kill <PID>
jobs
```

Observe what changes after each command.

# Mental Model You'll Reuse for the Next Year

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22360%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20760%20360%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2240%22%20y%3D%2230%22%20width%3D%22160%22%20height%3D%2260%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22120%22%20y%3D%2265%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EProgram%20on%20disk%3C%2Ftext%3E%3Cpath%20d%3D%22M120%2090%20V120%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22120%2C130%20112%2C118%20128%2C118%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22130%22%20width%3D%22160%22%20height%3D%2260%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22120%22%20y%3D%22165%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EShell%20launches%20it%3C%2Ftext%3E%3Cpath%20d%3D%22M120%20190%20V220%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22120%2C230%20112%2C218%20128%2C218%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22230%22%20width%3D%22160%22%20height%3D%2260%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22120%22%20y%3D%22265%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EKernel%20creates%3C%2Ftext%3E%3Ctext%20x%3D%22120%22%20y%3D%22283%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3Ea%20process%3C%2Ftext%3E%3Cpath%20d%3D%22M200%20260%20H300%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22300%2C260%20288%2C252%20288%2C268%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%22300%22%20y%3D%22210%22%20width%3D%22180%22%20height%3D%22100%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22390%22%20y%3D%22240%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EScheduler%20gives%3C%2Ftext%3E%3Ctext%20x%3D%22390%22%20y%3D%22262%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ECPU%20time%3C%2Ftext%3E%3Cpath%20d%3D%22M480%20260%20H580%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22580%2C260%20568%2C252%20568%2C268%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%22580%22%20y%3D%22210%22%20width%3D%22140%22%20height%3D%22100%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%2F%3E%3Ctext%20x%3D%22650%22%20y%3D%22240%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ESignal%3C%2Ftext%3E%3Ctext%20x%3D%22650%22%20y%3D%22262%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3Eends%20process%3C%2Ftext%3E%3C%2Fsvg%3E)

This exact lifecycle scales upward:

- Today: `sleep 60`
    
- Next month: Python servers
    
- Later: Docker containers
    
- After that: Kubernetes Pods
    
- Eventually: your distributed cloud where thousands of processes run across multiple machines.


## 21/09/26  2:30 - 3:15 -- 04:00 - 04:30 -- 04:45 - 

### Lets reverse engineer our node agent
#### what the node agent will do primarily
well it will need to tell about itself I mean its resources what is it able to contribute to the cause.
Node ID Which will be auto generated and will be unique.
|Information                 Why it matters|

|Node ID                    Unique identity|
|CPU cores                 Parallel work capacity|
|CPU usage                Current load|
|Total RAM                 Maximum memory available|
|Free RAM                  Can this task fit?|
|Disk space                 Can we store temporary files?|
|OS                              Compatibility|
|Architecture                x86 vs ARM|
|Uptime                        Stability|
|Running tasks             Avoid overload|

### ==Design Decision #1==

==Not all information changes at the same speed.==

==Split it into two categories.==

==![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22260%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20700%20260%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2240%22%20y%3D%2240%22%20width%3D%22260%22%20height%3D%22160%22%20rx%3D%2215%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22170%22%20y%3D%2270%22%20text-anchor%3D%22middle%22%20font-size%3D%2220%22%3EStatic%3C%2Ftext%3E%3Ctext%20x%3D%2260%22%20y%3D%22100%22%20font-size%3D%2216%22%3E%E2%80%A2%20Node%20ID%3C%2Ftext%3E%3Ctext%20x%3D%2260%22%20y%3D%22125%22%20font-size%3D%2216%22%3E%E2%80%A2%20OS%3C%2Ftext%3E%3Ctext%20x%3D%2260%22%20y%3D%22150%22%20font-size%3D%2216%22%3E%E2%80%A2%20CPU%20cores%3C%2Ftext%3E%3Ctext%20x%3D%2260%22%20y%3D%22175%22%20font-size%3D%2216%22%3E%E2%80%A2%20Architecture%3C%2Ftext%3E%3Crect%20x%3D%22400%22%20y%3D%2240%22%20width%3D%22260%22%20height%3D%22160%22%20rx%3D%2215%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22530%22%20y%3D%2270%22%20text-anchor%3D%22middle%22%20font-size%3D%2220%22%3EDynamic%3C%2Ftext%3E%3Ctext%20x%3D%22420%22%20y%3D%22100%22%20font-size%3D%2216%22%3E%E2%80%A2%20CPU%20usage%3C%2Ftext%3E%3Ctext%20x%3D%22420%22%20y%3D%22125%22%20font-size%3D%2216%22%3E%E2%80%A2%20Free%20RAM%3C%2Ftext%3E%3Ctext%20x%3D%22420%22%20y%3D%22150%22%20font-size%3D%2216%22%3E%E2%80%A2%20Running%20tasks%3C%2Ftext%3E%3Ctext%20x%3D%22420%22%20y%3D%22175%22%20font-size%3D%2216%22%3E%E2%80%A2%20Uptime%3C%2Ftext%3E%3C%2Fsvg%3E)==

==Why?==

==Because static information only needs to be collected once.==

==Dynamic information belongs in future heartbeat packets.==

==That reduces unnecessary work.==

==This is our first systems optimization.==


# Step 2: Where does each piece come from?

Now we investigate Linux itself.

Here's our investigation board.

|Data|Linux Source|
|---|---|
|CPU cores|`/proc/cpuinfo`|
|Memory|`/proc/meminfo`|
|Uptime|`/proc/uptime`|
|Running processes|`/proc/<PID>`|
|Disk|`statfs()` system call|
|Hostname|`/etc/hostname`|

Notice something beautiful.

The kernel isn't hiding information.

It's publishing it.

Our Node Agent is basically an interpreter.

# Step 3: Is `/proc` a real folder?

This is today's biggest concept.

Run this thought experiment.

Imagine your laptop has:

- 300 running processes.

Inside `/proc`, you'll find:

```
/proc/1
/proc/54
/proc/120
/proc/4312
/proc/9000
...
```

Now kill process `9000`.

The folder disappears instantly.

Ask yourself:

> Did the SSD physically delete a folder?

Probably not.

Exactly.

`/proc` is a virtual filesystem.

The kernel creates it on demand.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20height%3D%22320%22%20style%3D%22color%3Argb\(255%2C%20255%2C%20255\)%22%20viewBox%3D%220%200%20720%20320%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2260%22%20y%3D%2240%22%20width%3D%22240%22%20height%3D%2290%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22180%22%20y%3D%2275%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3EKernel%20State%3C%2Ftext%3E%3Ctext%20x%3D%22180%22%20y%3D%22100%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EProcesses%2C%20Memory%2C%20CPU%3C%2Ftext%3E%3Cpath%20d%3D%22M300%2085%20H420%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22420%2C85%20406%2C77%20406%2C93%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%22420%22%20y%3D%2240%22%20width%3D%22240%22%20height%3D%2290%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22540%22%20y%3D%2275%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3E%2Fproc%3C%2Ftext%3E%3Ctext%20x%3D%22540%22%20y%3D%22100%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EGenerated%20while%20you%20read%20it%3C%2Ftext%3E%3Cpath%20d%3D%22M540%20130%20V190%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Cpolygon%20points%3D%22540%2C204%20532%2C188%20548%2C188%22%20fill%3D%22currentColor%22%2F%3E%3Crect%20x%3D%22360%22%20y%3D%22205%22%20width%3D%22360%22%20height%3D%2280%22%20rx%3D%2212%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-width%3D%222%22%2F%3E%3Ctext%20x%3D%22540%22%20y%3D%22235%22%20text-anchor%3D%22middle%22%20font-size%3D%2218%22%3ENode%20Agent%3C%2Ftext%3E%3Ctext%20x%3D%22540%22%20y%3D%22258%22%20text-anchor%3D%22middle%22%20font-size%3D%2214%22%3EReads%20%2Fproc%20like%20ordinary%20files%3C%2Ftext%3E%3C%2Fsvg%3E)

This is one of Linux's greatest design ideas:

> Everything looks like a file.

Even when it isn't.

# Step 4: How will our Resource Monitor work?

Instead of coding, let's design the algorithm.

### CPU Information

Question:

Should we ask the kernel every second?

Not necessarily.

Number of CPU cores rarely changes.

Algorithm:

```
Startup
    ↓
Read CPU information
    ↓
Cache it
    ↓
Reuse forever
```

### Memory Information

Memory changes constantly.

Algorithm:

```
Heartbeat
    ↓
Read /proc/meminfo
    ↓
Parse values
    ↓
Send current numbers
```

Different data deserves different refresh rates.

That's another systems principle.

# Step 5: Our First C Tool (Next Session Preview)

This is where C enters naturally.

Instead of Java hiding everything,

we'll write something tiny.

Imagine:

```
./inspect_cpu
```

Output:

```
CPU Model : Intel...
Cores     : 8
Threads   : 8
```

But here's the twist.

We won't use a special CPU library.

We'll literally open:

```
/proc/cpuinfo
```

using C.

That means you'll finally use:

- `open()`
    
- `read()`
    
- `close()`
    

for a real project feature.

Those aren't textbook functions anymore.

They're how the Node Agent learns about its own machine.

Later, we'll recreate the same feature in Java and compare both implementations.

# Step 6: Cross-Team Feature Alignment

To keep every subject synchronized, here's the first dependency map.

|OS Team|Produces|Used By|
|---|---|---|
|Resource Monitor|Node resources|Software Dashboard|
|Node Identity|Node ID|Networking Registration|
|Process Monitor|Running tasks|Scheduler|
|Logging|Log files|Dashboard|

|Networking|Produces|Used By|
|---|---|---|
|TCP Client|Node connection|OS Agent|
|Heartbeat Packet|Live updates|Dashboard|
|Registration|Node discovery|Architecture|

|Architecture|Produces|Used By|
|---|---|---|
|Heartbeat protocol|Packet format|Networking|
|Scheduler rules|Task assignment|OS|
|Failure handling|Recovery logic|Software|

Notice how no team duplicates another's work.


QnA
1) Why did linux choose to represent cpuinfo as a file or any tool as a readable file.
 => linux had few options it could create a new command like cpuinfo or it could create a special api every program must learn or it could make a normal file with all the information regrading cpu.
 why choose c : consider there are the programs that need cpuinfo if e make a api all programs would need to wrap it but if we make a file each program can read that file **ONE interface thousand of programs**. 
 TO make tools reusable linux represents them as a file.

2) Is /proc/cpuinfo actually saved on the disk?
 =>  imagine you need memory info now memory is dynamic it changes at each instance if would need to store and modify this value it would not be feasible at all. now program meminfo cat /proc/meminfo The kernal has current memory state the kernal creates the text only when someone asks for it the program converts into a process and can be calculated at written in file only when we need to read it.  **That's why it is called the virtual filesystem** 
# A filesystem isn't always a disk

Most beginners think:

> Filesystem = folders on an SSD.

Operating systems think differently.

A filesystem is simply:

> A way to present information as files.

Examples:

|Filesystem|Stores|
|---|---|
|NTFS|Windows files|
|ext4|Linux files|
|FAT32|USB drives|
|procfs|Running system information|
|sysfs|Hardware information|

Notice that procfs stores almost nothing permanently.

It's a live window into the kernel.

