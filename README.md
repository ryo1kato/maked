maked - a simple build automation tool
=======================================

2011-09-22
Ryoichi Kato <ryo1kato@gmail.com>



What's "maked" ?
---------------------------------------

`maked` is a command to automate software build.
Actually, it's a simple Bash script to call "make"
(or "ant" or what ever you like) when a file under
certain directory is modified.



Why and when should I use "maked" ?
---------------------------------------

If you're using an editor like Emacs or Vi to write a program,
and run `make` (or `ant` or whatever) manually after every time you
save changes, then this script will do it for you.
(If you're using IDE like Eclipse, often comes with similar feature.)
Simple example to build Linux kernel's bzImage would using GNU make
would be like this:

    $ maked --make gmake -- -j3 bzImage

Or, if you're doing try-and-error on some configuration file for
say, Apache, you can used this command to automatically restart
apache every time you save changes on the configuration file.

    $ maked --directory /etc/httpd --make=apachectl restart

Or something more trivial like this:

    $ maked --cmd markdown README.md -f out.html


How it works
---------------------------------------

`maked` uses Linux's inotify syscalls to monitor any change under
certain directory (default is currenty directory)

When change is detected, it just call `make` command
(or any command you specify by `-m` option)

If you're familiar with shell scripts, below code snippet
-- though extremely simplified -- will give you an idea on
what `maked` can do for you.

    #!/bin/bash
    while true
    do
        inotifywait -r .
        make
    done

(Actuall script is more complex to support various options
such as `--interval`, `--cmd` and `--ignore`)


Installation / System Requirements
---------------------------------------
The script requires Bash and inotify-tools installation
on your Linux system. Most likely, bash is your default shell.

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
   increase maximum number of files a user is allowed to watch.



Usage
---------------------------------------

Just try `--help` option on the command.

    $ maked --help

