# OS
Ubuntu 16.04

# Connect to Paperspace over SSH
On local machine:
1. Generate a SSH key pair
    - `$ ssh-keygen -t ed25519 -f ~/.ssh/paperspace_ssh -C "my_email@gmail.com"`
      - `-t`: Specifies the type of key to create, Ed25519 in our case. Ed25519 is the current (2018) [recommended](https://security.stackexchange.com/a/144044) key algorithm.
      - `-f`: Specifies the filename of the generated key file. If you want it to be discovered automatically by the SSH agent, it must be stored in the default `.ssh` directory within your home directory.
      - `-C`: An option to specify a comment.

1. Add Your Key to SSH Agent
    - `$ eval "$(ssh-agent -s)"`
    - `$ ssh-add ~/.ssh/paperspace_ssh`

1. Add an entry to the ~/.ssh/config file. Where `1xx.xxx.xxx.xxx` is your Paperspace VM IP
    ```
    Host paperspace
      HostName 1xx.xxx.xxx.xxx
      User paperspace
      IdentityFile ~/.ssh/paperspace_ssh
      IdentitiesOnly yes
    ```
1. [Copy the local public key to a remote machine](https://unix.stackexchange.com/a/106482)
    - `$ scp ~/.ssh/paperspace_ssh.pub paperspace@1xx.xxx.xxx.xxx:~/.ssh/authorized_keys`

1. Edit the remote machine's authorized_keys file to add our newly created ssh key
    - Log into the remote machine and edit this file: ~/.ssh/authorized_keys
    - Paste the public key located at ~/.ssh/paperspace_ssh.pub
    - Save & close

1. Test your setup by logging in with:
    - $ ssh paperspace
    - You should _not_ be prompted to enter a ssh password

1. [Disable password access](https://askubuntu.com/a/1992/884977:) through SSH on the remote machine:
    - WARNING: make sure you're able to login with your ssh private key, else you won't be able to connect!
    - edit /etc/ssh/sshd_config
    - find this line: `#PasswordAuthentication yes`
    - change it to: `PasswordAuthentication no`
    - to ensure that Paperspace's web console doesn't break after the above change, append the following 2 lines to the end of the file to [allow password authentication for local network connections while enforcing ssh keys for WAN](https://serverfault.com/q/406839):
        ```
        Match Address 10.0.0.0/8,172.16.0.0/12,192.168.0.0/16
            PasswordAuthentication yes
        ```
    - save
    - restart ssh service: `$ sudo service ssh reload`
