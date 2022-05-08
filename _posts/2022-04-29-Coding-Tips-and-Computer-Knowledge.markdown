---
layout: post
title:  "Coding Tips and Computer Knowledge"
date:   2022-04-29
categories: jekyll update
---
## Introduction

Here is a list of several useful coding tips.

## Jupyter Notebook

.jupyter/jupyter_notebook_config.py and .jupyter/jupyter_notebook_config.json are two configuration files, the latter one has higher priority. 

```bash
jupyter notebook password
```

will generate password in `.json` file.

If you want to use password, use the code above. If you want to use no token no password, clear the password in `.json` file and add `jupyter_notebook_token = ""`in `.py` file.



## SSH

ssh-key is used to provide security. SSH is like a column of boxes. We can apply for one of them. At first, we will have password to login, but for more security, we  can buy a pair of key (private key) and lock (public key), we send the lock on the box (We need to add it into authorized _keys) and preserve the key in our .ssh directory locally. The config file is another way to log into ssh without password, which can use the arbitrary id_rsa file (private key) for authentication.



## Network

the gateway is the switcher (交换机), which always set it IP as 192.168.xxx.2. For example, the NAT mode for virtual machine in vmware: we set the vmenet8 (virtual network adapter 虚拟网卡) for the host (宿主) IP as 192.168.xxx.1, and the  gateway IP as 192.168.xxx.2, and the virtual machine IP as 192.168.xxx.3-254. Then the virtual machine can reach the internet directly. And it can also be reached from internet by port forwarding on host port.

