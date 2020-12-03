# lxsh
Start a linux shell here.

Let's say you're on a macos machine and know that there is a command in linux you'd like to use. Let's call it `cp -a` (which copies an entire directory structure). With lxsh, you can do this:

    [johnnyappleseed@mymac mydir]$ lxsh
    lxsh[23:06:05/Users/john/Projects/docker-tricks]$ cp -a here there

And done!

Here's what the lxsh alias looks like:

    echo "export PS1=\"lxsh[\[$(tput bold)\]\t\[$(tput sgr0)\]\w]\\$\[$(tput sgr0)\] \"" > $TMPDIR/a5ad217e-0f2b-471e-a9f0-a49c4ae73668

This creates the `a5ad217e-0f2b-471e-a9f0-a49c4ae73668` file in your host's `$TMPDIR`. The name has no signifigance other than it should be unique.  The contents of the file are the new `PS1` (prompt) variable for the shell you're about to start.

The docker command is as follows

    docker run --rm --name lxsh ... -it ubuntu

Runs the `ubuntu` image, naming it `lxsh`. Removing the container when done (`--rm`). 

There are two volumes:

    -v $PWD:$PWD 
    -v $TMPDIR/a5ad217e-0f2b-471e-a9f0-a49c4ae73668:/root/.bash_aliases 

The first `-v $PWD:$PWD` creates a volume named after the current directory and mounts the current directory to it. This probably won't work if some part of your path contains a space. The second is a dirty hack to change the prompt. We map the `$TMPDIR` file created earlier to the `/root/.bash_aliases` file in the container. This is not what the `.bash_aliases` was intended for, but has the effect of overriding the system generated `PS1`.

The last piece,

    -w $PWD

Makes sure the shell starts in the same directory that it was launched from.
