# install crystal lang on WSL ubuntu

Installing crystal on WSL(Windows Subsystem for Linux) is not a simple job "[because of a difference in how Windows and Linux handles connection attempts to localhost sockets.](https://github.com/Microsoft/WSL/issues/3286#issuecomment-403647475)" It fails while setting up repository as belows.
```bash
$ curl -sSL https://dist.crystal-lang.org/apt/setup.sh | sudo bash
Executing: /tmp/apt-key-gpghome.6HPc7xFJM3/gpg.1.sh --keyserver keys.gnupg.net --recv-keys 09617FD37CC06B54
gpg: connecting dirmngr at '/tmp/apt-key-gpghome.6HPc7xFJM3/S.dirmngr' failed: IPC connect call failed
gpg: keyserver receive failed: No dirmngr
...
Err:5 https://dist.crystal-lang.org/apt crystal InRelease
  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 09617FD37CC06B54
Reading package lists... Done
W: GPG error: https://dist.crystal-lang.org/apt crystal InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 09617FD37CC06B54
E: The repository 'https://dist.crystal-lang.org/apt crystal InRelease' is not signed.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

The setup.sh is as belows.
```bash
#!/usr/bin/env bash
apt-key adv --keyserver keys.gnupg.net --recv-keys 09617FD37CC06B54
echo "deb https://dist.crystal-lang.org/apt crystal main" > /etc/apt/sources.list.d/crystal.list
apt-get update
```
And the 1st line breaks. So you should manually deal with it.
https://stackoverflow.com/questions/50990195/crystal-installation-on-wsl-fails
```
$ curl -s "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x09617FD37CC06B54" | sudo apt-key add -
OK
$ echo "deb https://dist.crystal-lang.org/apt crystal main" > /etc/apt/sources.list.d/crystal.list
$ sudo apt-get update
..
Get:4 https://dist.crystal-lang.org/apt crystal InRelease [2496 B]
..
Get:9 https://dist.crystal-lang.org/apt crystal/main amd64 Packages [446 B]
Fetched 364 kB in 5s (66.7 kB/s)
Reading package lists... Done
$ sudo apt install crystal
```
DONE!
