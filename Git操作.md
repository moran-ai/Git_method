



# Git操作

#### 1.先在本地创建一个文件夹，用来做为本地仓库

```shell
20622@DESKTOP-4PHULHH MINGW64 /
$ cd D:\githubfile

20622@DESKTOP-4PHULHH MINGW64 /d/githubfile
$ cd demo
```

#### 2.进行初始化

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo
$ git init
Initialized empty Git repository in D:/githubfile/demo/.git/
```

#### 3.需要提前在文件夹中创建一个md文件，然后在这个文件中写入一些内容

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ touch README.md
```

#### 4.初始化这个mk文件

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$  git add README.md
```

#### 5.提交这个md文件到远程仓库  

提交时出现了这个错误，原因是gitbase是区分大小写的，所以需要忽视大小写

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git commit -m README.md
On branch master
nothing to commit, working tree clean
```

忽视大小写

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git config core.ignorecase false
```

进行提交，但出现了一个问题

因为 mi-flex.html 有更新未跟踪，使用git commit -am可以省略使用git add命令将已跟踪文件放到暂存区的功能
即可提交成功

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git commit -m README.md
On branch master
Changes not staged for commit:
        modified:   README.md

no changes added to commit
```

使用git commit -am进行提交，即可提交成功

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git commit -am README.md
[master 458227c] README.md
 1 file changed, 1 insertion(+)
```

#### 6.提交到github

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git remote add origin https://github.com/moran-ai/test.git
```

git remote add origin https://github.com/moran-ai/test.git 这个链接每个仓库都不一样

#### 7.提交

在提交时出现了错误

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (master)
$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'https://github.com/moran-ai/test.git'
```

md文件中写入内容

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test
$ echo "#test" >> README.md
```

初始化

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test
$ git init
Initialized empty Git repository in D:/githubfile/git_test/.git/
```

添加md文件

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test (master)
$ git add README.md
warning: LF will be replaced by CRLF in README.md.
The file will have its original line endings in your working directory
```

提交

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test (master)
$ git commit -m "first commit"
[master (root-commit) b4fa78b] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

连接到远程仓库

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test (master)
$ git remote add origin git@github.com:tianqixin/runoob-git-test.git
```

提交，但出现了问题

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test (master)
$ git push -u origin master
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

# 解决：配置公钥

```shell
20622@DESKTOP-4PHULHH MINGW64 /D/githubfile/git_test (master)
$ cd ~/.ssh

20622@DESKTOP-4PHULHH MINGW64 ~/.ssh
$ ls
known_hosts

20622@DESKTOP-4PHULHH MINGW64 ~/.ssh
$ git config --global --list
user.name=moran-ai
user.email=58198222+moran-ai@users.noreply.github.com
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
http.sslverify=false
```

配置公钥的格式  多敲几次回车  会在C盘生成id_rsa.pub和id_rsa 两个文件

```shell
$ ssh-keygen -t rsa -C "自己的邮箱"
```

然后将id_rsa.pub的内容复制到github的公钥中去

进行测试，成功的结果如下

```shell
20622@DESKTOP-4PHULHH MINGW64 ~/.ssh
$ ssh -T git@github.com
Hi moran-ai! You've successfully authenticated, but GitHub does not provide shell access.
```

https://www.cnblogs.com/zhangguorenmi/p/13034989.html





# 错误解决

错误1：

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/git_test (main)
$ git push -u origin main
Logon failed, use ctrl+c to cancel basic credential prompt.
To https://github.com/moran-ai/test.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://moran-ai@github.com/moran-ai/test.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

解决： git pull --rebase origin master

https://blog.csdn.net/k_young1997/article/details/90489734



错误2：

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/git_test (master)
$ git push -u origin master
Logon failed, use ctrl+c to cancel basic credential prompt.
remote: Permission to moran-ai/test.git denied to moran-ai.
fatal: unable to access 'https://github.com/moran-ai/test.git/': The requested URL returned error: 403
```

解决：

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/git_test (master)
$ vim .Git/config
```

https://blog.51cto.com/chaohuang/1895880

### 错误3

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (main)
$ git push -u origin main
Logon failed, use ctrl+c to cancel basic credential prompt.
Everything up-to-date
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

解决：
 ① 输入git log 查看日志，会看到这样的一个序列号  commit后面是序列号

```shell
$ git log 
commit 8eeb457a55c0248d504bd1a3343174128592ecc1 (HEAD -> main, origin/main)
Author: moran-ai <58198222+moran-ai@users.noreply.github.com>
Date:   Tue Jun 1 10:33:17 2021 +0800

    first commit
```

②  输入命令：git reset --hard + 序列号

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (main)
$ git reset --hard 8eeb457a55c0248d504bd1a3343174128592ecc1
HEAD is now at 8eeb457 first commit
```

③ 输入命令： git add .

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (main)
$ git add .
```

④ 输入命令： git commit -m "five commit"  "" 中得内容是修改过后的信息

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (main)
$ git commit -m "five commit"
[main 80f77e4] five commit
 3 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 IMG_20201201_002733.jpg
 create mode 100644 IMG_20201201_002742.jpg
 create mode 100644 IMG_20201201_002904.jpg
```

⑤ 提交 输入命令：git push -f origin main   

```shell
20622@DESKTOP-4PHULHH MINGW64 /d/githubfile/demo (main)
$  git push -f origin main
Logon failed, use ctrl+c to cancel basic credential prompt.
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 1.30 MiB | 1003.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To https://github.com/moran-ai/curriculum.git
   8eeb457..80f77e4  main -> main
```

https://www.cnblogs.com/kkvt/p/11692025.html



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

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

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





# 提交

创建一个新的仓库  需要创建一个README.md的文件

```bash
echo "# test" >> README.md
git init
git add README.md 
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/moran-ai/test.git
git push -u origin main
```

从现有的仓库进行推送

```bash
git remote add origin https://github.com/moran-ai/test.git
git branch -M main
git push -u origin main
```



```bash
echo "# Flask" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/moran-ai/Flask.git
git push -u origin main
```

缓存超出范围错误

https://blog.csdn.net/qq_34466755/article/details/113748527

![image-20210630174326331](C:\Users\20622\AppData\Roaming\Typora\typora-user-images\image-20210630174326331.png)

# git操作---> after content update 

#### 远程更新本地仓库

```bash
git pull --rebase origin 分支名字
```

#### 使用SSH链接远程仓库

使用SSH之前，需要先配置好公钥，使用SSH在进行提交的时候，可以不用输入账号密码，可以直接进行上传

```bash
echo "# IT" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin git@github.com:moran-ai/IT.git
git push -u origin master
git remote add origin git@github.com:moran-ai/IT.git

已经存在的仓库
git remote add origin git@github.com:moran-ai/IT.git
git branch -M master
git push -u origin master
```

#### 使用HTTPS协议链接远程仓库

```bash
echo "# IT" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/moran-ai/IT.git
git push -u origin master


已经存在的仓库
git remote add origin https://github.com/moran-ai/IT.git
git branch -M master
git push -u origin master
```

#### 将远程内容下载到本地

git clone 仓库的地址

```bash
git clone git@github.com:moran-ai/IT.git
```

配置公钥

**①  cd ~/.ssh**
**②  ls**
**③  git config --global --list 		           查看自己的邮箱和密码**
**④  ssh-keygen -t rsa -C "自己的邮箱"    生成id_rsa.pub和id_rsa 两个文件**
**⑤  将id_rsa.pub的内容复制到github的公钥中去**
**⑥  ssh -T git@github.com   				测试**

#### 创建分支  

```bash
git branch 分支名字
```

#### 查看分支  带有*的是当前分支

```bash
git branch 
```

#### 切换分支

```bash
git checkout 分支名字
```

#### 提交内容到其他分支

```python
git push origin 分支名字
```

#### 查看是否与远程仓库进行了关联

```bash
git remote -v
```

#### 解除关联

```bash
git remote rm origin
```

#### 合并两个分支

注意：

​	① 合并两个分支，想要最终把内容保存到那个分支，就切换到那个分支，然后执行下面的命令

```bash
git merge master
```

② 分支冲突

![image-20210708155043309](D:\githubfile\Git\image-20210708155043309.png)

 冲突产生的原因:如果两个分支之间的版本差距过大，就会产生冲突，版本只能有1个版本的差距

解决方案：手动进行解决，进入产生冲突的文件，然后删除=====>或者======head这样的数据，只要保留想要的数据，然后重新进行上传，即可解决

#### 删除某个分支

```bash
git branch -D 分支名 
```

#### 以图形化的形式展示日志

```bash
git log --graph --decorate --oneline --all
```

#### 查看日志

```bash
git log/ git reflog
```

#### 文件的回退与撤销

##### ① 回退

HEAD 表示当前版本的最新版本

HEAD^ 表示最新版本的上一个版本

HEAD^^ 表示最新版本的上两个版本  依次类推

HEAD~1 表示最新版本的上一个版本

HEAD~2 表示最新版本的上两个版本  依次类推

 回退命令1

```
git reset --hard HEAD
```

 回退命令2

```
git reflog  # 查询版本号
git reset --hard 版本号
```

##### ②撤销

**撤销工作区代码[没有添加到缓存区的代码] 只能撤回工作区，暂存区的代码，不能撤销缓存区的代码**

使用git status查看，如果有红色的文件，则代表是在工作区

```
git checkout 文件名
```

**撤销缓存区代码  将文件从缓存区撤销到工作区**  

使用git add 文件名 代表将文件存储到暂存区

```
git reset HEAD 文件名
```

#### 标签

##### ① 添加标签

```
git tag -a 标签名 -m '描述信息'
```

##### ② 查看标签

```
git tag
```

##### ③ 提交标签到远程仓库

```
git push origin 标签名
```

##### ④ 显示标签

```
git show 标签名
```

##### ⑤ 删除本地仓库标签

```
git tag -d 标签名
```

##### ⑥ 删除远程仓库标签

```
git push origin --delete tag 标签名
```

