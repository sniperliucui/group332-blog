---
title: Hello World
tags: hello world
categories: hello world
toc: true
date: 2023-01-14 00:00:00

---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

<!--more-->

## Quick Start

This is my fisrt page.

### Create a new folder

```bash
$ mkdir blog-source
$ cd blog-source
$ hexo init  # 初始化仓库
$ tree -L 1  # 查看结构(brew install tree)
```


### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)


### Run quickly

```bash
$ vi run.sh
```

with the following:

```
hexo clean 
hexo g
hexo s
```


start your local view with the code as follows:
```bash
sh run.sh
```


## Details

### Mathematical formula


use following code

```bash
{% mathjax %} 
{V = \left\lbrace v_{0}, v_{1}, …, v_{n} \right\rbrace} 
{% endmathjax %}
```

Math formulas can be displayed correctly, as follows:

{% mathjax %} 
{V = \left\lbrace v_{0}, v_{1}, …, v_{n} \right\rbrace} 
{% endmathjax %}



### Code 

python code test

```python
import numpy as np

a = np.arange(1, 1, 10)
```


java code test
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

## Blog 

![demo](/images/demo.png)



