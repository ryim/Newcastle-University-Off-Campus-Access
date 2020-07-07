# Temporary Newcastle University Unix Resource Access

This repository is aimed at users of the Unix resources at Newcastle University. Please distribute to anyone who could need it. Please report any issues via Github or by emailing richard.yim@newcastle.ac.uk. Any other comments are also welcome through these channnels.

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

Note: It's a pain to type your password in twice whenever you want to transfer files, but this is the way the technology works. To avoid this, you can set up RSA keys to do this (tutorial available at: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2). Newcastle University has recently changed its policy on RSA keys, so make sure that your key setup complies with their up-to-date guidelines.

## Licensing
Please see the "LICENSE" file distributed alongside this file for licensing information.
