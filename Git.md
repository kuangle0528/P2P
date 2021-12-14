# Git基本使用

### 1 基本使用

#### 初始化

``` shell
git init
```

#### 查看状态

```shell
git status
```

#### 添加到暂存区

``` shell
git add .
```

#### 提交到本地库

``` shell
git commit -m '注释'
```

### 2 创建用户

#### 系统用户

```shell
git config --system user.name "名字"
git config --system user.email "邮箱"

#查看系统用户
git config --system --list 
```

#### 全局用户

```shell
git config --global user.name "名字"
git config --global user.email "邮箱"

#查看全局用户
git config --global --list 
```

#### 本地库用户

```shell
git config user.name "名字"
git config user.email "邮箱"

#查看用户
git config --list 
```

### 3 基本操作

#### 查看区别

```shell
#比较工作区和暂存区
git diff 文件

#比较暂存区和分支区
git diff --cached 文件
```

#### 忽略文件

- 新建**.gitignore**文件

- 将要忽略的文件名添加到**.gitignore**中

- 添加和提交**.gitignore**文件

#### 撤销修改

- 工作区修改  未添加到暂存区

  从暂存区中剪切一份文件到工作区

  ```shell
  git checkout -- 文件
  ```

- 添加到暂存区  未提交到本地版本库

  ```shell
  git reset HEAD 文件
  ```

- 提交到本地版本库

  （回退到之前版本）

  - 查看日志信息

    ```shell
    git log 
    
    #以单行形式简单展示历史记录信息
    git log --pretty=oneline
    
    #简写commit-id方式来显示
    git log --pretty=oneline --abbrev-commit
    
    #回车：显示下一行
    #空格：显示下一页
    #q：退出git log命令
    
    #查看所有历史版本
    git reflog
    ```

  ```shell
  HEAD是当前版本，HEAD^是上一个版本
  #回退到指定版本
  #本地库的版本回退，工作区和暂存区没回退
  git reset --soft HEAD^
  git reset --soft commit-id
  
  #回退到指定版本
  #暂存区和本地库的版本都回退，工作区没回退
  git reset --mixed HEAD^
  
  #回退到指定版本
  #工作区，暂存区和本地库的版本都回退
  git reset --hard HEAD^
  ```
  
  #### 删除文件
  
  查看暂存区文件列表
  
  ```shell
  git ls-files
  ```
  
  查看本地库文件列表
  
  ```shell
  #查看主分支当前HEAD指针所指向的时间点的文件列表，查看上一版本可使用HEAD^
  git ls-files --with-tree=HEAD
  ```
  
  仅删除暂存区中指定文件 
  
  ```shell
  git rm --cached 文件
  
  #恢复
  git reset HEAD 文件
  ```
  
  完全删除
  
  ```shell
  #工作区和暂存区删除，本地库中还存在，若要将本地库中的也删除，直接commit即可
  git rm 文件
  ```


### 4 分支

#### 分支基本操作

  ```shell
  #创建并切换到分支
  git checkout -b 分支名称
  
  #等价于两条命令
  #新建dev分支
  git branch dev
  #切换到分支dev
  git checkout dev
  ```

  #### 查看系统分支

  列出当前系统中存在的所有分支，且当前分支前会显示*

  ```shell
  git branch
  ```

  #### 删除分支

  必须保证当前分支不是要被删除的分支

  ```shell
  git branch -d 分支名
  
  #强制删除
  git branch -D 分支名
  ```

  #### 合并分支

  如果要将分支B合并到分支A上，首先要将当前分支切换到A分支上，然后再运行合并分支命令

  ```shell
  git merge 分支B
  ```

  #### 分支合并冲突

修改同一文件同一行上同一列，会产生冲突

修改同一文件同一行上不同列，不会产生冲突

 在master分支下查看冲突的文件，可以看到冲突提示内容

**======= **    为冲突分隔线

**<<<<<<<HEAD ** 到分隔线的内容表示：当前分支中的内容

分隔线到    **>>>>>>>dev **  的内容表示：合并的dev分支的内容

#### 解决冲突

手动修改冲突内容，然后再add添加并commit提交

 		需要注意，冲突解决后提交的最终版本，即合并后的版本，仅仅是master分支中的版本，dev分支中的版本仍然是合并前的版本。即对于解决冲突后的master中的版本要超前于dev中的版本。
 		由于合并的本意是将dev分支上完成或阶段性完成的工作提交到master分支中，dev分支的工作就暂告一个段落。后面dev分支上的工作再开始时，要与master分支的内容保持一致，从master分支内容开始，所以一般在合并后，会立即将dev分支删除。再开始时重新创建dev分支。
查看master分支中的版本，为解决冲突后合并的最终版本。

#### 查看历史版本的合并显示

```shell
git log --pretty=oneline --graph

#简短commit-id
git log --pretty=oneline --graph --abbrev-commit
```

### 远程仓库

#### 从本地推送到远程

```shell
git remote add origin git@github.com:kuangle0528/P2P.git
#为一个远程版本库指定一个本地名称，例如git@github.com:kuangle0528/P2P.git的本地名称为origin
```

```shell
git push origin master 
#将本地版本库中master分支推送到origin远程库中

git push origin 
#将本地版本库中当前分支推送到origin远程库中

git push -u origin master
#将本地版本库master分支推送到origin远程库中，并将origin设置为默认的远程库，以后不用指定远程版本库名
```

#### 从远程克隆到本地

```shell
git clone git@github.com:kuangle0528/P2P.git
```

#### 从远程拉取/抓取到本地

```shell
git pull origin master 
#将远程仓库中master拉去到本地库，并与本地库master分支进行合并

git pull origin master:dev
#将远程仓库中master拉去到本地库，并与本地库dev分支进行合并

git pull
#从默认的远程仓库中拉取到本地当前分支，并合并
```

```shell
git fetch (远程库 分支名） 本地库分支名
#从远程库中抓取到本地，不会合并

git merge 远程分支
#合并分支
```

**pull** 相当于 **fetch + merge**

#### 查看本地的远程库信息

```shell
git remote

git remote -v #更详细

#查看本地库与远程库的对应关系
git branch -vv
```

#### 删除本地的远程库信息

```shell
git remote rm origin
```

### 版本发行

#### 轻量级标签

```shell
git tag 标签名称 commit-id
#若不指定commit-id,默认是当前版本
```

#### 附注标签

```shell
git tag -a 标签名称 -m 标签说明 commit-id
```

#### 查看标签

```shell
git tag
#输出顺序是字母顺序

git show 标签名称
#查看标签详情
```

#### 修改指定版本的代码

​		软件版本一旦被指定，即标签一旦与某一commit-id绑定，那么这个版本的代码还能修改吗﹖若将master回退到该commit-id，然后再修改代码，修改完成后再commit,我们会发现代码修改过了，但该标签绑定的commit-id并没有发生变化，即该软件版本的代码仍未修改。当然，此时我们可以将该标签删除，然后再定义一个同名标签与修改过代码的commit-id 绑定。这样也是可以的。

​		软件版本一旦被指定，即标签一旦与某一commit-id绑定，那么这个版本的代码还能修改吗﹖若将master回退到该commit-id，然后再修改代码，修改完成后再commit,我们会发现代码修改过了，但该标签绑定的commit-id并没有发生变化，即该软件版本的代码仍未修改。当然，此时我们可以将该标签删除，然后再定义一个同名标签与修改过代码的commit-id 绑定。这样也是可以的。

​		那应该怎么办?Git 将标签定义为了与分支同级别的概念，它丕仅驻是一个commit-id 的别名。Git 允许程序员使用分支切换命令git checkout将代码转向标签所指定的版本。

```shell
lok@DESKTOP-UL5IGK5 MINGW64 /e/git学习/P2P (master)
$ git checkout v3.0
Note: switching to 'v3.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 51f87dc 淇敼Git.md

lok@DESKTOP-UL5IGK5 MINGW64 /e/git学习/P2P ((v3.0))
$
```

​		命令执行完毕，系统给出了很多的提示，该提示的总体意思为:当前处于“游离”状态，在该状态下用户的任何修改与提交对任何的分支都没有影响(言外之意是:**其修改将不会被保留**）。若保留修改，则可以通过**git checkout -b**命令创建一个新的分支。

```shell
git checkout dev
```

​		在该新分支上再进行修改提交，然后再合并到master 分支，最后再将该分支删除。此时创建的分支名称可以随意。当前，合并到master 分支后，仍需要删除原标签，然后再与新的commit-id绑定。不过，生产环境下，一旦标签定义完成就不会删除。而是会再定义一个新的标签与新的commit-id绑定。

#### 推送标签

```shell
#推送指定标签
git push origin 标签名称

#推送全部标签
git push origin --tags
```

#### 删除标签

```shell
#删除本地标签
git tag -d 标签名称

#删除远程标签 ，需要先删除本地库中该标签
git push origin :refs/tags/标签名称
```

### 自定义远程版本库

1. 安装git

   ```shell
   # yum install -y git
   ```

2. 验证git安装是否成功

   ```shell
   # git --version
   ```

3. 创建git用户并设置密码 **kuangle**

   ```shell
   # useradd git
   
   # passwd git
   ```

4. 在git用户目录下创建**.ssh**文件夹，并在.ssh下创建**authoried_keys**文件

   ```shell
   # mkdir .ssh
   # touch .ssh/authoried_keys
   ```

5. 在**authoried_keys**文件中加入公钥

   ```shell
   # vim authoried_keys
   ```

6. 在**/usr**路径下创建**repositories**

   ```shell
   # cd /usr
   # pwd
   /usr
   # mkdir repositories
   ```

7. 初始化版本库

   ```shell
   # pwd
   /usr/repositories
   # git init --bare crm.git
   初始化空的 Git 版本库于 /usr/repositories/crm.git/
   ```

8. 修改版本库的权限

   ```shell
   # chown -R git:git crm.git
   # ll
   总用量 4
   drwxr-xr-x 7 git git 4096 12月 11 00:39 crm.git
   ```

9. 客户端连接自定义远程版本库

   ```shell
   $ git remote add custom git@192.168.200.130:/usr/repositores/crm.git
   ```
