# git使用踩坑与解决方法



#### 所有关于git的内容都可以参考

https://git-scm.com/book/zh/v2



#### <font color=FF0000>From 1.3</font>

##### config的三个作用域：local，global，system

**优先级：**<font color=FF0000>local > global > system</font>

**设置作用域：**

```bash
$ git config --local  # 缺省选择，只对某个仓库有效
$ git config --global  # 对当前用户所有账户有效
$ git config --system  # 对系统所有登录的用户有效
```

**清除作用域：**

```bash
$ git config --unset --local user.name
$ git config --unset --global user.name
$ git config --unset --system user.name
```

类似的，显示config的配置，--list可以添加上三个作用域，示例如下：

```bash
$ git config --local --list
$ git config --global --list
$ git config --system --list
```

其中，如果不指定作用域，将会显示所有的配置。

**<font color=FF0000>自行补充：</font>**

<mark>在配置`git config --global user.**`时出现将 `user.email `写成（配制成）`user.mail`，这时git并不会报错，且会记录到git list（可通过`git config --list`）查到</mark>

这时使用命令`git config --global --unset user.mail`即可将其删除（在`git config --list`中查不到）。



#### <font color=FF0000>From  1.4</font>

**建造Git仓库有两种场景**

- 把<font color=FF0000>已有</font>的项目代码纳入Git管理

  ```bash
  $ cd target_file_path
  $ git init git_proj_name
  ```

- <font color=FF0000>新建</font>的项目直接用Git管理

  ```bash
  $ cd target_file_path
  $ git init your_project #会在当前路路径下创建和项⽬目名称同名的⽂文件夹
  $ cd your_project
  ```



#### <font color=FF0000>From 1.5</font>

```bash
$ git add -u #对git已经跟踪了的文件，一起提交到暂存区
```



#### <font color=FF0000>From 1.6</font>

```bash
$ git add file_name  #将文件加入git管理
$ git rm file_name  #将文件从git管理中移除

$ git mv orgin_file_name target_file_name #将原始文件改名为目标文件名，类似于先mv再add 
```

**git还原**

```bash
$ git reset --hard  #暂存区工作路径下的所有变更都会清理掉，相当于还原
```



#### <font color=FF0000>From 1.7</font>

**`git log`命令**

```bash
$ git log --oneline  #以精简模式显示
$ git log -n4  #输出最近的4个commit（4可以改动）
```



```bash
$ git branch -v  #查看本地有多少分支 
```





#### <font color=FF0000>Win端下对已安装git进行版本升级</font>

2.17.1**之前**用

```bash
git update
```

2.17.1**之后**用

```bash
git update-git-for-windows
```

摘自：[Windows下如何对已安装的git进行版本升级？== eastegg的回答](https://segmentfault.com/q/1010000011704285)



#### <font color=FF0000>使用https url clone项目</font>

很多朋友在用github管理项目的时候，都是直接使用https url克隆到本地，当然也有有些人使用 SSH url 克隆到本地。然而，为什么绝大多数人会使用https url克隆呢？

这是因为，使用https url克隆对初学者来说会比较方便，复制https url 然后到 git Bash 里面直接用clone命令克隆到本地就好了。而使用 SSH url 克隆却需要在克隆之前先配置和添加好 SSH key 。

因此，如果你想要使用 SSH url 克隆的话，你必须是这个项目的拥有者。否则你是无法添加 SSH key 的。

#### <font color=FF0000>https和SSH clone的区别</font>

https可以随意克隆github上的项目，而不管是谁的；而SSH是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。

摘自：[github设置添加SSH](https://www.cnblogs.com/ayseeing/p/3572582.html)



#### <font color=FF0000>Github 上的账户的SSH keys与仓库的Deploy keys的区别</font>

- **Github账户的SSH keys**，相当于这个账号的最高级key，只要是这个账号有的权限（任何项目），都能进行操作。
- **仓库的Deploy keys**，顾名思义就是这个仓库的专有key，用这个key，只能操作这个项目，其他项目都没有权限。

说白了就相当于你有一所大别墅，<mark>SSH key能开别墅中的任何一个房间。而Deploy key只能开进别墅中的一个单间。</mark>

摘自：[github 上的账户的SSH keys与仓库的Deploy keys有何区别](https://segmentfault.com/q/1010000007356879)



#### <font color=FF0000>查看</font>

使用命令查看本地git安装地址

```bash
where git
```

摘自：[查看本地git安装位置](https://blog.csdn.net/weixin_42685022/article/details/84061043)



#### <font color=FF0000>问题汇总，出现如下问题：</font>

键入如下命令：

```bash
git clone git@github.com:***.git
```

出现：

```markdown
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

参考：[Permanently added the RSA host key for IP address '13.250.177.223' to t he list of known hosts.](https://blog.csdn.net/yushuangping/article/details/84240863) 或者见官方教程：[用户头像  ==>  Setting  ==>  SSH and GPG keys  ==>  generate a GPG key and add it to your account  ==>  Adding a new GPG key to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)

***

键入命令：

```shell
git push
```

出现：

```markdown
To github.com:PointerToNextPole/community.git
 ! [rejected]        master -> master (non-fast-forward)
error: 推送一些引用到 'github.com:PointerToNextPole/community.git' 失败
提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。
提示：再次推送前，先与远程变更合并（如 'git pull ...'）。详见
提示：'git push --help' 中的 'Note about fast-forwards' 小节。
```

解决方法：

```shell
git push -u origin +master
```

***



#### <font color=FF0000>用户名和邮箱</font>

**<font color=FF0000>查看</font>用户名和邮箱地址：**

```bash
# 设置用户名
$ git config user.name
# 设置用户邮箱
$ git config user.email
```

**<font color=FF0000>修改</font>用户名和邮箱地址：**

```bash
$  git config --global user.name  "xxxx"
$  git config --global user.email  "xxxx"
```

**如果在修改用户名和邮箱地址时出现如下情况：**

```
warning: user.email has multiple values
error: cannot overwrite multiple values with a single value
       Use a regexp, --add or --replace-all to change user.email.
```

**需要使用参数**`--replace-all`

```bash
$  git config --global --replace-all user.email "输入你的邮箱" 
$  git config --global --replace-all user.name "输入你的用户名"
```

**补充：查看git配置列表**

```bash
$  git config --list
```

参考：[Git修改和配置用户名和邮箱](https://www.cnblogs.com/sunshinekevin/p/11617547.html)



#### <font color=FF0000>Git 工作区、暂存区和版本库</font>

- **工作区：**就是你<mark>在电脑里能看到的目录</mark>。
- **暂存区：**英文叫stage, 或index。<mark>一般存放在 ".git目录下" 下的index文件（.git/index）中</mark>，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

![工作区、版本库中的暂存区和版本库之间的关系](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

摘自：[Git 工作区、暂存区和版本库](https://www.runoob.com/git/git-workspace-index-repo.html)



- `git log` 会按时间先后顺序列出所有的提交，最近的更新排在最上面。这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。
  - 其中一个比较有用的选项是 `-p` 或 `--patch` ，它会显示每次提交所引入的差异（按 **补丁** 的格式输出）。
  - 你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交
  -  你也可以为 `git log` 附带一系列的总结性选项。 比如你想看到每次提交的简略统计信息，可以使用 `--stat` 选项

- `git show` 此命令可以用于显示各种类型对象的相关信息



#### <font color=FF0000>Git设置添加一个新的远程 Git 仓库</font>

```shell
#Syntax
git remote add <shortname> <url>
#Example
git remote add origin git@github.com:PointerToNextPole/community.git
```





- 在提交（push）代码之后，若发现提交的代码存在问题，这时可以输入如下命令：

  ```shell
  git commit --amend --no-edit
  ```

  以修改提交

- 在提交（push）代码之后，若发现提交代码的注释（-m message）存在问题，这时可以输入如下命令：

  ```shell
  git commit --amend
  ```

  这时候会通过vim打开「提交」的相关信息（如下），只需要对其进行修改即可

  <img src="https://s1.ax1x.com/2020/06/26/NrRojI.png" style="zoom: 40%;" />

另外：在push代码时候可能需要使用如下命令，才能提交

```shell
git push -u orgin +master
```



#### git pull 和 git merge的区别

**git pull = git fetch + git merge**

- fetch和push命令可以分别对远程分支进行fetch和push操作

- pull不是直接跟远程分支对话的。

fetch同pull的区别在于：

- git fetch：是从远程获取最新版本到本地，不会自动merge
- git pull：是从远程获取最新版本并merge到本地仓库

从安全角度出发，git fetch比git pull更安全，因为我们可以先比较本地与远程的区别后，选择性的合并。
git push 默认推送到master。

摘自：[segmentfault Salamander的回答 -- git pull和git merge 区别? ](https://segmentfault.com/q/1010000009076820)



//TODO

```sh
# 创建一个新的分支branchName
git checkout -b branchName
```

```sh
#push到branchName的分支下
git push --set-upstream origin branchName
```



