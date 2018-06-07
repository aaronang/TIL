# Setting up Git and GitLab "properly"

What do I mean with "properly"? Every time we use `git clone`, `git fetch`, `git
pull`, or `git push`, Git prompts for a username and password. This is annoying.
The cause for these repeated prompts is when a project is cloned using the HTTPS
URL instead of the SSH URL. In this mini-tutorial, we will go through the
necessary steps to set up Git and GitLab "properly" such that we no longer
receive any annoying prompts.


1. In terminal:
   ```bash
   # In this mini-tutorial, we assume that we are working on a project called
   # "hello-ssh". The current working directory.
   $ pwd
   /path/to/hello-ssh   

   # Note that the remote URL is HTTPS, this causes the annoying prompts. To
   # use the SSH URL, we first need to generate a new SSH key pair.
   $ git remote -v
   origin  https://github.com/aaronang/hello-ssh.git (fetch)
   origin  https://github.com/aaronang/hello-ssh.git (push)   

   # Let's first check if an SSH key pair exists. If the following command
   # outputs a string that starts with `ssh-rsa` you can skip the steps to
   # generate an SSH key pair. 
   $ cat ~/.ssh/id_rsa.pub   

   # If an SSH key pair does not exists, we can generate the key pair with the
   # following command. Once you execute the command, you will be prompted to
   # input a file path to save the SSH key pair to. It is recommended to use
   # the suggested path. Afterward, you will be prompted for a password to
   # secure the SSH key pair.
   #
   # > It is a best practice to use a password for an SSH key pair, but it is
   # > not required and you can skip creating a password by pressing enter.
   # > Source: GitLab
   $ ssh-keygen -t rsa -C "your.email@example.com" -b 4096   

   # That's it! We have generated an SSH key pair. We need the public key for
   # GitLab. We can print the public key using `cat`.
   $ cat ~/.ssh/id_rsa.pub
   ssh-rsa SOME+SUPER+LONG+RANDOM+STRING== your.email@example.com
   ```

2. Once we have generated the SSH key pair, go to GitLab. In the top-right
   corner, click on the user badge and navigate to "Settings". In the sidebar on
   the left, navigate to "SSH Keys". 

3. Paste in your public SSH key that we obtained in the first step using `cat`
   program and give your key a title that helps you remember what the key
   represents. I usually name it after my machine name.

4. In terminal:
   ```bash
   # The current working directory.
   $ pwd
   /path/to/hello-ssh   

   # Note that the remote URL of the Git project is still using a HTTPS.
   $ git remote -v
   origin  https://github.com/aaronang/hello-ssh.git (fetch)
   origin  https://github.com/aaronang/hello-ssh.git (push)   

   # We can change the remote URL using the command:
   # 
   # git remote set-url origin <SSH_URL>
   #
   # Note: the SSH URL can be found on the project page on GitLab, see
   # screenshot below.
   $ git remote set-url origin git@github.com:aaronang/hello-ssh.git

   $ git remote -v
   origin  git@github.com:aaronang/hello-ssh.git (fetch)
   origin  git@github.com:aaronang/hello-ssh.git (push)  
   ```
   ![](/git/img/ssh.png)

After you have performed all the steps, you will no longer be prompted for username and password when using `git clone`, `git fetch`, `git pull`, or `git push` :tada:

## Resources

1. [GitLab and SSH keys](https://docs.gitlab.com/ee/ssh/)
2. [Which remote URL should I use?](https://help.github.com/articles/which-remote-url-should-i-use/)