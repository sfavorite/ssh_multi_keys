
# Using multiple ssh keys

I like to use multiple ssh keys so if I need to revoke a key I only have to replace that one key pair on my laptop and the server I authenticate to using that key pair.

Before doing any of the steps I <strong>highly</strong> recommend backing up all of the files in your .ssh directory. If you do not you can/will lock yourself out of accounts/systems! You have been warned!


## This is my (simplified) environment


### On my laptop  
Github keys         
-----------      
github_key.pub

github_key

Digital Ocean keys (hosting site)
---------------------------------
digital_key.pub

digital_key

### At github
github_key.pub

### At digital ocean
digital_key.pub

## Naming your keys

You can name the keys when you generate them, which I highly recommend so you don't get them confused since the default behavior of ssh-keygen is to name them all the same...not to mention if you don't it will over write existing keys you may have already. You did back up your .ssh directory, right? Warned twice.

Here is where you can put your own name in:
![Key Name](https://github.com/sfavorite/ssh_multi_keys/blob/master/images/ssh_key_name.jpg)

At the prompt enter a name that makes sense to you. <strong>If you do not put in a custom name you will over write any existing id_rsa key you have</strong>. Warned a third and final time. I use the pattern 'servername_crypto' that way the name of the key tells me which server and the crypto tells me the encryption protocol I used. So something like github_rsa for a Github key using RSA.

## Copy key to remote server

This will work for most servers but some, like Github, have a different protocol
for adding ssh keys. For Github you can find the directions [here](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

For normal servers once you have generated your keys copy the public key (the one with the .pub) to the remote system/server. Do NOT copy the private key (the one with no extension).

```
cd ~/.ssh
$ scp server1_key.pub username@server1:
```

This will added your public key to the home directory of username. Now ssh into the remote system.

```
ssh sfavorite@myotherserver.com
```

Now we will add the rsa key to your authorized keys file and remove it from your home directory.

```
cat ~/server1_key.pub >> ~/.ssh/authorized_keys
rm ~/server1_key.pub
```

## Testing Github

Once you have completed the above for Github you can follow their testing instructions
found [here](https://help.github.com/articles/testing-your-ssh-connection/). We will test connecting to other servers in a minute.

## SSH config file

Now that you have the keys added, you need to setup a config file on your <strong>laptop</strong> to match the key to the remote server.

In the .ssh directory of your laptop create a config file if one does NOT already exist. If one already exists skip creating a new file and use the existing.

```
$ cd .ssh/
$ touch config
$ chmod 700 .ssh/config
```

This will set the permissions to read only for the user of that home directory.

Now we need to tell ssh how to use all the keys in our .ssh directory. The basic outline of the config file is:

```bash
Host server1
    HostName p1.server1.com

    IdentityFile server1_rsa

    User username
```
Let's break down what each of the four lines does.

1. *Host server1*: this is a short name that you will use to ssh into the server. This name does not need to be in DNS
2. *HostName p1.server1.com*: this is the DNS resolvable name of the server
3. *IdentityFile*: This is the private key
4. *User username*: This is the username you want to use on the remote server. Most of the time this is your normal user id but not always.

Here is a short version of my config file.

```
Host github.com

    HostName github.com

    IdentityFile ~/.ssh/git_rsa

    User sfavorite

Host digital

    HostName p1.scotfavorite.net

    IdentityFile ~/.ssh/digitalocean_rsa

    User root
```

Please note in the above configuration the first line 'Host github.com' is the resolvable name. It needs to be github.com since you will be using commands such as 'git remote add origin git@github.com/sfavorite/coolstuff' and your system will try to match what is after the @ with your config file. The second server configuration starts with 'Host digital'. I have set my /etc/hosts file so the word digital resolves to my droplet. If you don't want to change your host file you should have *Host p1.yourdomain.com*.


## Test connection

Attempt ssh login without a user specified and hope for no password prompt.

![Test Login](https://github.com/sfavorite/ssh_multi_keys/blob/master/images/testing_login.jpg)

Success! We can now login to our configured servers using ssh-keys.

Hope this helps you manage your ssh keys in a secure manner.
