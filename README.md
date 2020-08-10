# Newcastle University Off-Campus Access

This is an UNOFFICIAL repository/guide, aimed at enabling staff/students at Newcastle University to work off-campus more effectively. Please distribute to anyone who could need it. Please report any issues via Github or by emailing richard.yim@newcastle.ac.uk. Any other comments are also welcome through these channnels.

## Accessing journal articles through Newcastle University's systems, without using RAS/Remote Desktop
Many journals and publishers restrict full text access to institutions/individuals who have subscriptions.
NU has subscriptions to most major journals, and as a member of staff/a student at NU, you have access as well.
Accessing these papers by launching a browser in RAS or Remote Desktop is possible, but both of those options can be sluggish and annoying to use.
There is a way of routing the traffic to/from a browser on your computer through a server at NU, so that it appears to the publishers that you are on campus.

### Windows users - Tunnelling your browser traffic through Newcastle University
The idea is to set up a SOCKS tunnel between your device and a server at NU, through which your web traffic can travel.
You then set your browser up to run through this tunnel.
According to the publishers' websites, your device is on the NU campus (your external IP address is an NU address), so they will serve you content without showing you the paywall.

1. Download and install PuTTY from `putty.org`.
1. Launch PuTTY from the Start Menu.
1. Go to `https://linuxize.com/post/how-to-setup-ssh-socks-tunnel-for-private-browsing/`. a fifth of the way down the page, there is a section entitled "Windwos". This will walk you through setting up your connection to any server. 
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
1. Get access to an SSH client or a unix terminal. Examples are available below.
   * There is an SSH client in RAS `https://services.ncl.ac.uk/itservice/core-services/software/ras/`
   * Linux Subsystem for Windows has a terminal which is capable of supporting SSH `https://www.illuminiastudios.com/dev-diaries/ssh-on-windows-subsystem-for-linux/`
   * Almost all Linux distributions come with SSH built in.
1. Launch the client/terminal and enter the following, replacing `username` with your Newcastle University username.
   ```
   ssh username@unix.ncl.ac.uk
   ```
   This brings you into AIDAN, Newcastle University's SSH server.
3. From Aidan, SSH into Rocket.
   ```
   ssh username@rocket.hpc.ncl.ac.uk
   ```

## Transferring files between your home Linux machine and Rocket
Rocket does not accept SSH connections from elsewhere on the internet.
In order to get around this, it is necessary to tunnel your SSH connection through AIDAN, since the AIDAN server is able to connect to Rocket and accepts connections from machines not on Newcastle University's network.
The `rocketscp` script allows you to create an SSH tunnel, and to move files to/from Rocket through the tunnel.

By default, the tunnel is active for 10 minutes.
If you need to transfer larger amounts of data through to/from Rocket than 10 min will allow, edit the `rocketscp` script, and replace `ControlPersist=600`, on line 94, with a larger number in seconds.

The following instructions will get you started with transferring files. It is useful to send a test file over, and to check that it arrived okay.

1. Download the `rocketscp` script.
1. Open `rocketscp` in a text editor, and replace `USERNAME`, on line 10, with your Newcastle University username.
1. Run `./rocketscp -h` to get usage information
1. Run `./rocketscp` to transfer files to/from Rocket.
1. Optional: You can add the directory to which you downloaded the `rocketscp` script to your $PATH in order to access them without having to specify a path to the location of these scripts.
If you are running the Bash shell, edit `~/.bashrc` and add the following line, replacing `/path/to/scripts` with the location of the scripts.
For other shells, add this line to the appropriate script which runs when you start a new shell.
   ```
   PATH="/path/to/scripts:${PATH}"; export PATH;
   ```
   Now, you can run `rocketscp` from anywhere on your system without having to type the whole path out.

Note: It's a pain to type your password in twice whenever you want to transfer files, but this is the way the technology works. To avoid this, you can set up RSA key pairs (tutorial available at: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2). Newcastle University has recently changed its policy on RSA keys, so make sure that your key setup complies with their up-to-date guidelines.

## Licensing
Please see the "LICENSE" file distributed alongside this file for licensing information.
