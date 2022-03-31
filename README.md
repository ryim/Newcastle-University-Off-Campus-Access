# Newcastle University Off-Campus Access

This is an UNOFFICIAL repository/guide, aimed at enabling staff/students at Newcastle University to work off-campus more effectively. Please distribute to anyone who could need it. Please report any issues via Github or by emailing richard.yim@newcastle.ac.uk. Any other comments are also welcome through these channnels.

## Accessing journal articles through Newcastle University's systems, without using /Remote Desktop
Many journals and publishers restrict full text access to institutions/individuals who have subscriptions.
NU has subscriptions to most major journals, and as a member of staff/a student at NU, you have access as well.
Accessing these papers by launching a browser in Remote Desktop is possible, but downloading a paper for offline use can be sluggish and annoying to use.
There is a way of routing the traffic to/from a browser on your computer through a server at NU, so that it appears to the publishers that you are on campus.

### Windows users - Tunnelling your browser traffic through Newcastle University
The idea of this process is to set up a SOCKS tunnel between your device and a server at NU, through which your web traffic can travel.
You set your browser up to send all of its traffic through this tunnel to an NU server, and the server then forwards the traffic out to the internet.
According to the publishers' websites, your device is on the NU campus (your external IP address is an NU address), so they will serve you content without showing you the paywall.

1. Download and install PuTTY from https://www.putty.org.
1. Launch PuTTY from the Start Menu.
1. Go to https://linuxize.com/post/how-to-setup-ssh-socks-tunnel-for-private-browsing/. a fifth of the way down the page, there is a section entitled "Windows". This will walk you through setting up your tunnel to any server. 
    1. In step 1 of the guide, use `unix.ncl.ac.uk` in the Host "Name/IP address" field.
    1. Follow the rest of the guide to set your tunnel up, and configure your browser to use the tunnel.
    1. When you are finished, don't forget to reset any browser settings, so that you can use your browser normally in future.
    1. Finally, close PuTTY.
<!---
On Windows, Part 2 of the following guide may help: `https://www.ocf.berkeley.edu/~xuanluo/sshproxywin.html`.
-->

### Accessing articles from Linux machines using NCL-chromium
For Linux users, the `NCL-chromium` Bash script, included in this repository, will automate the process of setting up a tunnel to AIDAN, one of NU's servers, and launch an instance of Chrome/Chromium with the appropriate options. This script has the advantage of tunnelling only a single browser's traffic through your university connection, rather than all of the traffic from your device. This is an early version of this script, so feedback is greatly appreciated.

#### Prerequisites
`NCL-chromium` requires that at least one of: Chrome, Chromium, or Chromium Browser Privacy is installed, and is launchable from the terminal using one of the following commands: `"chromium-browser-privacy", "chromium-browser", "chromium", "chrome"`.

#### Usage
1. Download a copy of NCL-chromium
1. Open it in a text editor. Replace `USERNAME` on line 9 with your NU username, then save it.
1. Make sure that Chrome/Chromium isn't open
1. Navigate to the location of NCL-chromium in your terminal, and launch the script with `./NCL-chromium`
1. Enter your NU password during SOCKS tunnel setup. The browser should launch automatically.

Once this is done, the script will create a process in the background. This process checks to see if the browser is still running, once a minute. If the browser is closed, then the process will kill the SOCKS tunnel without you having to do anything.

## Accessing Rocket via an SSH connection from outside the university
NUIT has provided the following helpful guide: https://services.ncl.ac.uk/itservice/research/hpc/remoteaccess/
<!--Rocket does not accept SSH connections from elsewhere on the internet, but AIDAN, one of NU's web-facing servers, does.-->
<!--One can connect to AIDAN via SSH, then SSH from AIDAN into Rocket.-->
<!--This can be done for other servers behind the NU firewall, as well.-->

<!--1. Get access to an SSH client or a unix terminal. Examples are available below.-->
<!--   * Preferred for Windows users: Download and install PuTTY from https://www.putty.org-->
<!--   * There is an SSH client in RAS https://services.ncl.ac.uk/itservice/core-services/software/ras/-->
<!--   * Linux Subsystem for Windows has a terminal which is capable of supporting SSH https://www.illuminiastudios.com/dev-diaries/ssh-on-windows-subsystem-for-linux/-->
<!--   * Almost all Linux distributions come with SSH built in.-->
<!--1. Launch the client/terminal and enter the following, replacing `username` with your Newcastle University username.-->
<!--   ```-->
<!--   ssh username@unix.ncl.ac.uk-->
<!--   ```-->
<!--   This brings you into AIDAN, Newcastle University's SSH server.-->
<!--3. From Aidan, SSH into Rocket.-->
<!--   ```-->
<!--   ssh username@rocket.hpc.ncl.ac.uk-->
<!--   ```-->

### Helpful note for UNIX/Linux users
SSH connections from UNIX/Linux systems, using default options, tend to die if left inactive for some time.
Using the command `ssh -o ServerAliveInterval=300` to connect to `username@server` tells the SSH process to ping the server every 5 minutes, to tell the server that the connection should be kept alive.
Adding an alias to your shell environment setup script (e.g.: `~/.bashrc` or `~/.cshrc`) means that you won't have to type it out every time.
For example: add `alias ssh="ssh -o ServerAliveInterval=300"` to the `~/.profile` file on AIDAN, and to `~/.bashrc` on your machine.

## Transferring files between your home Linux machine and a server on campus
(Note: The `nclscp` script mentioned in this section replaces the `rocketscp` script, which is now in the `Archive` folder.)

Rocket, and other servers, do not accept direct SSH connections from elsewhere on the internet.
In order to get around this, it is necessary to tunnel your SSH connection through AIDAN, since the AIDAN server is able to connect to Rocket and accepts connections from machines not on Newcastle University's network.
The `nclscp` script allows you to create an SSH tunnel, and to move files to/from a server through the tunnel.

By default, the tunnel is active for 1 hour.

The following instructions will get you started with transferring files. It is useful to send a test file over, and to check that it arrived okay.

1. Download the `nclscp` script and the `nclscp.config` file into the same directory.
1. Open `nclscp.config` in a text editor, and replace `USERNAME`, on line 2, with your Newcastle University username. This is necessary for logging into AIDAN.
    1. Whilst you're there, you can add tab-delimited lines to define how to access various servers on the NU network, through AIDAN. AIDAN itself doesn't need a line in this config file.
        - The `Alias` column is a shorthand name for the server that you can choose
        - The `hostname` column defines the actual location of the server (for example: `rocket.hpc.ncl.ac.uk`)
        - The `username` column is used to define usernames other than your campus username, if required. If not needed, two tabs between `hostname` and `port` to denote an empty column are fine.
        - The `port` column is the port on your machine which you want to forward. If you choose one, please ensure that this port is not used by any other service. There is provision for `nclscp` to choose its own ports, if you leave this column blank.
1. Run `./nclscp -h` to get usage information
1. Run `./nclscp` to transfer files to/from Rocket.
1. Optional: You can add the directory to which you downloaded the `nclscp` script to your $PATH in order to access them without having to specify a path to the location of these scripts.
If you are running the Bash shell, edit `~/.bashrc` and add the following line, replacing `/path/to/scripts` with the location of the scripts.
For other shells, add this line to the appropriate script which runs when you start a new shell.
   ```
   PATH="/path/to/scripts:${PATH}"; export PATH;
   ```
   Now, you can run `nclscp` from anywhere on your system without having to type the whole path out.

Note: It's a pain to type your password in twice whenever you want to transfer files, but this is the way the technology works. To avoid this, you can set up RSA key pairs (tutorial available at: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2). Newcastle University has recently changed its policy on RSA keys, so make sure that your key setup complies with their up-to-date guidelines.

The `nclscp` script automates the use of the external `scp` and `ssh` commands, and does not access or store any passwords itself.

## Licensing
Please see the "LICENSE" file distributed alongside this file for licensing information.
