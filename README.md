# How to set up SSH keys and connect them to GitHub
[Check the original post](https://dev.to/augustocarmona/how-to-set-up-ssh-keys-and-connect-them-to-github-3136 "Check the original post")
----

## Ever wonder what SSH is for?
----
Every time we connect to GitHub, to be able to do a push from our repository we have to enter our password. But this has two problems: the first is that we always have to do it (every time we need to git push) -if you are going to use long and complicated passwords, like me, it can be quite annoying-, and the second is that in principle, we are working with secure connections (https).
However, when working in this way, the username and password are stored in the local environment, so if at any point our laptop is stolen, our repositories will become highly vulnerable.

![](https://i.ibb.co/TLdv8Mq/Captura-de-Pantalla-2021-04-23-a-la-s-10-58-53.png)
> HTTPS protocol

So to protect the projects we have on GitHub and our work, maybe the best option is to create a public and private key environment on Git and Github.

If we clone our repositories with SSH, not only will the security be much more stronger, but we will not have to re-enter our password to be able to git push.
To do this we must create a public key and a private key in our local environment, and once created we must send our public key to GitHub, in this way we can use the SSH protocol at all times.
This connection is not only 100% encrypted, but we can also add a new passphrase to make it more secure ...

![](https://i.ibb.co/80zN41C/Captura-de-Pantalla-2021-04-23-a-la-s-10-59-10.png)
> SSH protocol

----

### Generate a new SSH key: (Any operating system)
In your console you just have to enter the following command (with the email you have associated with your GitHub account):

`ssh-keygen -t rsa -b 4096 -C "your@email.com"`

This command generates the key and then asks to be indicated in which part of our system it should save said key (I suggest simply to press enter, since the default location it offers us is our user).

![](https://i.ibb.co/mC5C40r/0005.jpg)
> Linux

After this, it will ask for a passphrase (a password with spaces), for example: "` 7h1s is 4 pa55phr4se` ".
The passphrase is optional, but I suggest you add it. They will not be asked to git push.

From now on, your public and private keys are already created, you just need to add them to your environment...

### Check process and add it to the environment (Windows and Linux)

`eval $ (ssh-agent -s)`

Check that the SSH keyserver is on. It will return `agent pid -and a number-` and then you can continue.

![](https://i.ibb.co/V3XYcw9/0006.png)

`ssh-add ~/.ssh /id_rsa`

Finally add your private key with this command, it is located in home, in the ssh folder, and the key is called id_rsa (id_rsa.pub is the public key, we will use it later).

![](https://i.ibb.co/fNvrcYZ/0007.jpg)

Here you are adding the private key to the system. You should never copy and paste this private key anywhere or show it to anyone, just add it here.

### Check Process and Add It to Environment (Mac)

- `eval "$ (ssh-agent -s)"`

Again, verify that the SSH keyserver is turned on. It will return `agent pid -and a number-` and then you can continue.

**Are you using macOS Sierra 10.12.2 or higher?** You have to modify (or create) the config file.

This file is in the same folder as the SSH keys (with `ls -al` you can see if it exists)

You have to do the following:

- `cd ~/.ssh` to go to the SSH folder
- If config doesn't exist, create one ...
- With Vim: `vim config` (to work on the console, in my opinion, this is the best editor)
- With VSCode: `code config`

![](https://i.ibb.co/jLW2MbM/Captura-de-Pantalla-2021-04-25-a-la-s-18-39-24.png)

- Paste the following configuration into the file ...
```bash
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```
- Add your key:
`ssh-add -K ~/.ssh/id_rsa`

![](https://i.ibb.co/5Fkddvg/Captura-de-Pantalla-2021-04-25-a-la-s-18-38-51.jpg)

----

###### SSH keys are not per repository or project, but per person, so I recommend following this process for each computer that you want to have connected to GitHub.

----
## Connecting to GitHub with SSH:
Find your public key where you have it stored (your .ssh folder, and it's called id_rsa.pub), open it with any editor (it can be VsCode or Vim), and copy it.

![](https://i.ibb.co/CQNGL4m/0004-3.jpg)

After this, go to your GitHub profile, to the `settings` tab,  option ` SSH and GPG Keys`,` New SSH Key`, name it as you like: "Dell Linux for example" and there paste your key public in `Key`.

![](https://i.ibb.co/PtBczfH/0002-2.jpg)

Hit `Add SSH Key`, enter your password and voila!

![](https://i.ibb.co/4TW8s8v/0001.jpg)

From now on, whenever you want to clone a repository on GitHub instead of connecting it with the HTTPS protocol, do it with the SSH protocol and you will no longer have to re-enter passwords every time you git push. And it's a thousand times more secure!
