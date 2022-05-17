# Git

本文阐述 Git 分布式版本控制系统在 Ubuntu 系统下的安装和使用。

## Git 工作模式

<img src="https://user-images.githubusercontent.com/45534476/168778955-0316f532-8ded-4561-aa04-25b250ea7c4c.png" width="500">

Workspace：工作区

Index/Stage：暂存区

Repository：本地仓库

Remote：远程仓库

## 安装和配置

```
colinfx@local:~$ sudo apt-get update
colinfx@local:~$ sudo apt-get install git
colinfx@local:~$ git --version
colinfx@local:~$ git config --global user.name "xiao.fei"
colinfx@local:~$ git config --global user.email "xiao.fei@polytechnique.edu"
```

注意替换个人用户名和邮箱地址，作为多人协作标识。

## 基本操作

### 初始化本地仓库（repository）

#### 指令

```
colinfx@local:~$ cd ~/testgit
colinfx@local:~/testgit$ git init
```

#### 解释

注意替换文件地址为实际位置，可以新创建一个工作区（workspace），也可以在现有的代码工作区的基础上初始化本地仓库。命令完成后会在该文件夹内新增一个 .git 子文件夹，用于跟踪管理版本。

### 添加变动到暂存区（stage）

#### 指令

``colinfx@local:~/testgit$ git add readme.txt``

``colinfx@local:~/testgit$ git add testfolder``

``colinfx@local:~/testgit$ git add -p``

#### 解释

`add` 命令将工作区中的单个文件或整个文件夹的所有变动暂存到暂存区（stage）。

`-p` 参数暂存整个工作区所有文件的变动。

### 提交一个暂存区快照到本地仓库（repository）

#### 指令

``colinfx@local:~/testgit$ git commit -m "add readme.txt"``

``colinfx@local:~/testgit$ git commit -a``

``colinfx@local:~/testgit$ git commit -am "add readme.txt"``

#### 解释

`commit` 命令将暂存区中的文件提交到本地仓库（repository）从而创建一个当前暂存的工作区文件状态和内容的快照。

`-m "add readme.txt"` 为本次提交添加一则注释。若没有`-m` 参数则会自动打开一个本地文本编辑器以输入注释。

`-a` 参数将当前工作区所有被追踪的文件改动，无论是否有被添加到暂存区，都考虑在内创建一个快照。所有在之前被 `add` 命令执行的以及不在 `.gitignore` 列表中文件都被视为被追踪的。

`-amend` 参数会修改上一次提交的快照而不是创建一个新的快照。

### 本地仓库检查

`git status` 命令检查 `git add` 和 `git commit` 命令的当前状态。

`git log -n <limit>` 命令查看近若干次的 `commit` 记录。

## 参考教程

https://www.eet-china.com/mp/a85036.html

https://www.atlassian.com/git/tutorials/undoing-changes
