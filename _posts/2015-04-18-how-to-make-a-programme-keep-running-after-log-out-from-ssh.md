---
layout: post
title: "How to make a programme keep running after log out from ssh"
description: ""
category: 
tags: []
---
{% include JB/setup %}

When using cluster to do computing, it is often useful to know how to keep your programme running after log out 
from ssh. This can save you from the trouble of writing "Don't Turn off the Computer" signs.

# Understanding Unix Job Control

Starting a job at a Unix Command prompt will cause that job to be attached to the shell that started the job.
If that shell is terminated, the job(s) that were started under it will be terminated as well.  
This is caused by the fact that when a UNIX process ends, all the processes contained within the process tree attached to the ended process
are also terminated.  

This whole reaction chain starts from the father process (in this case the terminal created by ssh session) receiving a SIGHUP from the user when you
log off. The ssh session then terminates and sends a SIGHUP to all the processes in its job list (shells), and thus SIGHUP propagtes recursively (to the processes 
within each shell) until every process in the process tree attached to the terminal is ended.

# Saving Your Programme from Getting Killed

The basic idea to prevent your programme from termination is to supress SIGHUP somehow. And there are two ways to do that.

## Using disown

The command disown removes the process from the job list of its father shell, but it still leaves it connected to the terminal. 
The results is that the shell will not send it a SIGHUP. 

However, note that the process is still connected to the terminal for input and output, so if the terminal is destroyed (which can happen if it was a pty, like those created by xterm or ssh, and the controlling program is terminated, by closing the xterm or terminating the SSH connection), the program will fail as soon as it tries to read from standard input or write to standard output.

Obviously, disown can only be applied to background jobs, because you cannot enter it when a foreground job is running. And one more thing, disown is a BASH command, and may not be available in csh etc.

## Using nohup

The command nohup, on the other hand, acts on the receiver side and prevents the process from receiving a SIGHUP (thus the name), even if the shell still tries to send SIGHUP to it.

Unlike disown, nohup effectively separate the process from the terminal by closing standard input (the program will not be able to read any input, even if it is run in the foreground)
and redirecting standard output and standard error to the file nohup.out, so the program will not fail for writing to standard output if the terminal fails, and whatever the process writes is not lost.

Note that nohup does not remove the process from the shell job control and also does not put it in the background. For example, unlike with disown, the shell will still tell you when the nohup job 
has completed (unless the shell is terminated before, of course).

# Examples

The following example is tested on the clusters.

## nohup

{% highlight bash %}
[user@cluster ~]$ nohup sleep 3600 &
[1] 17576
[user@cluster ~]$ logout
rlogin: connection closed.
{% endhighlight %}

## disown

{% highlight bash %}
[user@cluster ~]$ bash
bash-4.1$ sleep 3600
^Z
[1]+  Stopped                 sleep 3600
bash-4.1$ bg
[1]+ sleep 3600 &
bash-4.1$ disown
{% endhighlight %}

By the way, if you do not nohup a job when it begins, you can use disown to modify a running job; 
with no arguments disown modifies the current job, which is the one that was just backgrounded.
