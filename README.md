## Set up git and configuring with Github account:octocat:

This configuration can be done in windows, using github bash and similarly in linux.

### Installing Git
Download the installer for Windows from the Git [official site](https://git-scm.com/download/).

Git for Windows comes with Git Bash (it's own command prompt), and it looks better than Windows default prompt. 

### Set up git 

- First we need creates a new ssh key, using the provided email as a label. Open Git Bash after installation and type: 
``` console
$ ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"
```
It will return: 
``` console
Generating public/private rsa key pair.  
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```
Press Enter. This accepts the default file location.

Then he asks for a password. This password will have to be typed every time you download something from a repository or send something there. You can leave it blank. This is your choice. If you want to leave without, just enter. If not, enter the password and confirm.
``` console
Enter passphrase (empty for no passphrase): [Type a passphrase]  
Enter same passphrase again: [Type passphrase again]
```
It will return a confirmation: 
``` console 
Your identification has been saved in /Users/you/.ssh/id_rsa.  
# Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
# The key fingerprint is:
# 08:21:02:ca:85:d6:42:d2:00:f9:43:9d:f0:a2:db your_email@domain.com
The key's randomart image is:
+---[RSA 4096]----+
|    ..      .    |
+----[SHA275]-----+
```
- Start the ssh-agent in the background. Let's activate it:
``` console
$ exec ssh-agent bash [Press enter]
$ eval ssh-agent -s [Press enter]
```
It will return something like that: 
``` console
SSH_AUTH_SOCK=/tmp/ssh-9S2E7m94sl07/agent.18936; 
export SSH_AUTH_SOCK;
SSH_AGENT_PID=18172; 
export SSH_AGENT_PID;
echo Agent pid 18172;
```
- Add your SSH key to the ssh-agent:
``` console
$ ssh-add ~/.ssh/id_rsa [Press enter]
```
It will return: 
``` console 
Identity added: /c/Users/you/.ssh/id_rsa (your_email@domain.com)
```
- Copies your SSH key to your clipboard
``` console 
$ cat ~/.ssh/id_rsa.pub > /dev/clipboard 
```

2. Logged into your GitHub account, go to the top right options, click your profile picture:

`Settings > SSH and GPG Keys > New SSH Key`

Give a title to your key, and past the key int the blank large box.

### Testing if your configuration is correct
Still on Git Bash: 
``` console
$ ssh -T git@github.com
```
It will return: 
``` console 
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
Then everything is correct!

Now anytime you clone a repository, GitHub will automatically give you the `SSH URL` and you will have to provide your `SSH key` paraphrase anytime you push and pull changes and you will not need to type in your username or your password anymore.

### Add your Git username and set your email

Important: every Git commit will use this information (username and email) to identify you as the author.

Add your username:
```console
git config --global user.name "YOUR_USERNAME"
```

Set your email address: 
```console
git config --global user.email "your_email_address@example.com"
```

Obs.: You’ll need to do this only once, since you are using the `--global` option. Check global infos: `git config --global --list` 

### Basic Git Commands
```console
# View your remote repositories
git remote -v

# Create a branch
git checkout -b NAME-OF-BRANCH

# Work on an existing branch - To switch to an existing branch, so you can work on it:
git checkout NAME-OF-BRANCH

# Send changes to gitlab/github.com - To push all local commits to the remote repository:
git push REMOTE NAME-OF-BRANCH

```
### Checkout a branch into a local repository
On your local system, make sure you have a local repository cloned from the remote repository. Then, do the following:

List all your branches, at to the root of the local repository: ``` $ git branch -a ```

Something similar to the following:
```console
* master   <feature_branch>
  remotes/origin/<feature_branch>
  remotes/origin/master
```

Checkout the branch you want to use.
```console
$ git checkout <feature_branch>
```
Confirm you are now working on that branch:
```console
$ git branch
```
Something similar to the following:
```console
$ git branch 
* <feature_branch>
  master
```

### Checking for existing SSH keys 

Windows: Open Git Bash.
```console
# To see if existing SSH keys are present:
$ ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist. Check the directory listing to see if you already have a public SSH key.
# Copy existing ssh key
$ cat ~/.ssh/id_rsa.pub
```
### Untrack files already added to git repository based on .gitignore 
1) Commit all your changes
Before proceeding, make sure all your changes are committed, including your .gitignore file.

2) Remove everything from the repository. To clear your repo: `git rm -r --cached .`
```console
#rm is the remove command
#-r will allow recursive removal
#–cached will only remove files from the index. Your files will still be there.
#The . indicates that all files will be untracked.
```
The rm command can be unforgiving.

3) Re add everything `git add .`

4) Commit `git commit -m ".gitignore fix"`
Done! Repo is clean :)
