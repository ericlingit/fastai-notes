# OS
Windows 10

# Git
I had previously set up [Windows subsystem for Linux (Ubuntu)](https://docs.microsoft.com/en-us/windows/wsl/install-win10). I'll use that instead of installing Git again on Windows.

To access your Windows files from inside the Linux subsystem, go to `/mnt/`, you'll see the drive letters for `c` or `d`.

For example, my github notes are located at `C:/Users/user/Desktop`. I can access from Linux subsystem via `/mnt/c/Users/user/Desktop`.

To make things easier, create a symbolic:

`ln -s /mnt/c/Users/user/Desktop ~/Desktop`

# Connect to Github over SSH

In the Linux terminal:

1. Generate a SSH key:
    - `$ ssh-keygen -t rsa -b 4096 -C "my_github_email@gmail.com"`
1. Give it a file name:
    - `github_ssh`
    - This will create 2 files in the Home folder of the Linux subsystem: `github_ssh`, and `github_ssh.pub` (a private and a public key)
1. Add the new key to `ssh-agent`:
    - `$ eval $(ssh-agent -s)`
    - `$ ssh-add github_ssh`
1. Copy the public key to Windows 10:
    - `$ cp github_ssh.pub Desktop`

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

You can now git clone over SSH and modify it without logging in:

`$ git clone git@github.com:ericlingit/fastai-notes.git`

