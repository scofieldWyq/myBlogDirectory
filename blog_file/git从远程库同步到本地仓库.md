title: git从远程库同步到本地仓库
date: 2016-02-29 01:38:04
tags:

- git
- 远程仓库
- merge

---

# 目录
1. [远程仓库发生改变，本地仓库没有改变](#d1)
2. [远程仓库没有改变，本地仓库发生改变](#d2)
3. [远程仓库，本地仓库都发生改变](#d3)

# 有三种方式的情况下，如何做到没有Bug的合并代码。。。
---


## <span id="d1">远程仓库发生改变，本地仓库没有改变</span>

- 首先，查看远程仓库 `git remote -v`
```{bash}

{UserName}-MacBook-Pro:Understanding_Unix-Linux_Programming {UserName}$ git remote -v
origin	git@github.com:{User}/Understanding_Unix-Linux_Programming.git (fetch)
origin	git@github.com:{User}/Understanding_Unix-Linux_Programming.git (push)

```

- 把远程库更新到本地 `git fetch origin master`
```{bash}

{UserName}-MacBook-Pro:Understanding_Unix-Linux_Programming {UserName}$ git fetch origin master
Warning: Permanently added the RSA host key for IP address '{IP address such as: 192.168.1.1 }' to the list of known hosts.
From github.com:{User}/Understanding_Unix-Linux_Programming
 * branch            master     -> FETCH_HEAD

```

- 比较远程更新和本地版本库的差异 `git log master.. origin/master`
```{bash}

{UserName}-MacBook-Pro:Understanding_Unix-Linux_Programming {UserName}$ git log master.. origin/master
commit ce39f8b3eeee898a2a038444f897f2aef3673493
Author: {User} <794870409@qq.com>
Date:   Fri Feb 26 14:14:39 2016 +0800

    {The context origin added ... }

```


- 合并远程库 `git merge origin/master`
  + 有差异
```{bash}

wuyingqiangs-MacBook-Pro:Understanding_Unix-Linux_Programming wuyingqiang$ git merge origin/master
Updating eb32b20..ce39f8b
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```
  + 无差异
```{bash}

wuyingqiangs-MacBook-Pro:Understanding_Unix-Linux_Programming wuyingqiang$ git merge origin/master
Already up-to-date

```
---

## <span id="d2">远程仓库没有改变，本地仓库发生改变</span>
  以后再说咯。。。

---

## <span id="d3">远程仓库，本地仓库都发生改变</span>
  以后再说咯。。。

---

# 总结

- 掌握 `git` 远程和本地的同步很重要的。
- 还有解决同步产生的问题也是锻炼自己的一个机会。
