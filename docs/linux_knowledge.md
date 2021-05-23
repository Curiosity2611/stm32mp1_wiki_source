# Linux Introduction 

Welcome to Introduction to Linux section! Here you’ll find some general linux concepts which will come in handy when we dive deeper into working with STM32MP1 DK. 

## Common Linux Commands

### The Shell

The shell is basically a program that takes your commands from the keyboard and sends them to the operating system to perform. If you’ve ever used a GUI, you’ve probably seen programs such as “Terminal” or “Console” these are just programs that launch a shell for you. <br>

Depending on the distribution your shell prompt might change, but for the most part it should adhere to the following format:

    username@hostname:current_directory 
    deepak@STM32MP1:/home $

Notice the $ at the end of the prompt? Different shells will have different prompts, in our case:

- the $ is for a normal user
- the # is for a root user

Let’s start with a simple command, echo. The echo command just prints out the text arguments to the display.

    $ echo Hello World
    Hello World

### pwd (Print Working Directory)

Everything in Linux is a file. Every file is organized in a hierarchical directory tree. The first directory in the filesystem is aptly named the root directory. The root directory has many folders and files which you can store more folders and files, etc. Here is an example of what the directory tree looks like:

    /

    |-- bin

    |   |-- file1

    |   |-- file2

    |-- etc

    |   |-- file3

    |   -- directory1

    |       |-- file4

    |       -- file5

    |-- home

    |-- var

The location of these files and directories are referred to as paths. If you had a folder named `home` with a folder inside of it named `etc` and another folder in that folder called `sys`, that path would look like this: `/home/etc/sys` <br>

Navigation of the filesystem, much like real life is helpful if you know where you are and where you are going. To see where you are, you can use the `pwd` command, this command means “print working directory” and it just shows you which directory you are in, note the path stems from the root directory.

    $pwd
    /home/root

### cd (Change Directory)

Now that you know where you are, let’s see if we can move around the filesystem a bit. There are two different ways to specify a path, with absolute and relative paths.

- ** Absolute path **: This is the path from the root directory. The root is the head or parent directory. The root directory is commonly shown as a slash `/`. Every time your path starts with `/` it means you are starting from the root directory. For example, `/home/deepak/Desktop`.

- ** Relative path **: This is the path from where you are currently in filesystem. If I was in location `/home/deepak/Documents` and wanted to get to a directory inside Documents called `files`, I don’t have to specify the whole path from root like `/home/deepak/Documents/files`, I can just go to `files/` instead.

        cd /home/root

It can get pretty tiring navigating with absolute and relative paths all the time, luckily there are some shortcuts to help you out.

- `.` (current directory). This is the directory you are currently in.
- `..` (parent directory). Takes you to the directory above your current.
- `~` (home directory). This directory defaults to your “home directory”. Such as /home/deepak.
- `-` (previous directory). This will take you to the previous directory you were just at.
- `cd` (home directory). This is another way to go to home directory. 

        $ cd .
        $ cd ..
        $ cd ~
        $ cd -

### ls (List Directories)

Now that we know how to move around the system, how do we figure out what is available to us? The `ls` command will list directories and files in the current directory by default, however you can specify which path you want to list the directories of.

    $ ls
    bin   dev  home  lost+found  mnt   root  sbin  tmp  var
    boot  etc  lib	 media	     proc  run	 sys   usr  vendor

    $ ls /home/deepak
    example_file_1  example_file_2

Also note that not all files in a directory will be visible. Filenames that start with `.` are hidden, you can view them however with the `ls` command and pass the `-a` flag to it (a for all).

    $ ls -a

There is also one more useful `ls` flag, `-l` for long, this shows a detailed list of files in a long format. This will show you detailed information, starting from the left: `file permissions`, `number of links`, `owner name`, `owner group`, `file size`, `timestamp of last modification`, and `file/directory name`

    $ ls -l
    total 37
    drwxr-xr-x   2 root root  5120 Mar  9  2018 bin
    drwxr-xr-x  18 root root  1024 Mar 15  2019 boot
    drwxr-xr-x  13 root root 13840 Apr  9 02:28 dev
    drwxr-xr-x  44 root root  4096 Mar  2  2019 etc
    drwxr-xr-x   3 root root  1024 Mar  9  2018 home
    drwxr-xr-x  12 root root  4096 Mar  9  2018 lib
    drwx------   2 root root 12288 Mar 15  2019 lost+found
    drwxr-xr-x   2 root root  1024 Mar  9  2018 media
    drwxr-xr-x   2 root root  1024 Mar  9  2018 mnt
    dr-xr-xr-x 133 root root     0 Jan  1  1970 proc
    drwxr-xr-x   5 root root  1024 Apr  8 07:48 root
    drwxr-xr-x  13 root root   380 Apr  9 02:29 run
    drwxr-xr-x   3 root root  4096 Mar  9  2018 sbin
    dr-xr-xr-x  12 root root     0 Jan  1  2000 sys
    drwxrwxrwt   7 root root   180 Apr  9 09:43 tmp
    drwxr-xr-x  11 root root  1024 Mar  9  2018 usr
    drwxr-xr-x   9 root root  1024 Mar  2  2019 var
    drwxr-xr-x   4 root root  1024 Mar 15  2019 vendor

Commands have things called flags (or arguments or options, whatever you want to call it) to add more functionality. You can add them both together. For example, `-la`. The order of the flags determines which order it goes in. You can also do ls `-al` and it would still work. <br>

Run `ls` with different flags and see the output you receive.

- `ls -R`: recursively list directory contents
- `ls -r`: reverse order while sorting
- `ls -t`: sort by modification time, newest first

### touch 

Touch allows you to the create new empty files.

    $ touch mysuperduperfile

Touch is also used to change timestamps on existing files and directories. Give it a try, do an `ls -l` on a file and note the timestamp, then `touch` that file and it will update the timestamp.

### file

We learned about touch but did you notice that the filename didn’t conform to standard naming like you’ve probably seen with other operating systems like Windows? Normally you would expect a file called banana.jpeg and expect a JPEG picture file. <br>

In Linux, filenames aren’t required to represent the contents of the file. You can create a file called `funny.gif` that isn’t actually a GIF.

To find out what kind of file a file is, you can use the file command. It will show you a description of the file’s contents.

    $ file Documents
    Documents: directory

### cat

Let’s learn how to read a file. A simple command to use is the cat command, short for concatenate, it not only displays file contents but it can combine multiple files and show you the output of them.

    $ cat dogfile birdfile

!!! Tip 
    It’s not great for viewing large files and it’s only meant for short content. There are many other tools that we use to view larger text files that we’ll discuss later. 

### less

If you are viewing text files larger than a simple output then you can use `less` command. The text is displayed in a paged manner, so you can navigate through a text file page by page.<br>

Go ahead and look at the contents of a file with less. Once you’re in the less command, you can actually use other keyboard commands to navigate in the file.

    $ less /home/deepak/Documents/text1

Use the following command to navigate through less:

- `q` - Used to quit out of less and go back to your shell.
- `Page up`, `Page down`, `Up` and `Down` - Navigate using the arrow keys and page keys.
- `g` - Moves to beginning of the text file.
- `G` - Moves to the end of the text file.
- `/sometexttosearch` - You can search for specific text inside the text document. Prefacing the words you want to search with `/`
- `h` - If you need a little help about how to use less while you’re in less, use help.

### history

In your shell, there is a history of the commands that you previously entered, you can actually look through these commands. This is quite useful when you want to find and run a command you used previously without actually typing it again.<br>

    $ history
Want to run the same command you did before, just hit the up arrow.

Want to run the previous command without typing it again? Use `!!` If you typed `cat file1` and want to run it again, you can actually just go `!!` and it will run the last command. <br>

Another history shortcut is `ctrl-R`, this is the reverse search command, if you hit `ctrl-R` and you start typing parts of the command you want it will show you matches and you can just navigate through them by hitting the `ctrl-R` key again. Once you found the command you want to use again, just hit the `Enter` key. <br>

!!! Tip
    If the terminal window gets cluttered, use the `$ clear` command to clear up your display.


While we are talking about useful things, one of the most useful features in any command-line environment is tab completion. If you start typing the beginning of a command, file, directory, etc and hit the Tab key, it will autocomplete based on what it finds in the directory you are searching as long as you don’t have any other files that start with those letters. For example if you were trying to run the command chrome, you can type chr and press Tab and it will autocomplete chrome.

### cp (Copy)

Use copy command to copy files from one location to other location. 

    $ cp mycoolfile /home/deepak/documents/cooldocs
`mycoolfile` is the file you want to copy and `/home/deepak/Documents/cooldocs` is where you are copying the file to.

** Wildcard ** - A wildcard is a character that can be substituted for a pattern based selection, giving you more flexibility with searches. You can use wildcards in every command for more flexibility.

- `*` the wildcard of wildcards, it's used to represent all single characters or any string.
- `?` used to represent one character
- `[]` used to represent any character within the brackets

        $ cp *.jpg /home/deepak/Pictures

This will copy all files with the .jpg extension in your current directory to the Pictures directory.

** Copying Directories ** <br>
A useful command is to use the `-r` flag, this will recursively copy the files and directories within a directory.

    $ cp -r Pumpkin/ /home/deepak/Documents

One thing to note, if you copy a file over to a directory that has the same filename, the file will be overwritten with whatever you are copying over. Inorder to avoid that you can use the `-i` flag (interactive) to prompt you before overwriting a file.

    $ cp -i mycoolfile /home/deepak/Pictures

### mv (Move)

Used for moving files and also renaming them. Quite similar to the `cp` command in terms of flags and functionality. <br>

** Rename File **
You can rename files like this:

    $ mv oldfile newfile

Or you can actually move a file to a different directory:

    $ mv file2 /home/deepak/Documents

And move more than one file:

    $ mv file_1 file_2 /somedirectory

** Rename Directory **
You can rename directories as well:

    $ mv directory1 directory2

Like `cp`, if you `mv` a file or directory it will overwrite anything in the same directory. So you can use the `-i` flag to prompt you before overwriting anything.

    $ mv -i directory1 directory2

** Create a Backup File **   
Let’s say you did want to `mv` a file to overwrite the previous one. You can also make a backup of that file and it will just rename the old version with a `~`

    $ mv -b directory1 directory2

### mkdir (Make Directory)

We might need some directories to store all the files we’ve been working on. The `mkdir` command (Make Directory) is useful for that, it will create a directory if it doesn’t exist. <br>

** Multiple Directories ** <br>

You can even make multiple directories at the same time.

    $ mkdir books paintings

** Sub Directories **   

You can also create subdirectories at the same time with the `-p` (parent flag)

    $ mkdir -p books/hemmingway/favorites

### rm (Remove)

To remove files you can use the `rm` command. The `rm` (remove) command is used to delete files and directories.

    $ rm file1

!!! Warning
    Take caution when using `rm`, you cannot undo once done. Be careful. 

Fortunately there are some safety measures put into place, so you can’t just remove a bunch of important files. Write-protected files will prompt you for confirmation before deleting them. If a directory is write-protected it will also not be easily removed. <br>

Now if you don’t care about any of that, you can absolutely remove a bunch of files.

    $ rm -f file1
`-f` or force option tells `rm` to remove all files, whether they are write protected or not, without prompting the user (as long as you have the appropriate permissions).

    $ rm -i file
Adding the `-i` flag like many of the other commands, will give you a prompt on whether you want to actually remove the files or directories.

    $ rm -r directory
You can’t just `rm` a directory by default, you’ll need to add the `-r` flag (recursive) to remove all the files and any subdirectories it may have. <br>

You can remove a directory with the rmdir command.

    $ rmdir directory

### find

With all the files we have on the system it can get a little hectic trying to find a specific one. Well there’s a command we can use for that, `find`

    $ find /home -name puppies.jpg

With `find` you’ll have to specify the directory you’ll be searching it, what you’re searching for, in this case we are trying to find a file by the name of `puppies.jpg`

You can specify what type of file you are trying to find.

    $ find /home -type d -name MyFolder

You can see that I set the type of file I’m trying to find as `d` for directory and I’m still searching by the name of `MyFolder`.

One cool thing to note is that find doesn’t stop at the directory you are searching, it will look inside any subdirectories that directory may have as well.

### help

Linux has some great built-in tools to help you how to use a command or check what flags are available for a command. One tool, `help`, is a built-in bash command that provides help for other bash commands (echo, logout, pwd, etc).

    $ help echo

This will give you a description and the options you can use when you want to run `echo`. For other executable programs, it’s convention to have an option called `--help` or something similar.

    $ echo --help

Not all developers who ship out executables will conform to this standard, but it’s probably your best shot to find some help on a program.

### man 

You can see the manuals for a command with the `man` command.

    $ man ls

Man pages are manuals that are by default built into most Linux operating systems. They provide documentation about commands and other aspects of the system.

### whatis

If you are ever feeling doubtful about what a command does, you can use the `whatis` command. The `whatis` command provides a brief description of command line programs.

    $ whatis cat

The description gets sourced from the manual page of each command. If you ran `whatis cat`, you’d see there is a small blurb with a short description.


### alias

Sometimes typing commands can get really repetitive, or if you need to type a long command many times, it’s best to have an alias you can use for that. To create an alias for a command you simply specify an alias name and set it to the command.

    $ alias foobar='ls -la'

Now instead of typing `ls -la`, you can type `foobar` and it will execute that command. Keep in mind that this command won't save your alias after reboot, so you'll need to add a permanent alias in:

`~/.bashrc` 
or similar files if you want to have it persist after reboot. <br>

You can remove aliases with the unalias command:

    $ unalias foobar

### exit

To exit from the shell, you can use the `exit` command

    $ exit

Or the `logout` command

    $ logout

Or if you are working out of a terminal GUI, you can just close the terminal.


## Working with Text

### stdout (Standard Out)

Use `echo` to print something on the screen. 

    $ echo Hello World

We know this prints out Hello World to the screen, but how? Processes use I/O streams to receive input and return output. By default the echo command takes the input (standard input or stdin) from the keyboard and returns the output (standard output or stdout) to the screen. So that's why when you type echo Hello World in your shell, you get Hello World on the screen. However, I/O redirection allows us to change this default behavior giving us greater file flexibility.

    > 

The > is a redirection operator that allows us the change where standard output goes. It allows us to send the output of echo Hello World to a file instead of the screen. If the file does not already exist it will create it for us. However, if it does exist it will overwrite it (you can add a shell flag to prevent this depending on what shell you are using). <br>

And that's basically how stdout redirection works!

    $ echo Hello World > peanuts.txt

What just happened? Well check the directory where you ran that command and you should see a file called peanuts.txt, look inside that file and you should see the text Hello World. Lots of things just happened in one command so let's break it down.

!!! Warning 
     If the file already exist it will overwrite it the file with the new file. You can always use the shell flag `-i` to warn you before overwriting. 

If you just want to append the test to the file then use `>>` instead of `>`
    
    $ echo Hello World >> peanuts.txt

This will append Hello World to the end of the peanuts.txt file, if the file doesn't already exist it will create it for us like it did with the `>` redirector!


### stdin (Standard In)

There are different standard input (stdin) streams. We know that we have stdin from devices like the keyboard, but we can use files, output from other processes and the terminal as well, let's see an example.

Let's say we have a peanuts.txt file and it has the text Hello World in it.

    $ cat < peanuts.txt > banana.txt 
Just like we had `>` for stdout redirection, we can use `<` for stdin redirection.

Normally in the cat command, you send a file to it and that file becomes the stdin, in this case, we redirected peanuts.txt to be our stdin. Then the output of cat peanuts.txt which would be Hello World gets redirected to another file called banana.txt, and if banana.txt file doesn't exist, it will be created.

### grep

The grep command is quite possibly the most common text processing command you will use. It allows you to search files for characters that match a certain pattern. What if you wanted to know if a file existed in a certain directory or if you wanted to see if a string was found in a file? You certainly wouldn't dig through every line of text, you would use grep!

Let's use our `sample.txt` file as an example:

    $ grep fox sample.txt

You should see that grep found fox in the `sample.txt` file.

You can also grep patterns that are case insensitive with the `-i` flag:

    $ grep -i somepattern somefile

To get even more flexible with `grep` you can combine it with other commands with `|`

    $ env | grep -i User

As you can see `grep` is pretty versatile. You can even use regular expressions in your pattern

    $ ls /somedir | grep '.txt$'

Should return all files ending with `.txt` in `somedir`.


### Text Editors

Vim and emacs are popular text editors that are installed by default on most Linux distributions and they both have their pros and cons. They are essentially coding, word document processing and basically all in one editors. <br>

#### Vim (Vi Improved)

Vim stands for vi (Improved) just like its name it stands for an improved version of the vi text editor command. It's super lightweight, opening and editing a file with vim is quick and easy. It's also almost always available, if you booted up a random Linux distribution, chances are vim is installed by default.

To fire up vim just type:

    vim

#### Vim Search Patterns

To search for an expression just type the `/` key and then your search result while you are in a vim session. Once you hit enter, you can press `n` to go forward or `N` to go backward in your search results.


    My pretty file is very pretty.


    /pretty



    will find the pretty words in the text file.


The ? search command will search the text file backwards, so in the previous example, the last pretty would come up first. 


    My pretty file is very pretty.


    ?pretty



    will find the pretty words in the text file.


#### Vim Navigation

Now you may notice, the mouse is nowhere of use here. To navigate a text document in vim, use the following keys:

- `h` or the left arrow - will move you left one character
- `k` or the up arrow - will move you up one line
- `j` or the down arrow - will move you down one line
- `l` or the right arrow - will move you right one character

#### Vim Appending Text

Now you may have noticed if you tried to type something you wouldn't be able to. That's because you are in command mode. This can get pretty confusing especially if you just want to open a file and enter text. The command mode is used for when you enter commands like `h`,`j`,`k`,`l` etc. To insert text you'll need to enter insert mode first. `

- `i` - insert text before the cursor
- `O` - insert text on the previous line
- `o` - insert text on the next line
- `a` - append text after the cursor
- `A` - append text at the end of the line

Notice how when you type any of these insertion modes, you'll see that vim has entered insert mode at the bottom of the shell. To exit insert mode and go back to command mode, just hit the `Esc` key.

#### Vim Editing

Now that we have a couple of lines written, let's edit it a bit more and remove some cruft.

- `x` - used to cut the selected text also used for deleting characters
- `dd` - used to delete the current line
- `y` - yank or copy whatever is selected
- `yy` - yank or copy the current line
- `p` - paste the copied text before the cursor

#### Vim Saving and Exiting

Now that you've done your editing it's time to actually save and quit out of vim:

- `:w` - writes or saves the file
- `:q` - quit out of vim
- `:wq` - write and then quit
- `:q!` - quit out of vim without saving the file
- `ZZ` - equivalent of :wq, but one character faster
- `u` - undo your last action
- `Ctrl-r` - redo your last action

You may not think `ZZ` is necessary, but you'll eventually see that your fingers may tend to lean towards this rather than `:wq`. <br>

## Permissions 

### File Permissions

Files have different permissions or file modes. Let's look at an example:

    $ ls -l Desktop/
    drwxr-xr-x 2 pete penguins 4096 Dec 1 11:45 .

There are four parts to a file's permissions. The first part is the filetype, which is denoted by the first character in the permissions, in our case since we are looking at a directory it shows `d` for the filetype. Most commonly you will see a `-` for a regular file.

The next three parts of the file mode are the actual permissions. The permissions are grouped into 3 bits each. The first 3 bits are user permissions, then group permissions and then other permissions. I've added the pipe to make it easier to differentiate.

    d | rwx | r-x | r-x 
Each character represent a different permission: 

- `r:` readable
- `w:` writable
- `x:` executable (basically an executable program)
- `-:` empty

So in the above example, we see that the user pete has read, write and execute permissions on the file. The group penguins has read and execute permissions. And finally, the other users (everyone else) has read and execute permissions.

### Modifying Permissions
Changing permissions can easily be done with the chmod command.
First, pick which permission set you want to change, user, group or other. You can add or remove permissions with a `+` or `-`, let's look at some examples.

Adding permission bit on a file 

    $ chmod u+x myfile
The above command reads like this: change permission on myfile by adding executable permission bit on the user set. So now the user has executable permission on this file!

Removing permission bit on a file 

    $ chmod u-x myfile

Adding multiple permission bits on a file 

    $ chmod ug+w

There is another way to change permissions using numerical format. This method allows you to change permissions all at once. Instead of using `r`, `w`, or `x` to represent permissions, you'll use a numerical representation for a single permission set. So no need to specify the group with `g` or the user with `u`.

The numerical representations are seen below:

- `4`: read permission
- `2`: write permission
- `1`: execute permission

Let's look at an example:

    $ chmod 755 myfile

Can you guess what permissions we are giving this file? Let's break this down, so now `755` covers the permissions for all sets. The first number `7` represents user permissions, the second number `5` represents group permissions and the last `5` represents other permissions.

But, `7` and `5` weren't listed above, where are we getting these numbers? Remember we are combining all the permissions into one number now, so you'll have to get some math involved.

`7 = 4 + 2 + 1`, so `7` is the user permissions and it has `read`, `write` and `execute` permissions

`5 = 4 + 1`, the group has `read` and `execute` permissions

`5 = 4 +1`, and all other users have `read` and `execute` permissions

One thing to note: it's not a great idea to be changing permissions, you could potentially expose a sensitive file for everyone to modify, however many times you legitimately want to change permissions, just take precaution when using the `chmod` command.

## Packages 

### Software Distribution

Your system is comprised of many packages such as internet browsers, text editors, media players, etc. These packages are managed via package managers, which install and maintain the software on your system. Not all packages are installed through package managers though, you can commonly install packages directly from their source code, however the majority of the time you will use a package manager to install software. 

What are packages? You may know them as Chrome, Photoshop, etc and they are, but what they really are just lots and lots of files that have been compiled into one. The people (or sometimes a single person) that write this software are known as upstream providers, they compile their code and write up how to get it installed. These upstream providers work on getting out new software and update existing software. When they are ready to release it to the world, they send their package to package maintainers, who handle getting this piece of software in the hands of the users. These package maintainers review, manage and distribute this software in the form of packages.

### Package Repositories

Repositories are just a central storage location for packages. There are tons of repositories that hold lots of packages and best of all they are all found on the internet. Your machine doesn't know where to look for these repositories unless you explicitly tell it where to look.

For example, let's say I want WackyWidgets Software on my machine. Well WackyWidgets manages their own repositories for their widget packages, inside this repository are 10 packages, the CoolWidget package, the SuperWidget package, etc. WackyWidgets hosts this repository at a source link called: http://download.widgets/linux/deb/

Now instead of going to their website to download the package directly, you can tell your machine to find WackyWidgets software from the source link.

Your distribution already comes with pre-approved sources to get packages from and this is how it installs all the base packages you see on your system. On a Debian system, this sources file is the `/etc/apt/sources.list` file. Your machine will know to look there and check for any source repositories you added.

### apt (Package Management)

One of the most popular management systems is apt. 

    Debian: $ apt install package_name

Remove a package

    Debian: $ apt remove package_name


Updating packages for a repository

It's always best practice to update your package repositories so they are up to date before you install and update a package.

    Debian: apt update; apt upgrade


Get information about an installed package

    Debian: apt show package_name


## Compile Source Code

Often times you will encounter an obscure package that only comes in the form of pure source code. You'll need to use a few commands to get that source code package compiled and installed on your system.

First thing is first, you'll need to have software to install the tools that will allow you to compile source code.

    $ sudo apt install build-essential
Once you do that, extract the contents of the package file, most likely a .tar.gz file.

    $ tar -xzvf package.tar.gz

Before you do anything, take a look at the `README` or `INSTALL` file inside the package. Sometimes there will be specific installation instructions.

Depending on what compile method that the developer used, you'll have to use different commands, such as `cmake` or something else.

However, most commonly you'll see basic make compilation, so we'll discuss that:

Inside the package contents will be a configure script, this script checks for dependencies on your system and if you are missing anything, you'll see an error and you'll need to fix those dependencies.

    $ ./configure
The `./` allows you to execute a script in the current directory.

    $ make
Inside of the package contents, there is a file called `Makefile` that contains rules to building the software. When you run the make command, it looks at this file to build the software.

    $ sudo make install
This command actually installs the package, it will copy the correct files to the correct locations on your computer.

If you want to uninstall the package, use:

    $ sudo make uninstall

Be wary when using `make install`, you may not realize how much is actually going on in the background. If you decide to remove this package, you may not actually remove everything because you didn't realize what was added to your system. Instead forget everything about `make install` that I just explained to you and use the `checkinstall command`. This command will make a `.deb` file for you that you can easily install and uninstall.

    $ sudo checkinstall
This command will essentially `make install` and build a `.deb` package and install it. This makes it easier to remove the package later on.

