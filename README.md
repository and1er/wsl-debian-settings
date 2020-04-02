# wsl-debian-settings

Settings and package installations that I use for WSL Debian on Windows 10 to help myself to reproduce the environment in clear WSL Debian distro.

The scripts and commands should be run in WSL command line. To run WSL CLI there are a several ways:

- run `wsl` in `cmd.exe` or `powershell.exe` terminals;
- run the distro's icon.

## Resetting Distro

To revert the distro to the factory defaults: [Reset and Unregister WSL Linux Distro in Windows 10](https://winaero.com/blog/reset-unregister-wsl-linux-distro-windows-10/)

```bat
> wsl --list --all
Windows Subsystem for Linux Distributions:
Debian (Default)

> wsl --unregister Debian
Unregistering...

> wsl --list --all
Windows Subsystem for Linux has no installed distributions.
Distributions can be installed by visiting the Microsoft Store:
https://aka.ms/wslstore

> debian
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: alosenkov
New password:
Retype new password:
passwd: password updated successfully
Installation successful!
alosenkov@LOSENKOV-A:~$
```

The clear WSL distro is ready to work.

## Initial Installation

### Upgrading the distro

```bash
sudo apt update && sudo apt full-upgrade
```

### Package installation

```bash
sudo apt install \
    man-db \
    vim \
    python3 \
    python3-pip \
    ssh-client \
    sshpass \
    curl \
    rsync \
    tree \
    wget
```

### Python pip packages installation

```bash
python3 -m pip install --upgrade --user pip ansible paramiko
```

Making the user locally installed packages available in system `$PATH` (may be should be placed to `~/.profile` or `~/.bash_profile` (without `export` keyword) to be consistent):

```bash
export PATH=$PATH:~/.local/bin/
```

### Generate SSH keys

```bash
mkdir -p ~/.ssh \
    && ssh-keygen -t rsa -q -N '' -f ~/.ssh/id_rsa
```

### Change prompt command color

The default color is green and it is the same as default color on Debian servers. To make the color different

```bash
vim ~/.bashrc 
```

аnd in block

```vim
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;33m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt
```

the light-green color `01;32` was changed to light-yellow `01;33`.
