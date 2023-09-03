---
title: "SSH auth key forwarding"
date: 2023-08-02T12:34:00+03:00
draft: false
---

Ok, Let's imagine that you have git repositary with the code you need to run in production server. And of course you have access to this server via ssh with ssh key authentication. 

To get code from git repositary to the server you have two options:
1) via HTTPS and enter credentials every time you download source code
2) via SSH and you need to setup ssh key authentication between production server and git server.

Obviously, first option is useless, so we have to use second. So, let's use second! But actually we have third option - key forwarding. It allows you to use your local ssh keys on the server when you connected to this server. 

Here step-by-step guide to set ssh key forwarding.

### Enable forwarding in ssh config in local machine

```bash
# vim ~/.ssh/config
Host leks.us
  ForwardAgent yes
```

You should explicitly enable SSH key for certain server and set path to private key
```bash
# vim ~/.ssh/config
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

For MacOS, you also should add your private key in Apple keychain
```bash
ssh-add --apple-use-keychain ~/.ssh/id_rsa
```

If your MacOS says something like `ssh-add Error connecting to agent: No such file or directory` just run this:
```bash
ssh-agent <your shell>  # for zsh, for example: ssh-agent zsh 
```

Your production server by default should support ssh key forwarding. But, if you have any problem, try this [troubleshooting guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding#troubleshooting-ssh-agent-forwarding)

Here is some benefits of using SSH key forwarding:
1) You don't care about key management. Everybody use their own keys located in their computers.
2) Everybody has the same permissions based on their own local keys, you don't need to duplicate keys with the same permissions.

Of course,  there is some cons of this technology, for example, you can't setup different user permissions for github server from local machine and from production server.
