
# Using multiple ssh keys

I like to use multiple ssh keys so if I need to revoke a key I only have to replace that one key pair on my laptop and the server I authenticate to with that key. 


## This is my (simplified) environment

### On my laptop                 
github_key.pub

github_key

digital_key.pub

digital_key

### At github
github_key.pub

### At digital ocean
digital_key.pub

## Naming your keys 

You can name the keys when you generate them, which I highly recommend so you don't get them confused since the default will name them all the same...not to mention over write existing keys. 

Here is where you can put your own name in:
![Key Name](https://github.com/sfavorite/ssh_multi_keys/blob/master/images/ssh_key_name.jpg)

At the prompt enter a name that makes sense to you. I use 'servername_rsa' that way the name of the key tells me which server and the rsa tells me the encryption protocol I used. 


## Copy key to remote server

Once you have generated your keys copy the public key (the one with the .pub). Do NOT copy the private key (the one with no extension). 

```
$ scp ~.ssh/server1_key.pub username@server1:
```

This will added to the home directory of username.

Now add the rsa key to your authorized keys file. 

```
cat ~/server1_key.pub >> ~/.ssh/authorized_keys
rm ~/server1_key.pub
```

## SSH config file

Now that you have the keys added, you need to setup a config file on your laptop to match the key to the remote server.

In your home directory create a config file if one does NOT already exist. If one already exists skip creating a new file and use the existing. 

```
$ touch .ssh/config
$ chmod 700 .ssh/config
```

This will set the permissions to read only for the user. 

Now we need to tell ssh how to use all the keys in our .ssh directory. The basic outline of the config file is:

Host server1
      HostName p1.server1.com
      IdentityFile server1_rsa
      User username

Let's break down what each of the four lines does. 

1. Host server1: this is a short name that you will use to ssh into the server. This name does not need to be in DNS
2. HostName p1.server1.com: this is the DNS resolvable name of the server
3. IdentityFile: This is the private key
4. This is the username you want to use on the remote server. Most of the time this is your normal user id but not always. 

Here is a short version of my config file.

```
Host github

    HostName github.com
    
      IdentityFile ~/.ssh/git_rsa
    
      User sfavorite

Host digital

    HostName p1.scotfavorite.net
    
      IdentityFile ~/.ssh/digitalocean_rsa
    
      User root
```

Hope this helps you manage your ssh keys in a secure manner. 
    