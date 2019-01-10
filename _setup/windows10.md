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
    - `$ ssh-keygen -t ed25519 -a 100 -C "my_github_email@gmail.com"`
        - `-t`: specifies type of key. [Why ed25519?](https://security.stackexchange.com/q/143442)
        - `-a`: rounds of key derivations (makes your key's password harder to brute-force)
        - `C`: comment
1. Give it a file name:
    - `github_ssh`
    - This will create 2 files in the Home folder of the Linux subsystem: `github_ssh`, and `github_ssh.pub` (a private and a public key)
1. Add the new private key to `ssh-agent`:
    - `$ eval $(ssh-agent -s)`
    - `$ ssh-add github_ssh`
1. Copy the public key to Windows 10:
    - `$ cp github_ssh.pub ~/Desktop`
1. [Add the public key to your Github account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
1. Test your connection (in Linux terminal):
    - `$ ssh -T git@github.com`
    - You should see this message:
    > Hi ericlingit! You've successfully authenticated, but GitHub does not provide shell access.
    - If you receive a "permission denied" message, see ["Error: Permission denied (publickey)"](https://help.github.com/articles/error-permission-denied-publickey).
1. set the remote repository url

    `$ git remote set-url origin git@github.com:ericlingit/fastai-notes.git`

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

1. Generate a GPG key pair
    - `$ gpg --full-generate-key`
    - Select "RSA and RSA" for your key type
    - 4096 for key size
    - The "Real name" should match your github account name. For me, it's `ericlingit`
    - The "Email address" should _not_ match your Github email! Github has enabled email privacy by default. This [prevents git pushes that can expose your email](https://stackoverflow.com/a/51097104/9762732) to the world. Instead, you should [use a private, noreply address from github](https://help.github.com/articles/setting-your-commit-email-address-on-github/). My address looks like this: 
    
        `ericlingit@users.noreply.github.com`
    
    - When finished, you should get an output that contains this portion:

    ```
    gpg: key DB3DA594230C0E46 marked as ultimately trusted
    gpg: revocation certificate stored as '/home/eric/.gnupg/openpgp-revocs.d/6475F2FF9432A8621B79F499DB3DA594230C0E46.rev'
    public and secret key created and signed.

    pub   rsa4096 2019-01-10 [SC]
        6475F2FF9432A8621B79F499DB3DA594230C0E46
    uid                      ericlingit <ericlingit@users.noreply.github.com>
    sub   rsa4096 2019-01-10 [E]
    ```
    - Explanation: `DB3DA594230C0E46` is your key ID, and `6475F2FF9432A8621B79F499DB3DA594230C0E46` is the fingerprint of your key (note that the key ID is the last 16 digits of the fingerpint). The revocation certificate should be published to a key server if you lose your secret key.
    - You can list your keys with:

        `$ gpg --list-secret-keys --keyid-format LONG`
1. Export the public key: `$ gpg -a --export 8E88FC612D3C6489 > ~/Desktop/github_gpg_public.asc`
1. Open `github_gpg_public.asc` with a text editor and copy its content, and then [paste in your Github account's settings page](https://help.github.com/articles/adding-a-new-gpg-key-to-your-github-account/).
1. Back in the linux terminal, `cd` to your working directory and enter the following commands:
    ```
    $ git config user.name ericlingit
    $ git config user.email ericlingit@users.noreply.github.com
    $ git config user.signingkey 8E88FC612D3C6489
    $ git config commit.gpgsign true
    ```
Your commits will now be automatically signed.

# Connect to Paperspace Jupyter server over SSH

1. Make sure you've [enabled public IP](https://support.paperspace.com/hc/en-us/articles/236362888-Public-IP-Addresses)
1. Follow the steps outlined [here](https://github.com/ericlingit/fastai-notes/blob/master/_setup/ubuntu.md#connect-to-paperspace-over-ssh)
1. On Paperspace website console:
    - Start your VM
1. On local machine:
    - Download & run putty.exe
    - In "Session":
        - Host name: `1xx.xxx.xxx.xxx` (this is your paperspace VM IP)
        - Port: 22
    - In "SSH" -> "Tunnels"
        - Source port: `8888`
        - Destination: `localhost:8888`
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
    - `$ jupyter notebook --no-browser --port 8888`
    - You'll be shown a localhost url to copy and paste into the browser, like this:
    > `http://localhost:8888/?token=a92b521a5e2fe37037d8265be76caed123232a7802b8825e`
    - Copy (hightlight with mouse cursor, then press ctrl+shift+c) and paste it into your browser, and hit Enter. You're now connected to the remote VM's Jupyter notebook.

Why go through all of this trouble when you can simply launch Jupyter notebook in Paperspace console and connect via the remote IP? Because it's not encrypted.
