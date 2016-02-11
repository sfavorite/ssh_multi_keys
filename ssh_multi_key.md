---
title: "Multiple SSH Keys"
author: "Scot Favorite"
date: "February 11, 2016"
output: pdf_document
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

![Key Name](images/ssh_key_name.jpeg)
