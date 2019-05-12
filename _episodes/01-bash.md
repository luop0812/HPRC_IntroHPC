---
title: "Connect to the HPC"
teaching: 10
exercises: 5
questions:
- "How do I connect to an HPC system?"
objectives:
- "Be able to connect to a remote HPC system"

keypoints:
- "We connect to remote servers, like HPC's, using the Terminal"
- "SSH is a secure protocol for connecting to remote servers"
- "To connect to a server, you need its address, an open port, and your user ID"
- "With OnDemand, you can upload and download files, create, edit, submit, and monitor jobs, run GUI applications, and connect via SSH, all via a web broswer."
---

## Opening a Terminal

Connecting to an HPC system is most often done with a program known as "SSH" (**S**ecure **SH**ell) 
which is accessed through a Terminal. Linux and Mac users will find a Terminal program already installed on their computers.  
Windows PC users will need to install a Terminal emulator, I suggest using [PuTTY](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe) 
or [MobaXterm](https://mobaxterm.mobatek.net/download.html). 

## Connecting with SSH

The SSH program needs at least one parameter to connect to a remote server:

* **Server Address** The web address of the server to connect to

Additional parameters may also be necessary:

* **Port Number** The specific port used to connect to the server
* **User ID** Your user ID on the HPC system  

<span style="color:white">blankline</span>
   


## Basic SSH Connection

Open your terminal, and input the following command.

~~~
ssh username@terra.tamu.edu
~~~
{: .bash}

If you've never connected to this particular server before you'll encounter a message similar to this:

~~~
The authenticity of host 'terra.tamu.edu' can't be established.
RSA key fingerprint is 2a:b6:f6:8d:9d:c2:f8:2b:8c:c5:03:06:a0:f8:59:12.
Are you sure you want to continue connecting (yes/no)?
~~~
{: .bash}

This is your computer warning you that you're about to connect to another computer, type \"yes\" to proceed.  This will add the HPC to your \"known hosts\", and you shouldn't see the message again the future.

~~~
Warning: Permanently added 'terra.tamu.edu' (RSA) to the list of known hosts.
~~~~
{: .bash}

You should now be prompted to input your password.  Type it in carefully (no characters will appear on the screen), then press ENTER.

~~~
username@terra.tamu.edu's password: 
~~~
{: .bash}

If you entered your password appropriately, congratulations, you're now connected to the HPC!  

<span style="color:white">blankline</span>

## Connecting with our OnDemand web portal

It is important to know how to login with SSH since it is the traditional method and you may have to use it. 
The OnDemand prtal provides the same functionality for remote SSH login. 

OnDemand is our "one stop shop" for access to our High Performance Computing resources. With OnDemand, you can upload and 
download files, create, edit, submit, and monitor jobs, run GUI applications, and connect via SSH, all via a web broswer, 
with no client software to install and configure.

### Logging on to OnDemand

Open a new browser window and go to [https://portal.hprc.tamu.edu](https://portal.hprc.tamu.edu). This is the homepage for the Ada and the Terra OnDemand portal. 

The rest of the exercises we'll work on today will be with Ada or Terra OnDemand.

>You can check our [user guide](https://hprc.tamu.edu/wiki/SW:Portal) for more information.
{: .callout}

Ada/Terra OnDemand uses TAMU CAS for authtication. Click [Terra OnDemand Portal](https://portal-terra.hprc.tamu.edu), and type in your NetID and password. 
Once you are logged in, you'll see the dashboard. At the top are the pulldown menus we'll use to access the clusters.

![OnDemand Dashboard](../files/Pulldown.png)      

As you are logging in to Terra, you can log in to Ada without further authentication due to the single-sign-on feature of CAS. 

We'll open a shell tab under 'Clusters', select 'Terra shell access'.

> ## The Command Prompt
> The command prompt is the symbol or series of characters which precedes each shell command, and let the user know the shell is ready to receive commands.  For example, when you initially login to the HPC cluster your command prompt should resemble `[username@terra2 ~]$`. If your command prompt changes to `>`, the shell is expecting further input. Use the key-binding CTRL+C to escape shell commands, returning your prompt from `>` to `[username@terra2 ~]$`.  
>
> The command prompt is controlled by the shell variable PS1. Let's check the current PS1 setup with the command `echo $PS1`. If we change its value, our prompt will change too. Let's try with `'PS1='\s-\v\\$ '`. Let's change it back to the default on HRPC clusters `PS1='[\u@terra2 \W]\$ '`.
>
> Make sure you can copy and paste this command into the shell tab, that will make the rest of this workshop much easier.
>
> More information about bash can be found at [Bash Reference Manual](http://www.gnu.org/software/bash/manual/bashref.html).
{: .callout}









