---
title: "Tips&Tricks in Linux CLI I widly use, part 2"
date: 2023-08-13T12:19:00+03:00
draft: false
---

So, in [first part](https://leks.us/posts/tipstricks-in-linux-cli-i-widly-use-part-1/) I wrote about keys I push to improve expirience with Linux CLI. Now I want to tell you about built-in commands.

##### `cd - ` to back to previous directory

You edit nginx config in `/etc/nginx/sites-available/`, and also need to create/edit file/folder in root site directory. You can switch between this directories set command `cd - `. This command jump you to previous directory. It has just one remembered directory - previouse. So you can loop between two working dorectories.

##### `!!` - insert last comand 

It is very usual situation when you are working in CLI and receive result `permission denied`. Oh, you forget, this command requires SUDO. So, what to do? When I was a newbie in CLI, I press up arrow button, then go to the begining of command and write `sudo `, then press enter. Booring. Now I do it easlier:
```bash
sudo !!
```

This comand is evaluates in `sudo <your last command>`. So, just write it and press Enter!

##### `!$`, `!*` - insert almost last comand

This is variation of `!!` command. `!$` insert last part of the command (command splitted by space), `!*` insert all except first part (fisrt part of command is always some program).

Where is it using? Well, `!$` I use rarly, for example, you want to know what is inside some big file. And if it is junk, delete it. You run command:
```bash
tail -100 /path/to/some_file
```
to see the last 100 lines. Ok, it's junk. So, now you need to write `rm` and repeat path to file. With `!$` you don't need it, just write:
```bash
rm !$
```
After pressing Enter this command evaluates to `rm /path/to/some_file`. Nice, isn't it?

`!*` command I use more often. It is useful when you need change program to run, leave it's params untouched. Most common use for me is upload/download files via ssh with `scp` program. To connect via ssh you usually write something like:
```bash
ssh user@some.ip.address.here
```

Well, I don't remember a huge count of IP-addreses I working with. So, what I do? After disconnecting via SSH I type:
```bash
scp !*
```
And string evaluates to: 
```bash
scp user@some.ip.address.here
```
It's a magic! Next I write second path of this command (source or dest) and fill path to file I copy.

##### Replace part of wrong string in previouse command to correct

Imagine, you write a long, very command, and after running understand that you have a mistake. How often people do? They push up arrow and move cursor to the place with mistake and fix it. Right? Right, but it's not fast. Here I teach you the fasters method. For example, you type wring port in this command (should be 9443 instead of 9442):
```bash
docker run -d -p 9442:9442 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
The best way to resolve it is write this:
```bash
^9442^9443
```
After push Enter this command evaluates to the previous command with replaced strings from `9442` to `9443`! 

Maybe I will remember more useful tips I using, but this is most common. I hope, you will take it to enforce your expirience with Linux CLI!