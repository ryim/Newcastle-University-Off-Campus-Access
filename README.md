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
In order to get around this, it is necessary to tunnel your SSH connection through AIDAN, since the AIDAN server is able to connect to Rocket.
The `NCL-rocket-ssh` script sets this tunnel up for 10 minutes, so that you can use the `rocketscp` script to move files to/from Rocket through the tunnel.

By default, the tunnel is active for 10 minutes.
If you need to transfer larger amounts of data through to Rocket, edit the `NCL-rocket-ssh` script, and replace `ControlPersist=600` with a larger number in seconds.

The following instructions will get you started with transferring files. It is useful to send a test file over, and to check that it arrived okay.

1. Download the `rocketscp` and `NCL-rocket-ssh` scripts.
1. Open `NCL-rocket-ssh` in a text editor, and replace `USERNAME` with your Newcastle University username.
1. Run `./NCL-rocket-ssh` to start your 10 minute file transfer window.
1. Run `./rocketscp -h` to get usage information
1. Run `./rocketscp` to transfer files to/from Rocket.
