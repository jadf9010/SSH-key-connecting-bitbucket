# SSH-key-connecting-bitbucket for MACOS
Manage SSH access keys for a project. The keys apply to every repository in the project.

Using access keys avoids the need to store user credentials on another system, and means that the other system doesn't have to use a specific user account in Bitbucket Server. For example, access keys can be used to allow your build and deploy server to authenticate with Bitbucket Server to check out and test source code.
https://confluence.atlassian.com/bitbucketserver/ssh-access-keys-for-system-use-776639781.html

Before you can use SSH keys to secure a connection with Bitbucket Server the following must have already been done: 
-
- Your Bitbucket Server administrator must have already enabled SSH access, on Bitbucket Server.
- You must have already created an SSL key.

Using SSH keys to allow access to Bitbucket Server repositories
To get the SSH key to work with your build, or other, system, you need to:

Create the SSH key:
  -  
  An SSH key consists of a pair of files. One is the private key, which should never be shared with anyone. The other is the public key:
  
1- Ensure you have a SSH key first. Or create one on the command line:\
2- Open the Terminal
> ssh-keygen -t rsa -C "your_email@example.com"

Your private key is saved to the **id_rsa** file in the **.ssh directory** and is used to verify the public key you use belongs to the same Bitbucket Server account.

Your public key is saved to the **id_rsa.pub;** file and is the key you upload to your Bitbucket Server account.


If you want to see where the system keep the SSH keys (Public and Private)
-
- Open the terminal
- cd ~/.ssh
- ls

**You'll see the SSH keys (Public and Private)**
> config\
> id_rsa.pub\
> id_rsa\
> user-Bitbucket.pub\
> user-Bitbucket\
> key_backup

Add the public key to Bitbucket Server account as described here:
  -    
Add an SSH access key to either a Bitbucket Server project or repository
You simply copy the public key, from the system for which you want to allow access, and paste it into Bitbucket Server.

Copy the public key. One approach is to display the key on-screen using cat, and copy it from there:

```
1- Open the terminal
2- cat < ~/.ssh/id_rsa.pub  
3- Now, in Bitbucket Server, go to the Settings tab for the project or repository.
Click Access keys and then Add key.
4- Paste the key into the text box and click Add key.
```
Now go back to your command line and follow the next steps:

Add the private key to that system. For Bitbucket in MacOs:
  -  
  Open the Terminal:
> ssh-add -K ~/.ssh/[your-private-key]  
> ssh-add -K ~/.ssh/id_rsa

**This command use the -K parameter that allow store the private key in the keychain in MAC**  

## Configure SSH for Git Hosting Server

Add the following text to `.ssh/config` (`.ssh` should be found
in the root of your user home folder):

```
Host github.com
 Hostname github.com
 User userName
 IdentityFile ~/.ssh/id_rsa 
 UseKeychain yes
 AddKeysToAgent yes
```
  
Configure SSH to always use the keychain
-
In the section Troubleshooting below It was solved

DONE.
-
You can now open source tree and clone a branch.

TIPS
-
This show all the identities that has the agent.
> ssh-add -L

This is for list all the elements in the folder **.ssh**
> ls -al ~/.ssh

Troubleshooting
  -
**How to permanently add a private key with ssh-add on MacOS?**

**Solution**\
After you add the private key : 

> ssh-add -K ~/.ssh/[your-private-key]

You must run this command:

> echo ssh-add -A | cat >> ~/.bash_profile

Or

> echo ssh-add -K ~/.ssh/[your-private-Key] | cat >> ~/.bash_profile

Theses command will be added to **.bash_profile** file the command you've added **ssh-add -A** or **ssh-add -K ~/.ssh/[your-private-Key]**
> **ssh-add -A** : run manually to load your saved keychain.

The **.bash_profile** contains all the startup configuration and preferences for your command line interface. 

https://unix.stackexchange.com/questions/140075/ssh-add-is-not-persistent-between-reboots/335720#335720?newreg=eab245b81e504751a3820ade565a0729

Others solutions
-

A solution would be to force the key files to be kept permanently, by adding them in your ~/.ssh/config file:

> IdentityFile ~/.ssh/gitHubKey\
> IdentityFile ~/.ssh/id_rsa

This file **config** should be in the ~/.ssh directory.
you can use this command to modify it:

> nano ~/.ssh/config

**Other solution could be to solved that problem by using -K option for ssh-add:**

ssh-add -K ~/.ssh/your_private_key

But this way no longer works. Keys added to the keychain via ssh-add -K are not automatically re-added to the ssh-agent after a reboot.

> In ~/.ssh create config file with the following content:

```
Host * (asterisk for all hosts or add specific host)
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile <key> (e.g. ~/.ssh/userKey)
```
SSH issues
-
If you're having problems with SSH, here are some things you can try when troubleshooting your issues.

Test SSH authentication
-
Use the commands in this section to troubleshoot SSH authentication issues.
https://confluence.atlassian.com/bitbucket/troubleshoot-ssh-issues-271943403.html

This command checks your SSH agent for an SSH key, and then checks if that private key matches a public key for an existing Bitbucket account:

```
Git
$ ssh -T git@bitbucket.org
```
If you don't have any keys loaded in the agent:
```
$ ssh -T hg@bitbucket.org
Permission denied (publickey).
```
If your local machine is unable to get the bitbucket.org IP address:
````
$ ssh -T hg@bitbucket.org
ssh: connect to host bitbucket.org port 22: Connection refused
````
If your connection is successful:
````
$ ssh -T hg@bitbucket.org
conq: logged in as teamsinspace.
You can use git or hg to connect to bitbucket. Shell access is disabled.
````
https://stackoverflow.com/questions/3466626/how-to-permanently-add-a-private-key-with-ssh-add-on-ubuntu

https://apple.stackexchange.com/questions/48502/how-can-i-permanently-add-my-ssh-private-key-to-keychain-so-it-is-automatically

https://github.com/jirsbek/SSH-keys-in-macOS-Sierra-keychain

https://github.com/wwalker/ssh-find-agent
http://blog.joncairns.com/2013/12/understanding-ssh-agent-and-ssh-add/
https://gist.github.com/bsara/5c4d90db3016814a3d2fe38d314f9c23
