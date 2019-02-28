---
layout: post
title: Running Docker commands on multiple items
date: 2019-02-27 13:04:29 +0000
author: Alan Gardner
categories: docker devops
---

I recently had to delete a number of docker images from my local machine. The command for removing images has the following format:

```bash
docker image rm [OPTIONS] <image_id_1> [<image_id_2> <image_id_3 ...]
```

My initial instinct was to use a file glob, to see if that worked.

```bash
docker image rm foo*
```

However it turns out that you can’t do this. The way to achieve this instead is to use the result of a listing call to generate the required IDs. For example, let’s say you want to remove all images from your machine that started with the word `foo`. You can list the images that have a REPOSITORY name that includes the term "foo" with the command:

```bash
$ docker images foo*

REPOSITORY    TAG       IMAGE ID        CREATED         SIZE
foo_web       latest    875f314768f4    5 months ago    123MB
foo_worker    latest    12ab875f3147    3 months ago    456MB
```

The output of the `docker images` command is a table by default, but we only want the image IDs so that we can pass them to the `docker image rm` command. We can do this with the `-q` option:

```bash
$ docker images -q foo*

875f314768f4
12ab875f3147
```

Putting the commands together we get:

```bash
$ docker image rm $(docker images -q foo*)
```

or if you use Fish shell:

```bash
# don't include $ before parentheses and quote the glob
$ docker image rm (docker images -q 'foo*')
```
