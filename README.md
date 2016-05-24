fswatch-rsync
=============

About
-----

This is a script to automatically synchronize a local project folder to a folder on a remote server. It watches the local folder for changes and recreates the local state on the target machine as soon as a change is detected.

**Notes:**

* The script ignores hidden files, you can customize the sync behaviour by changing the rsync call in the script file.
* The script syncs every second, this can be changed by setting the LATENCY variable.

**Warning:**
This script enforces the local state of the folder to the remote location, meaning if additional files are created on the remote location they will be deleted, so don't store results or any other work in subfolders on the remote location. If you need to work inside the target folder you can of course create hidden folders or files since hidden files will be ignored as mentionend above.

Prerequisites
-------------

The script uses fswatch (<https://github.com/alandipert/fswatch>) and rsync (typically available on all UNIX based systems). To build and install fswatch you will need a C++ compiler. 

Setup
-----

1. Download and install fswatch: <https://github.com/alandipert/fswatch>.
2. Make fswatch available in your PATH variable. This can be achieved by appending PATH=/path/to/where/you/installed/fswatch:$PATH to ~/.bash_profile (OSX) or ~/.bash_rc (most Linux). ** OR ** you can add your path to fswatch to the script itself instead by changing the FSWATCH_PATH variable.
3. Set up ssh-key authentification: If you don't have an ssh key-pair, generate one using `ssh-keygen`. Then copy the key to the middle server using `ssh-copy-id -i ~/.ssh/id_rsa.pub ssh_user@middleserver`. Follow the same procedure for the authentification from middle to target server (create a keypair on the middle server and copy it to the target server). See the Note below if you're on a Mac and don't have `ssh-copy-id` installed.
4. Make the script executable: `chmod u+x ./fswatch-rsync.sh`

**Note:** On a Mac you can either install `ssh-copy-id` via MacPorts/Brew/etc, compile it from source or you can just append the content of your public key (default `~/.ssh/id_rsa.pub`) to the `~/.ssh/authorized_keys` file on the server. Make sure the access rights are set properly though (e.g. chmod 700 for `~/.ssh` and 600 for `~/.ssh/authorized_keys`, see <http://unix.stackexchange.com/questions/36540/why-am-i-still-getting-a-password-prompt-with-ssh-with-public-key-authentication>)

Usage
-----

`fswatch-rsync.sh /local/path /targetserver/path ssh_user [targetserver] [target_ssh_user]` 

The compulsory arguments are

* `/local/path`: your local project path
* `/targetserver/path`: the path on the target cluster server
* `ssh_user`: your username 

Optionally you can specify 

* `[targetserver]`: the target server (default is `home`)
* `target_ssh_user`: If your username for the middle server differs from the username at the target server  you can specify this argument. Then `ssh_user` is used as user for the middle server and `target_ssh_user` for the target server. 

**Note:** If the names of the local and remote folder are the same rsync will create a folder with the same name inside the remote folder. In that case just leave out the name in of the remote folder in the target path


Contact and Contribution
------------------------

Clemens Westrup

Please let me know if there's any problems with this script or the setup, I'm happy about any feedback. And of course also feel free to fork this and contribute here on GitHub. The package is free to use without any license (public domain).
