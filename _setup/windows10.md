# OS
Windows 10

# Git
I had previously set up [Windows subsystem for Linux (Ubuntu)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) with git installed. I'll use that instead of installing Git again on Windows.

To access your Windows files from inside the Linux subsystem, go to `/mnt/`, you'll see the drive letters for `c` or `d`.

For example, my github notes are located at `C:/Users/user/Desktop`. I can access from Linux subsystem via `/mnt/c/Users/user/Desktop`.

To make things easier, create a symbolic link to it:

`$ ln -s /mnt/c/Users/user/Desktop ~/Desktop`

# Connect to Github over SSH

In the Linux subsystem terminal:

1. Generate a SSH key-pair:
    - `$ cd ~/.ssh`
    - `$ ssh-keygen -t rsa -b 4096 -C "my_github_email@gmail.com"`
1. Give it a file name:
    - `github_ssh`
    - This will create 2 files in the Home folder of the Linux subsystem: `github_ssh`, and `github_ssh.pub` (a private and a public key)
1. Add the new private key to `ssh-agent`:
    - `$ eval $(ssh-agent -s)`
    - `$ ssh-add github_ssh`
1. Copy the public key to Windows 10:
    - `$ cp github_ssh.pub ~/Desktop`

1. Switch over to Windows 10 and locate the file `github_ssh.pub` in your Desktop folder.
1. Open it with a text editor and copy its content
1. Open a browser, go to github.com, and sign into your account.
1. [Add the SSH public key to your account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
1. Test your connection (in Linux terminal):
    - `$ ssh -T git@github.com`
    - You should see this message:
    >Hi ericlingit! You've successfully authenticated, but GitHub does not
provide shell access.
    - If you receive a "permission denied" message, see ["Error: Permission denied (publickey)"](https://help.github.com/articles/error-permission-denied-publickey).

1. git clone over SSH:

    `$ git clone git@github.com:ericlingit/fastai-notes.git`

1. set the remote repo url

    `git remote set-url origin git@github.com:ericlingit/fastai-notes.git`

You can now `git push` to your github account.

If you have [more than 1 SSH key](https://stackoverflow.com/q/7927750/9762732) in your `.ssh` folder, you need to edit your `~./ssh/config` file with the following entry. Otherwise git won't know which key it should use to log into github:
```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_ssh
  IdentitiesOnly yes
```

# [Sign your commits](https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/)

1. Generate a gpg key pair
1. Copy the public key and paste in your Github account's settings page.
1. List your keys with `$ gpg --list-secret-keys --keyid-format LONG`
1. Copy the key ID that looks like this: `6F3A9EE04AE75559`
1. `cd` to your working directory
1. `$ git config user.signingkey 6F3A9EE04AE75559`
1. commit with `-S` option: `$ git commit -S -m "commit msg here"`

To sign commits without having to specify the `-S` option, you can set this:

`$ git config commit.gpgsign true`

# Connect to Paperspace Jupyter server over SSH

1. Make sure you've [enabled public IP](https://support.paperspace.com/hc/en-us/articles/236362888-Public-IP-Addresses)
1. On Paperspace website console:
    - Start your VM
1. On local machine:
    - Download & run putty.exe
    - In "Session":
        - Host name: 184.105.175.191
        - Port: 22
    - In "SSH" -> "Tunnels"
        - Source port: `8889`
        - Destination: `localhost:8889`
        - Click Add
    - In "Connection":
        - Seconds between keepalives: `60` (this prevents the client from automatically disconnecting by sending a keepalive message every 60 seconds)
    - Go back to "Session":
        - Saved session: `paperspace`
        - Click Save
    - Click Open, and accept the key when prompted.
    - login as: `paperspace`
    - password: Copy the password sent to your inbox by Paperspace. To paste into putty, right click inside the terminal (you won't see any characters show up), then press Enter.
1. Once logged in:
    - `$ jupyter notebook --no-browser`
    - You'll be shown a localhost url to copy and paste into the browser, like this:
    > `http://localhost:8889/?token=a92b521a5e2fe37037d8265be76caed123232a7802b8825e`
    - Copy (hightlight with mouse cursor, then press ctrl+shift+c) and paste it into your browser, and hit Enter. You're now connected to the remote VM's Jupyter notebook.

Why go through all of this trouble when you can simply launch Jupyter notebook in Paperspace console and connect via the remote IP? Because it's not encrypted.

Ideally you'd want to login with a SSH key. I'll add instructions for doing that later (maybe).