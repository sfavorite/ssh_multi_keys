---
title: "Multiple SSH Keys"
author: "Scot Favorite"
date: "February 11, 2016"
output: html_document
---
---
title: "ssh shortcuts"
author: "Scot Favorite"
date: "February 11, 2016"
output: html_document
---

# Using multiple ssh keys
I like to use multiple ssh keys so if I need to revoke a key I only have to replace that one key on my laptop and the server I authenticate to with that key. 

This is my (simplified) environment

## On my laptop                 
github_key.pub

github_key

digital_key.pub

digital_key

## At github.com
github_key.pub

## At digital ocean
digital_key.pub

## Naming your key's 
You can name the keys when you generate them, which I highly recommend so you don't get them confused since the default will name them all the same...not to mention over write existing keys. 

Here is where you can put your own name in:
![Key Name](https://github.com/sfavorite/ssh_multi_keys/blob/master/images/ssh_key_name.jpg)

Once you have generated your keys copy the public key (the one with the .pub). Do NOT copy the private key (the one with no extension). 


$ scp ~.ssh/aserver_key.pub username@aserver:


This will added to the home directory of username.

Now add the rsa key to your authorized keys file. 

cat ~/aserver_key.pub >> ~/.ssh/authorized_keys

Remove the key from the home direcotory.

rm aserver_key.pub

Now that you have the keys added you need to setup a config file on your laptop to match the key to the remote server. 

