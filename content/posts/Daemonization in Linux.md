---
title: "Daemonization in Linux"
date: 2023-07-25T23:48:00+03:00
draft: true
---

First of all, let's figure it out what is daemonization. Imagine, you have simple python script you run on some server via ssh. If you close ssh connection, script will stop working.

How to resolve this issue? Well, you can install terminal manager like `tmux`. It creates multipy terminals and you can switch between it. And switching to another terminal don't stop running processes in previous terminal window. There is also `screen` app with the same functionality.

But what if your server will be rebooted? Unfortunately, your script will not started. And now we announced process daemonization! You can daemonize any script, any process, any program. And it will can autostart after server's boot, it can auto restart if falture, it can work in background, and you can read logs with nice view!

`systemd` is a linux program-arbitr for other programs. Generally speaking, this is the program to run other programs and control this running programs. Program which can run in background named daemon. When you move usual program to background you make daemonization of this program.

It is very easy to make daemonization fo your script (or any program). Just follow this steps. Note, all of these steps should be running under root priveleges:

1. Create file in `/etc/systemd/system` folder named like your script with `.service` extension. For example, `my_cool_script.service` with this text inside:
```ini
[Service]
ExecStart=/path/to/python3 /path/to/script/my_cool_script.py
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Reload systemd service. To manage this program there is another program named `systemctl`. So you should run reload command like this:
```bash
systemctl daemon-reload
```
3. Start your script to executing:
```bash
systemctl start my_cool_script.service
```
4. If you need to start your script after system boot, run this command. It will create symlink and enable this feature:
```bash
systemctl enable my_cool_script.service
```

And that's all! Let's explain what means every line at the `my_cool_script.service` file.
`[Service]` - is a name of settings section, you should leave it as is. Next `ExecStart` stores command systemd should to run. `Restart` - is param to set sholud systemd restart your script after it's fault or not. `[Install]` is settings section when we start our daemonized program. `WantedBy=multi-user.target` mean that your script should run after server boot.

Next time I'll tell you about daemonize more complex scripts like Django.