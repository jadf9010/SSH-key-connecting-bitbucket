# SSH-key-connecting-bitbucket for MACOS
Manage SSH access keys for a project. The keys apply to every repository in the project.

Using access keys avoids the need to store user credentials on another system, and means that the other system doesn't have to use a specific user account in Bitbucket Server. For example, access keys can be used to allow your build and deploy server to authenticate with Bitbucket Server to check out and test source code.
https://confluence.atlassian.com/bitbucketserver/ssh-access-keys-for-system-use-776639781.html

Before you can use SSH keys to secure a connection with Bitbucket Server the following must have already been done: 
-
- Your Bitbucket Server administrator must have already enabled SSH access, on Bitbucket Server.
- You must have already created an SSL key. See Creating SSH keys. Alternatively, you can use an existing key, if it isn't already being used for a personal account in Bitbucket Server.

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
> ssh-add -K ~/.ssh/id_rsa

DONE.
-
You can now open source tree and clone a branch.

Troubleshooting
  -
How to permanently add a private key with ssh-add on MacOS?

A solution would be to force the key files to be kept permanently, by adding them in your ~/.ssh/config file:

> IdentityFile ~/.ssh/gitHubKey
> IdentityFile ~/.ssh/id_rsa

This file ** config ** should be in the ~/.ssh directory.
you can use this command to modify it:

> nano ~/.ssh/config

https://stackoverflow.com/questions/3466626/how-to-permanently-add-a-private-key-with-ssh-add-on-ubuntu

https://apple.stackexchange.com/questions/48502/how-can-i-permanently-add-my-ssh-private-key-to-keychain-so-it-is-automatically
