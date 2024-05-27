	---
title: "Creating Dockerfile"
date: 2023-09-12T13:01:00+03:00
draft: false
---

### Difference between CMD and ENTRYPOINT

Both instructions run command inside docker after it starts. Difference in behavior is happend when you add some additional params while run container:
```bash
docker run <some_image> <some_command>
```
If you set `some-command`, this command rewrite CMD instruction in Dockerfile. But if you have ENTRYPOINT instruction in Dockerfile, `some_command` will not rewrite, but **append** to the end of your ENTRYPOINT instruction. 

What happend if Dockerfile has both instructions - ENTRYPOINT and CMD? It will be joined, for example:
```bash
# some Dockerfile
ENTRYPOINT ['docker-entrypoint.sh']
CMD ["apache2-foreground"]
```

will be evaluated to:
```bash
docker run <some_image> docker-entrypoint.sh apache2-foreground
```


### Difference between ADD and COPY


### Difference between Bind Mount and Volume Mount

Volume mounting - 


