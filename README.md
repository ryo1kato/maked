maked - a simple build automation tool
=======================================

2011-09-22
Ryoichi Kato <ryo1kato@gmail.com>

------------------------------------------------------------------------------

What's "maked" ?
---------------------------------------

"maked" is a command to automate building software.
Actually, it's a simple bash script to call "make"
(or "ant" or what ever you like) when a file under
certain directory is modified.



Why and when should I use "maked" ?
---------------------------------------

If you're using an editor like Emacs or Vi to write a program,
and run "make" (or "ant" or whatever) manually after every time you
save changes, then this script will do it for you.
(If you're using IDE like Eclipse, often comes with similar feature.)

    $ maked --make gmake -- -j3 vmlinux

Or, if you're doing try-and-error on some configuration file for
say, apache, you can used this command to automatically restart
apache every time you save changes on the configuration file.

    $ maked --make=apachectl restart --directory /etc/httpd



How it works
---------------------------------------

"maked" uses inotify syscalls to monitor any change under
certain directory (default is currenty directory)

When change is detected, it just call `make` command
(or any command specified by `-m` option)



Installation / System Requrement
---------------------------------------

1. Install inotify-tools
       $ sudo apt-get install inotify-tools    (On Debian / Ubuntsu etc.)
     or
       $ sudo yum install inotify-tools        (On Redhat/Fedora etc.)

    See https://github.com/rvoicilas/inotify-tools/ for more infomation.

2. Fetch this repository and just copy "maked"
   into `/usr/local/bin` (or wherever you want)

        $ git clone http://github.com/ryo1kato/maked.git
        $ sudo cp maked/maked /usr/local/bin

3. Make sure the she-bang line points to a Bash binary.
   (Default is `#!/bin/bash`; modify the first line if you have
    Bash installed on different path.)

4. Give it 'x'(executable) permission.

        $ sudo chmod +x /usr/local/bin

5. If your code base is large(more than 8K files), you might have to
   adjust `/proc/sys/fs/inotify/max_user_watches` kernel parameter to
   increase maximum number of files a user allowed to watch.



Usage
---------------------------------------

Just try `--help` option on the command.

    $ maked --help

