# Git & Git 相关 CI/CD 笔记



## Git 学习笔记 & 使用踩坑解决方法



#### 一些资料

- Git 官方文档：https://git-scm.com/docs 
- 官方中文书籍：https://git-scm.com/book/zh/v2。
- [atlassian 公司出的 git 教程](https://www.atlassian.com/git/tutorials) 要比 Git 官方文档易读很多
- [猴子都能懂的GIT入门](https://backlog.com/git-tutorial/cn/) 适合入门，及建立知识框架
- [工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100) 画了一些动图，便于理解
- [图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html) 一些图解，说了一些上一篇文章没说的东西
- [这才是真正的GIT——GIT内部原理](https://www.lzane.com/tech/git-internal) 一共有三篇，这是第一篇。说明了一些 Git 原理性的东西，配合动图，清晰易懂
- [给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960) 一些补充的命令
- [GitHub - Docs](https://docs.github.com/cn) GitHub 官方文档（中文）



#### From 1.3

##### config 的三个作用域：local，global，system

**优先级：**<font color=FF0000>local > global > system</font>

**设置作用域：**

```bash
git config --local  # 缺省选择，只对某个仓库有效
git config --global  # 对当前用户所有账户有效
git config --system  # 对系统所有登录的用户有效
```

**清除作用域：**

```bash
git config --unset --local user.name
git config --unset --global user.name
git config --unset --system user.name
```

类似的，显示config的配置，--list可以添加上三个作用域，示例如下：

```bash
git config --local --list
git config --global --list
git config --system --list
```

其中，如果不指定作用域，将会显示所有的配置。

**自行补充：**

<mark>在配置`git config --global user.**`时出现将 `user.email `写成（配制成）`user.mail`，这时git并不会报错，且会记录到git list（可通过`git config --list`）查到</mark>

这时使用命令`git config --global --unset user.mail`即可将其删除（在 `git config --list`中查不到）。

#### From  1.4

**建造Git仓库有两种场景**

- 把<font color=FF0000>已有</font>的项目代码纳入 Git 管理
  
  ```bash
  cd target_file_path
  git init git_proj_name
  ```

- <font color=FF0000>新建</font>的项目直接用 Git 管理
  
  ```bash
  cd target_file_path
  git init your_project # 会在当前路路径下创建和项⽬目名称同名的⽂文件夹
  cd your_project
  ```

**补充：**

`git init --bare <directory>`：在指定目录创建一个空的 Git 仓库。运行这个命令会创建一个名为 directory，只包含 .git 子目录的空目录。

补充内容摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)

#### From 1.5

```bash
git add -u # 对git已经跟踪了的文件，将其一起提交到暂存区；另外，对于新添加的文件，将不会被添加
```

另外，本讲 讲了“Git 的工作区、暂存区和版本库”；由于第一次没有记录，第二次重学时，下面已经记录了，这里略。

#### From 1.6

```bash
git add file_name  # 将文件加入git管理
git rm file_name  # 将文件从git管理中移除( remove tracked file from the git index)，也可同时从“工作区”和“暂存区”移除。

git mv orgin_file_name target_file_name # 使用 git 将原始文件改名为目标文件名，类似于先 mv 再 git add。另外，git mv 也同样，可以起到移动文件的目的。
# 还有 git mv -f origin_file existing_file 命令：强制重命名或移动，这个文件已经存在，将要覆盖掉
```

**补充：**关于 **git rm**：

> **summary:** The git rm command is used to remove files from a Git repository.
>
> The primary function of `git rm` is to remove tracked files from the Git index. Additionally, `git rm` can be used to remove files from both the staging index and the working directory 即：git rm 可以从「工作区」和「缓存区」中移除文件
>
> There is no option to remove a file from only the working directory. The files being operated on must be identical to the files in the current `HEAD`  即：git rm 无法只移除工作区的文件（可用 rm 命令实现），同时，git rm 只能删除 当前 HEAD 的文件
>
> 详见：[Atlassian git tutorials - git rm](https://www.atlassian.com/git/tutorials/undoing-changes/git-rm)

**git还原**

```bash
git reset --hard  # 暂存区工作路径下的所有变更都会清理掉，相当于还原；这个命令是相当危险的。
```

#### From 1.7

**git log 命令**

```bash
git log --oneline  # 以精简模式显示
git log -n4  # 查看最近的几个commit，这里是4
```

为了更好的讲解 git log，这里创建下分支：

```bash
git branch -v # 查看各个分支，以及最后提交的信息。另外，-a 选项时查看本地和远端有多少分支(-a 等价于 --all)；同时，这两个选项可以连起来写，即 -av
git checkout -b branchName commitHash # 创建一个新的分支，并 **切换到该新创建的分支上**。其中：branchName 为分支名，commitHash 为提交的SHA hash（即：Secure Hash Algorithm hash）；表示根据「哪一个提交」创建一个分支。另外，commitHash 可以通过 git log 查到。
# 上面 hash 相关的内容了解自：careerkarma.com/blog/git-log
```

可以使用 git log branchName，查看某一个分支的日志。同样，也可以使用 git log --all 命令，查看所有分支的信日志。如下所示，输入 git log --all，会输出所有的（共两个）分支，master 和 testBranch：

<img src="https://s2.loli.net/2022/02/19/DaPWZ8uFi7B5voy.png" alt="image-20220219222624280" style="zoom:50%;" />

另外，由于上面的 git log --all 在很多分支的情况下，看起来很累，可以加上 --graph 选项，使得以图形化的方式展现；可以看出分支之间的关系：

<img src="https://s2.loli.net/2022/02/19/OHIW6UoYeGTDabV.png" alt="image-20220219223318357" style="zoom:50%;" />

补充一下，删除分支命令：`git branch -d branchName`，注意：branchName 要完整，否则不行；另外，无法在某个分支下，删除当前分支。

#### From 1.8

讲解 gitk 的内容，略。

#### From 1.9

.git 文件夹中的内容包含：

<img src="https://s2.loli.net/2022/02/19/WVTbQfr8nypC9ig.png" alt="image-20220219234827964" style="zoom:50%;" />

- **HEAD 文件：**cat 一下，会发现打印的是：

  ```
  ref: refs/heads/testBranch
  ```

  它当前指向的分支。即：<font color=FF0000 size=4>**HEAD 文件记录的是：当前工作在哪一个分支上**</font>。

  另外，这里 refs/heads/testBranch 在 .git 文件夹中确实存在该文件。具体关于 refs 文件夹，下面有说。

- **config 文件：**存放配置信息。也就是 `git config` 命令输出相关的内容（比如 `git config user.name`），下图是输出 `cat config` 的结果：

  <img src="https://s2.loli.net/2022/02/20/bfuyoDw4k8izSvM.png" alt="image-20220220133215591" style="zoom:50%;" />

  输入 `git config core.repositoryFormatVersion` 即可显示：0；类似的：在 `git config core.repositoryFormatVersion` 后面再加一个参数，作为repositoryFormatVersion 的值，即可修改 config 文件中 repositoryFormatVersion 对应的内容。

- **ref 文件夹：**包含 heads 和 tags 两个文件夹

  - **heads 中存放的是分支，分支意味着 <font color=FF0000 size=4>独立的开发空间</font>**

    cat 一下 heads 文件夹中的 “分支文件”，比如 master，会发现：打印出 hash 值，这个 hash 值是 commit 的 id 号。

    在 heads 文件夹中，输入如下命令，可以知道 hash 值的类型是 commit

    ```sh
    git cat-file -t hashVal # commit
    ```

  - **tags 中存放的是类似于“里程碑”的东西，比如 "v1.1版本"的标签**

    cat 一下 tags 中的文件（应该可以成为 tag），打印的结果也是 hash 值，即：commit 的 id号。

    类似的，运行 `git cat-file -t hashVal` 输出的结果为 tag。

    运行命令 `git cat-file -p hashVal` 可以打印出对应的内容（-p 选项就是查看对应内容的），这些信息包含 tag 的指向（指向的是 object）。

    // 这里图床丢了一张图片...

    对这个对象运行 `git cat-file -t 198010a`（198010a 是上面 object 对应的 hash 值），可以发现他是一个 commit

- **objects 文件夹：**objects 文件夹是 git 中非常重要的文件夹，是 git 文件系统核心的内容

  <img src="https://s2.loli.net/2022/02/20/MumQfHXoO8svWRe.png" alt="image-20220220163534510" style="zoom:50%;" />

  objects文件夹 包含大量的 两位 16进制命名的文件夹，也有 info、pack 文件夹。pack 文件夹会做一个自我梳理的过程，如果 objects 文件夹中 16进制文件夹很多时，git 会将其打包，打包的文件放在 pack 文件夹中。

  先进入 16进制的文件夹，比如上面的 19，其中有以 hash 命名的文件。这里 hash 文件，完整的 hash 值应该是 “19 + hash 值”。这说明：objects 文件夹下的 “两位 16进制命名的文件夹”只是对 这些杂乱的 hash 文件做了一下分类，并把用于分类的“两位16进制”从文件名中删去了。所以在输入 hash值时，要补上

  输入命令：`git cat-file -t 19+hashVal`，打印出：tree。输入 `git cat-file -p 19+hashVal` 查看内容：

  <img src="https://s2.loli.net/2022/02/20/Ssu8rvVwWFDmiPt.png" alt="image-20220220170918498" style="zoom:50%;" />

  另外，对 5efb9bc29c4 进行 `git cat-file -t` 操作，打印出它的类型是 tree；再对它进行 `git cat-file -p` 操作：

  <img src="https://s2.loli.net/2022/02/20/EPvgTcuMs8YVQwZ.png" alt="image-20220220171941181" style="zoom:50%;" />

  它是一个 blob，并且对应的文件是 test.txt

由上面总结，git 中有三种类型（通过 `git cat-file -t` 显示出来），为：commit、tree、blob。

另外，上面没搞懂为什么一会儿用 `git cat-file -t`，一会儿用 `git cat-file -p` 很懵，看到下面“commit、tree、blob 三者之间的关系” 就会明白。

**补充：** git cat-file 还有 -s 选项，表示显示 对象 (\<object>) 的大小。其他选项和参数详见：[Pro Git - git cat-file - options](https://git-scm.com/docs/git-cat-file#_options)

#### From 1.10

commit、tree、blob 三者（注：这三者都是 git 中的对象(object) ）之间的关系

![image-20220220174718938](https://s2.loli.net/2022/02/20/TkZIqPmWVj5lv2o.png)

- **commit**

  每一次执行 `git commit` 操作都会创建一个 commit 对象。

  一个 commit 对应一棵树 tree（如图上 commit 部分（蓝色部分）tree 键值对所示），这棵树代表了：当前 commit 的视图，视图中存放了一个快照，快照中存放了当前 commit 对应的本项目仓库的所有文件的快照。即：在每次提交中，这个项目长什么样就是通过这棵树展示出来

- **tree**

  tree 就是文件夹，比如上图 tree 的部分（黄色部分），tree 中也可以包含 tree（即：子文件夹），如 tree 中的 images 文件夹上，键值对 就是显示的类型为 tree；同时，下面也展示了 images 文件夹的 tree。

- **blob**

  blob 与文件名没有关系，只要文件内容是一样的，则 git 会认为这是同一个文件；git 也只会存一份，可以大大节约存储空间。

这里说明了 commit、tree、blob 之间的关系，至于上面 使用 `git cat-file -t` 和 `git cat-file -p` 用的有点懵，对文件结构不了解；看完了这个，清楚了很多。

**补充：**

这里 commit、tree、blob 三者的关系，在视频 [[中文] 这才是真正的 Git——Git 内部原理揭秘！（freeCodeConf 2019 深圳站）](https://www.bilibili.com/video/av77252063) 的开始，也有讲解。下图是是其中的 ppt，同样说明了关系：

<img src="https://s2.loli.net/2022/02/24/sTJUXl4vY3NQ2dt.png" alt="image-20220224232018021" style="zoom: 25%;" />

##### Git 是怎么储存信息的

这里会用一个简单的例子让大家直观感受一下git是怎么储存信息的。

首先我们先创建两个文件

```shell
git init
echo '111' > a.txt
echo '222' > b.txt
git add *.txt
```

Git会将整个数据库储存在`.git/`目录下，如果你此时去查看`.git/objects`目录，你会发现仓库里面多了两个object。

```shell
$ tree .git/objects
.git/objects
├── 58
│   └── c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
├── c2
│   └── 00906efd24ec5e783bee7f23b5d7c941b0c12c
├── info
└── pack
```

好奇的我们来看一下里面存的是什么东西

```sh
cat .git/objects/58/c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c
xKOR0a044K%
```

怎么是一串乱码？这是因为Git将信息压缩成二进制文件。但是不用担心，因为Git也提供了一个能够帮助你探索它的api `git cat-file [-t] [-p]`， `-t`可以查看object的类型，`-p`可以查看object储存的具体内容。

```shell
$ git cat-file -t 58c9
blob
$ git cat-file -p 58c9
111
```

可以发现这个 object 是一个 blob 类型的节点，他的内容是111，也就是说这个 object 储存着 a.txt 文件的内容。

这里我们遇到第一种Git object，blob类型，它只储存的是一个文件的内容，不包括文件名等其他信息。然后将这些信息经过SHA1哈希算法得到对应的哈希值 58c9bdf9d017fcd178dc8c073cbfcbb7ff240d6c，作为这个object在Git仓库中的唯一身份证。

也就是说，我们此时的Git仓库是这样子的：

<img src="https://www.lzane.com/tech/git-internal/p1s1.png" alt="https://www.lzane.com/tech/git-internal/p1s1.png" style="zoom:80%;" />

我们继续探索，我们创建一个commit。

```shell
$ git commit -am '[+] init'
$ tree .git/objects
.git/objects
├── 0c
│   └── 96bfc59d0f02317d002ebbf8318f46c7e47ab2
├── 4c
│   └── aaa1a9ae0b274fba9e3675f9ef071616e5b209
...
```

我们会发现当我们 commit 完成之后，Git 仓库里面多出来两个object。同样使用`cat-file`命令，我们看看它们分别是什么类型以及具体的内容是什么。

```sh
$ git cat-file -t 4caaa1
tree
$ git cat-file -p 4caaa1
100644 blob 58c9bdf9d017fcd178dc8c0... 	a.txt
100644 blob c200906efd24ec5e783bee7...	b.txt
```

这里我们遇到了第二种 Git object 类型——tree，它将当前的目录结构打了一个快照。从它储存的内容来看可以发现它储存了一个目录结构（类似于文件夹），以及每一个文件（或者子文件夹）的权限、类型、对应的身份证（SHA1值）、以及文件名。

此时的Git仓库是这样的：

<img src="https://s2.loli.net/2022/02/25/dV3PM69ylRZriWL.png" alt="https://www.lzane.com/tech/git-internal/p1s2.png" style="zoom:80%;" />

```sh
$ git cat-file -t 0c96bf
commit
$ git cat-file -p 0c96bf
tree 4caaa1a9ae0b274fba9e3675f9ef071616e5b209
author lzane 李泽帆  1573302343 +0800
committer lzane 李泽帆  1573302343 +0800
[+] init
```

接着我们发现了第三种 Git object 类型——commit（**注：**commit 对象也是由 commit 命令创建的；不过是先创建 tree 对象，然后再创建 commit 对象 ），它储存的是一个提交的信息，包括对应目录结构的快照tree的哈希值，上一个提交的哈希值（这里由于是第一个提交，所以没有父节点。在一个merge提交中还会出现多个父节点），提交的作者以及提交的具体时间，最后是该提交的信息。

此时我们去看Git仓库是这样的：

<img src="https://s2.loli.net/2022/02/25/cxpkSemdHl2JnFD.png" alt="img" style="zoom:80%;" />

到这里我们就知道Git是怎么储存一个提交的信息的了，那有同学就会问，我们平常接触的分支信息储存在哪里呢？

```sh
$ cat .git/HEAD
ref: refs/heads/master
$ cat .git/refs/heads/master
0c96bfc59d0f02317d002ebbf8318f46c7e47ab2
```

在Git仓库里面，HEAD、分支、普通的Tag可以简单的理解成是一个指针，指向对应commit的SHA1值。

<img src="https://s2.loli.net/2022/02/25/PcZYAbotzXFW8EI.png" alt="img" style="zoom: 80%;" />

其实还有第四种Git object，类型是tag，在添加含附注的tag（`git tag -a`）的时候会新建，这里不详细介绍，有兴趣的朋友按照上文中的方法可以深入探究。

至此我们知道了Git是什么储存一个文件的内容、目录结构、commit信息和分支的。**其本质上是一个key-value的数据库加上默克尔树形成的有向无环图（DAG）**。这里可以蹭一下区块链的热度，区块链的数据结构也使用了默克尔树。

另外，commit、tree、blob 这三者 都是一创建就不可变更的 (immutable)；refs 是可以变更的，相当于一个指针。

以上内容摘自：[这才是真正的GIT——GIT内部原理](https://www.lzane.com/tech/git-internal) 演讲视频：[[中文] 这才是真正的 Git——Git 内部原理揭秘！（freeCodeConf 2019 深圳站）](https://www.bilibili.com/video/av77252063) 

#### From 1.11

根据实验表明：commit、tree、blob 三者都会被加入到 objects 文件夹下。如下文件结构

<img src="https://s2.loli.net/2022/02/20/6DAd1YGfOB9vJ7H.png" alt="image-20220220184824301" style="zoom: 33%;" />

通过如下命令 `find .git/objects -type f` ，打印出的结果为：

<img src="https://s2.loli.net/2022/02/20/5IYaEUyPr7lNDSt.png" alt="image-20220220185214559" style="zoom:50%;" />

即上面的 一个 commit，两个 tree，一个 blob，共四个文件。

#### From 1.12

分离头指针 (detached HEAD) 表示，当前工作在没有分支的状态下。

可以使用 `git checkout commitHash` 创建一个“分离头指针”，创建时，会出现下面的提示；提示说明了创建“分离头指针”的注意事项。

> Note: switching to '198910a3b2d'.
>
> <mark>You are in 'detached HEAD' state</mark>. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by switching back to a branch.
>
> If you want to create a new branch to retain commits you create, you may do so (now or later) by using -c with the switch command. Example:
>
>   git switch -c \<new-branch-name>
>
> Or undo this operation with:
>
>   git switch -
>
> Turn off this advice by setting config variable advice.detachedHead to false
>
> HEAD is now at 198910a add test.txt

**分离头指针有优点也有缺点：**

- **缺点：**分离头指针的改动不会被保存，一旦切换到其他分支上，做的改动将可能被 git 当作垃圾清理掉。所以，如果想要做变更，建议与某一个分支相关联；对于分支，git 是不可能将其清理掉的。
- **优点：**如果你想做一点试验性的、不想保存的变更，这是只需要切换分支，这些“分离头指针”上的变更就会自动被清理，不需要做处理。另外，由下面（2.3）可知，变基需要「分离头指针」实现。

在 commit 之后退出 分离头指针时，git 会做如下提示：

<img src="https://s2.loli.net/2022/02/20/oCgbVur1HTXO8Wl.png" alt="image-20220220195359974" style="zoom:50%;" />

提示中也提及了，保存分离头指针为一个分支的方法：`git branch branchName commitHash`

#### From 1.12

`git diff commitHash1 comitHash2`：比较两个 commit 之间的区别。

另外，这里的 commitHash 可以用 HEAD 进行指代，同时也可以用 HEAD 进行其他指代，比如：HEAD\^ 表示HEAD 的父亲，同理：HEAD\^\^1 可以表示为 HEAD 父亲的父亲  

**补充：** ^ / ~ 的用法

- `^`：表示第几个父/母亲 ——　git存在多个分支合并的情况，所以不只有1对父母亲
- `~`：表示向上找第几代，相当于连续几个 `^`

**注：**这里稍微有点不理解，但是根据例子还是有规律可循的：默认垂直的那个分支是被优先选择的（即：^ / ^1 的情况）；^2 表示没有选择垂直的分支的另一个分支。~ 直接就是去垂直的那一个分支了。

学习自：[git 中 HEAD^ 和 HEAD~ 是啥](https://www.jianshu.com/p/624bf81a3290)

#### From 2.1

删除分支，使用 `git branch -d branchName`。

**补充：git branch 的 -d 和 -D 选项 的区别：**

- **git branch -d：**会<font color=FF0000>在删除前检查 merge 状态</font>（其与上游分支 或者 （其）与head）。
- **git branch -D：**是 `git branch --delete --force` 的简写，它<font color=FF0000>会直接删除</font>（**注：**即，不会检查 merge 状态）。

摘自：[删除分支 git branch -d与git branch -D的区别](https://blog.csdn.net/qq_33592641/article/details/103871482)

#### From 2.2

如果发现 <font color=FF0000 size=4>**最近一次**</font> `git commit -m 'commit msg'` 中的内容存在问题，需要修改；则可以使用 `git commit --amend` 进行变更。另外，下面也有讲解。

#### From 2.3

对之前的（不仅仅是第一次） `git commit -m msg` 的msg 进行变更。需要使用「变基」，`git rebase` ，如下：

```sh
git rebase -i targetCommitHash # -i 表示交互式的，另外，需要注意的是：targetCommitHash 需要是 被修改msg 的 commit 的前面的 commit 的 hash
```

交互结果如下：

<img src="https://s2.loli.net/2022/02/20/Yn7Slg51w69AIc2.png" alt="image-20220220235258909" style="zoom:50%;" />

其中 commands 说明了模式，如 msg "master commit"，左边的 pick，则是 p, pick。但是，如果要修改 msg，则需要将 pick 模式，修改为新的模式：reword / r，（改为 reword 和 r 均可）即：

```
reword adb8494 master commit with rebase and change msg
```

按下 `:wq`，这时候会弹出新的交互页面，如下：

<img src="https://s2.loli.net/2022/02/20/Qq59AuJIyHCnVUO.png" alt="image-20220220235850827" style="zoom:50%;" />

再次修改首行的 msg 即可。修改完成后，输入 git log，就可以知道发现 msg 已经修改成功。

另外，<font color=FF0000 size=4>**变基的行为只在自己的分支上做变更，没有上传代码到版本库**</font>；如果已经被上传到集成分支上，则不能随意的变基，这样会影响到合作者的开发。

#### From 2.4

把 <font color=FF0000 size=4>**连续的多个**</font> commit 整理成一个，还是需要使用 rebase 命令，但是需要输入的 commitHash 是 连续多个 commit （这个整体）的前面一个 commitHash

输入命令 `git rebase -i prevCommitHash`，这时候会显示出 prevCommitHash 的 commit，后面所有的 commit；显示出的内容，格式和上面（From 2.3的图示）的一样。

如下示例，将 3ab109b 和 3903518 两个 commit 合并：

![image-20220221142034021](https://s2.loli.net/2022/02/21/xbQ86ePrRtJi5T4.png)

上面的打印，显示出多个 commit（其中包含待合并的多个 commit），要选择其中一个作为合并后的「基准」commit，它的 command 不做修改，依然是 pick；剩下的 command 修改为 squash。

在上面的示例中，将 3ab109b 设置为 基准 commit，command 依然为 pick；3903518 的 command 设置为 squash（如果有更多需要被修改的，依次类推，command 修改为 squash）：

![image-20220221142343821](https://s2.loli.net/2022/02/21/KA4eU5YsfGwLcT7.png)

normal 模式下，键入 `:wq`，以保存；这时候会显示：

![image-20220221142634014](https://s2.loli.net/2022/02/21/b3XN1ceJyFvMZPj.png)

在第二行加上 一个新的 commit msg，作为合并后的 commit msg；如下（红框中的是添加的 commit msg，其他是删除空行后的文字）：

![image-20220221142842642](https://s2.loli.net/2022/02/21/2EfR3NAXGKBL8St.png)

继续进入 normal 模式，键入 `:wq` ，以保存；会完全退出 `git rebase -i`，会有如下显示：

![image-20220221143158727](https://s2.loli.net/2022/02/21/BldUZYp25nJPXE3.png)

这时候进行检查：`git log`，效果图如下；会发现 3ab109b 和 3903518 被合并成了 2ba50c92（这个 commit 是新生成的）；另外，新的 commit msg 成功显示，合并之前的 commit msg 也被成功保留：

![image-20220221143337478](https://s2.loli.net/2022/02/21/KYFqk9elCxmWunX.png)

**补充：**

如果要合并的是最近的几个commit，可以用`git reset --soft HEAD~n && git commit -m <msg>` 来实现

补充内容摘自：[这才是真正的GIT——GIT实用技巧](https://www.lzane.com/tech/git-tips)

#### From 2.5

<font color=FF0000 size=4>**把间隔（即：不连续）的几个 commit 整理成一个**</font>，依然使用 `git rebase -i  `，如下示例：

打印 `git log`，有四个 commit：

![image-20220221161420466](https://s2.loli.net/2022/02/21/3a8BJPfORTQtcEN.png)

准备将 c4110f0 和 e6df709 合并（即：最上面 和 最下面 两个），输入命令 `git rebase -i 2d154a4`

![image-20220221161619294](https://s2.loli.net/2022/02/21/wiV7KUuqtZzFxH4.png)

上面打印出来 e6df709、b1bd23e 和 c4110f0 三个 commitHash，需要添加 96d3967 这个 commitHash；同时，<font color=FF0000 size=4>**要把 c4110f0 这条记录和 e6df709 放在一起**</font>（具体参考下图），并修改 c4110f0 的 command 为 squash

![image-20220221161800773](https://s2.loli.net/2022/02/21/8IT9RqweCDAMUcm.png)

normal 模式键入 `:wq!`，显示如下：

![image-20220221162023462](https://s2.loli.net/2022/02/21/yPrR6WE5nmIq8Fo.png)

在第二行添加 commit msg（下图红框）

![image-20220221162905045](https://s2.loli.net/2022/02/21/3V1wl6BLyIfFGrZ.png)

在 normal 模式键入 `:wq!`，退出 `git rebase`，有如下显示：

![image-20220221163115191](https://s2.loli.net/2022/02/21/lBS1VcrLH5vNWQm.png)

再输入 `git log --graph`，显示日志，结果说明：成功合并

![image-20220221163228987](https://s2.loli.net/2022/02/21/H9ZxRkf51r6BTe7.png)

<font size=4>**补充，也类似于上面 `git rebase -i` 的总结：**</font>

使用 `git rebase -i` 可以 <font color=FF000 size=4>**改写、替换、删除 或 合并 提交 ⭐️⭐️⭐️**</font>。

![改写提交](https://s2.loli.net/2022/02/22/2diIRUCJHha5KOc.png)

![合并提交](https://s2.loli.net/2022/02/22/CJ9AtFqvQHB17cZ.png)

**主要使用的场合：**

- 在 push 之前，重新输入正确的提交注解（注：修改之前的任意多条）
- 清楚地汇合内容含义相同的提交（注：合并多条提交）
- 添加最近提交时漏掉的档案（注：这条《玩转 Git 三剑客》中没有）

「补充内容」摘自：[猴子都能懂的GIT入门 - 改写提交的历史记录](https://backlog.com/git-tutorial/cn/stepup/stepup6_5.html)

#### From 2.6

<font color=FF0000 size=4>**查看 “缓存区”和 当前 head 文件之间的差异**</font>，使用命令 `git diff --cached`

如下图：test3 原本是 "at test3"；现在，在新的一行添加了 “before git diff --cached”。这时候，打印如下：

![image-20220221165300079](https://s2.loli.net/2022/02/21/36sZIHrT8jORlxk.png)

其中 “before git diff --cached” 左侧 “+” 的就是表示添加（不仅仅是新增，也可以是修改中的新增）。另外，“-” 表示被修改删除的部分（不仅仅是删除，也可以是修改中的删除）。

**注：**markdown 中可以使用 ```diff，来表示文件变更（写法参见：[markdown的diff效果](https://cloud.tencent.com/developer/article/1745311)）

#### From 2.7

<font color=FF0000 size=4>**比较工作区和暂存区所含文件的差异**</font>，使用 `git diff` 即可（这是所有文件的差异）。

如果只对其中一个文件进行比较，则：`git diff -- fileName.ext [fileName2.ext ...]`

#### From 2.8

<font color=FF0000 size=4>**让 暂存区（所有的文件）恢复成和 HEAD 的一样**</font>，可以使用 `git reset HEAD` 。检验 reset 是否成功，可以通过：上面的 `git diff --cached` 来查看，如果为空，则成功

另外，一般让 暂存区的 部分文件 恢复成和 HEAD 一样，可以添加参数 `git reset HEAD <file>...`

课程在这里提了一下 `git stash` 命令，作用是：将修改的内容保存至<font color=FF0000>**堆栈区**</font>，以方便你临时切出去修改其他东西之后再回来，修改内容依然存在。另外，下面的 2.14 有更多讲解。

#### From 2.9

<font color=FF0000 size=4>**让工作区的文件恢复为和暂存区一样**</font>，则使用 `git checkout -- <file>...`，在执行之后，可以通过 `git diff <file>...` 来查看是否恢复成功

**总结：**

- 取消工作区的变更，则执行 `git checkout -- <file>...`
- 取消暂存区的变更，则执行 `git reset`

#### From 2.10

<font color=FF0000 size=4>取消暂存区 **部分文件** 的更改</font>，即：暂存区的部分文件恢复和 HEAD 一样。则可以使用上面提及的 `git reset HEAD -- <file>...`

#### From 2.11

<font color=FF0000 size=4>**消除最近的几次提交**</font>，可以使用 `git reset --hard commitHash`，这个命令非常危险：HEAD 将会变化，暂存区和工作区的内容都将恢复为 commitHash 所对应的 内容，即内容丢失；所以，慎用。

如下示例，输入 `git log`，显示如下：

<img src="https://s2.loli.net/2022/02/21/zkrUhnJmRdfvuA3.png" alt="image-20220221200134503" style="zoom:50%;" />

想要消除最上面的两个 commit，可以输入命令：`git reset --hard b23cc36`。这时候，再输入 `git log`，结果如下：

<img src="https://s2.loli.net/2022/02/21/JjhUgdbTOi17Moe.png" alt="image-20220221200407603" style="zoom:50%;" />

#### From 2.12

<font color=FF0000 size=4>查看不同提交的指定文件的差异</font>，比如对比两个分支之间的区别，可以使用 `git diff branch1 branch2` 来查看；这个命令是展示全部文件的，可能会比较多。如果想要展示部分文件之间的差异，可以加上 file 参数，即：`git diff branch1 branch2 -- <file>`。补充，这里  branch 就是一个指针，指向 当前分支最近的一个 commit

注意，这里除了可以使用分支之外，还可以使用 分支所在的 commit 进行比较；即：`git diff commitHash1 commitHash2 --<file>`

#### From 2.13

<font color=FF0000 size=4>使用 git 删除文件</font>，可以使用 `git rm <file>` 命令，让该文件同时在 工作区 和暂存区被删除。如果直接使用 `rm <file>`，则 只是在 工作区中被删除，而缓存区没有被删除。

#### From 2.14

<font color=FF0000 size=4>开发中临时加塞了紧急任务，该如何处理？</font>即，对代码做了修改；但是突然来的任务不需要这里的修改，需要切出去；这时候需要使用 `git stash` 命令，将修改的内容存储到堆栈区。在使用 `git stash` 命令之后，可以使用 `git stash list` 查看 stash 列表。如下：

<img src="https://s2.loli.net/2022/02/21/Ohz13RoFg9fqUVs.png" alt="image-20220221210139366" style="zoom:50%;" />

在处理完任务后，可以使用 `git stash pop` 或者 `git stash apply` 命令进行恢复 stash（注，补充：暂存的内容可以恢复到其他任意指定的分支上）。相关命令如下：

- **git stash apply：**环境将会恢复，stash 中的数据，将不会被清除。

  可以使用 `git stash apply <stashName>`（如stash@{1}）指定恢复哪个stash到当前的工作目录

- **git stash pop：**将当前stash中的内容弹出，并应用到当前分支对应的工作目录上。环境会恢复，stash 中的数据将会被清理。

  如果从stash中恢复的内容和当前目录中的内容发生了冲突，也就是说，恢复的内容和当前目录修改了同一行的数据，那么会提示报错，需要解决冲突，可以通过创建新的分支来解决冲突。

- **git stash save \<msg> ：**作用等同于git stash，区别是可以加一些注释

- **git stash list：**查看当前stash中的内容

- **git stash drop [stashName]：**从堆栈中移除某个指定的stash，stashName 即：类似 stash@{1}；如果不加上 stashName，则默认为：最近的一次stash

- **git stash clear：**清除堆栈中的所有内容

- **git stash show：**查看堆栈中最新保存的stash和当前目录的差异

  可以通过 `git stash show <stashName>` 的方式，指定某一个 stash 与当前目录的差异

  通过 `git stash show [<stashName>] -p` 查看详细的不同

- **git stash branch：**从最新的stash创建分支。

  应用场景：当储藏了部分工作，暂时不去理会，继续在当前分支进行开发，后续想将stash中的内容恢复到当前工作目录时，如果是针对同一个文件的修改（即便不是同行数据），那么可能会发生冲突，恢复失败，这里通过创建新的分支来解决。可以用于解决stash中的内容和当前目录的内容发生冲突的情景。
  发生冲突时，需手动解决冲突。
  
  「命令相关内容」摘自：[git stash详解](https://blog.csdn.net/stone_yw/article/details/80795669)
  
- **git stash push：**在看[[中文] 这才是真正的 Git——Git 内部原理揭秘！（freeCodeConf 2019 深圳站）](https://www.bilibili.com/video/av77252063) 时候，发现这样一条命令：git stash push [-u | --include-untracked]，可以让你获得一个干净的工作区间。



#### From 2.15

<font color=FF0000 size=4>使用 .gitignore文件</font>，一般格式为：

- **[*]\<file>[.ext]：**忽略的文件，其中 [*] 表示：可选的文件名通配符；[.ext] 表示：可选的文件扩展名
- ***foder/：**忽略文件夹

另外，在 实际使用 git 时，发现有些想要忽略文件的文件，已经被添加到暂存区了；这时候就需要从暂存区中删除（而不从项目目录中删除），就需要使用 `git rm --cached fileName` 以把文件从暂存区中删除。

#### From 2.16

<font color=FF0000 size=4>Git 的备份</font>

**Git 常用的传输协议：**

| 常⽤协议         | 语法格式                                                     | 说明                      |
| ---------------- | ------------------------------------------------------------ | ------------------------- |
| 本地协议（1）    | /path/to/repo.git                                            | 哑协议                    |
| 本地协议（2）    | ﬁle:///path/to/repo.git                                      | 智能协议                  |
| http / https协议 | http://git-server.com:port/path/to/repo.git<br>https://git-server.com:port/path/to/repo.git | 平时接触到的 都是智能协议 |
| ssh协议          | user@git-server.com:path/to/repo.git                         | ⼯作中最常⽤ 的智能协议   |

**哑协议与智能协议之间的区别**

- 直观区别：哑协议传输进度不可⻅见，智能协议传输可见。
- 传输速度：智能协议⽐哑协议传输速度快。

所以更推荐智能协议

在使用 `git clone` 命令时，可以加上 `--bare` 选项，表示clone的仓库不带工作区；以后在 push 时可以方便些

这里还提及了 `git remote` 相关的 命令，是将本地 git 和 远端的 git 仓库建立联系的命令。其中至关重要是的是 `git remote add remoteName remoteUrl`（在 GitHub 上新建立一个项目，可以不使用 git clone，而是使用 git remote add + git fetch + git merge ( git fetch + git merge == git pull )）。其他相关的命令，下面有说，这里略

#### From 4

- 不同人修改了不同文件如何处理？                 Git 会自动合并

- 不同人修改了同文件的不同区域如何处理？  Git 会自动合并

- 不同人修改了同文件的同一区域如何处理？  Git 不会自动合并，需要开发者自行协商

- 同时变更了文件名和文件内容如何处理？      Git 会自动合并

- 把同一文件改成了不同的文件名如何处理？ Git 不会自动合并，需要开发者自行协商

  **补充：**发生冲突的图解：

  ![img](https://s2.loli.net/2022/02/24/U2Do3IfxqX7AteL.gif)

  摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

#### From 5.1

在团队协作时，严禁使用 `git push -f`，同时在 代码同步软件（比如 gitlab）可以限制开发者使用 `git push -f`

#### From 5.2

在团队合作时，公共分支是严禁拉到本地后，做变基( rebase )操作的。因为公共分支的历史不能被更改，所以不能使用 “会更改历史记录” 的 rebase 操作的。

可能产生的问题：做了变基之后，其他同事的代码 对应的 commit 会因为历史变更，让他本地的代码的头指针 和远端代码的头指针不是 fast-forward 了，导致在 push 代码时会报错。

#### // TODO 到这里 git 部分就讲完了，剩下的是 github gitlab CI/CD 的部分；暂时用不到，略。



#### <font color=FF0000>使用https url clone项目</font>

很多朋友在用github管理项目的时候，都是直接使用https url克隆到本地，当然也有有些人使用 SSH url 克隆到本地。然而，为什么绝大多数人会使用https url克隆呢？

这是因为，使用https url克隆对初学者来说会比较方便，复制https url 然后到 git Bash 里面直接用clone命令克隆到本地就好了。而使用 SSH url 克隆却需要在克隆之前先配置和添加好 SSH key 。

因此，如果你想要使用 SSH url 克隆的话，你必须是这个项目的拥有者。否则你是无法添加 SSH key 的。

#### <font color=FF0000>https 和 SSH clone的区别</font>

https可以随意克隆github上的项目，而不管是谁的；而SSH是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。

摘自：[github设置添加SSH](https://www.cnblogs.com/ayseeing/p/3572582.html)

#### <font color=FF0000>Github 上的账户的SSH keys与仓库的Deploy keys的区别</font>

- **Github账户的SSH keys**，相当于这个账号的最高级key，只要是这个账号有的权限（任何项目），都能进行操作。
- **仓库的Deploy keys**，顾名思义就是这个仓库的专有key，用这个key，只能操作这个项目，其他项目都没有权限。

说白了就相当于你有一所大别墅，<mark>SSH key能开别墅中的任何一个房间。而Deploy key只能开进别墅中的一个单间。</mark>

摘自：[github 上的账户的SSH keys与仓库的Deploy keys有何区别](https://segmentfault.com/q/1010000007356879)

**使用命令查看本地git安装地址**

```bash
where git # 注：类似的，可以通过 type git 起到类似的效果
```

摘自：[查看本地git安装位置](https://blog.csdn.net/weixin_42685022/article/details/84061043)



#### <font color=FF0000>问题汇总，出现如下问题：</font>

- **键入命令：**

  ```bash
  git clone git@github.com:***.git
  ```

  出现：

  ```
  Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
  git@github.com: Permission denied (publickey).
  fatal: Could not read from remote repository.
  
  Please make sure you have the correct access rights
  and the repository exists.
  ```

  参考：[Permanently added the RSA host key for IP address '13.250.177.223' to t he list of known hosts.](https://blog.csdn.net/yushuangping/article/details/84240863) 或者见官方教程：[用户头像  ==>  Setting  ==>  SSH and GPG keys  ==>  generate a GPG key and add it to your account  ==>  Adding a new GPG key to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)

- **键入命令：**

  ```bash
  git push
  ```

  - **出现：**

    ```bash
    To github.com:userName/projName.git
     ! [rejected]        master -> master (non-fast-forward)
    error: 推送一些引用到 'github.com:userName/projName.git' 失败
    提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。
    提示：再次推送前，先与远程变更合并（如 'git pull ...'）。详见
    提示：'git push --help' 中的 'Note about fast-forwards' 小节。
    ```

    解决方法：使用如下命令

    ```bash
    git push -u origin +master
    ```

  - **出现：**

    ```
    remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
    remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
    fatal: Authentication failed for 'https://github.com/github-user-name/proj-name.git/'
    ```

    这是由于 Github 更新了提交策略导致（具体说明，详见上面的链接：[Token authentication requirements for Git operations](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)）。解决方法如下：

    - **生成 Token：**具体步骤详见 [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)。另外，GitHub 的 Docs 中也有中文译文，这篇同样，页面上设置即可。

    - **在命令行中使用 Token：**这里主要讨论 “策略生效之前就已经在本地“ 的老项目 更新后无法提交的问题

      - **已经有仓库：**为仓库添加token

        ```sh
        git remote set-url origin https://<your-token>@github.com/<GitHub-user-name>/<repo-name>.git
        ```

      - **原本没有仓库：**git clone 添加token

        ```sh
        git clone https://<your-token>@github.com/<GitHub-user-name>/<repo-name>.git
        ```

      「在命令行中使用 Token」这部分摘自：[GitHub改为token验证后,如何提交代码?](https://python.iitter.com/other/39385.html)



***

#### 用户名和邮箱

**<font color=FF0000>查看</font>用户名和邮箱地址：**

```bash
# 设置用户名
git config user.name
# 设置用户邮箱
git config user.email
```

**<font color=FF0000>修改</font>用户名和邮箱地址：**

```bash
git config --global user.name  "xxxx"
git config --global user.email  "xxxx"
```

**如果在修改用户名和邮箱地址时出现如下情况：**

```
warning: user.email has multiple values
error: cannot overwrite multiple values with a single value
       Use a regexp, --add or --replace-all to change user.email.
```

**需要使用选项 **`--replace-all`

```bash
git config --global --replace-all user.email "输入你的邮箱" 
git config --global --replace-all user.name "输入你的用户名"
```

参考：[Git修改和配置用户名和邮箱](https://www.cnblogs.com/sunshinekevin/p/11617547.html)

#### 一些其他 git config 配置

- 如下命令能让Git以彩色显示：

  ```sh
  git config --global color.ui auto
  ```

- <font color=FF0000>**可以为Git命令设定别名**</font>。例如：把「checkout」缩略为「co」，然后就使用「co」来执行命令。

  ```sh
  git config --global alias.co checkout
  ```

  以上摘自：[猴子都能懂的GIT入门 - 教程1 Git的基础 - 初期设定](https://backlog.com/git-tutorial/cn/intro/intro2_2.html)

- **查看 Git 配置列表**

  ```bash
  git config --list
  ```

- 编辑Git配置文件

  ```bash
  git config -e [--global]
  ```

  以上内容摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)

  

#### Git 工作区、暂存区和版本库

- **工作区：**就是你<mark>在电脑里能看到的目录</mark>
- **暂存区：**英文叫 stage 或 index （**注：**vue3 作为 vue 默认版时，vue的官网也重写了；而当前 ( 2022/2/23 ) vue 新官网中文版还没正式上线，目前的网站为：https://staging-cn.vuejs.org ，其中 staging 的 应该就是 “暂存” 之意 ）。<mark>一般存放在 ".git目录下" 下的index文件（.git/index）中</mark>，所以我们把暂存区有时也叫作索引（index）。（注：将文件从工作区到暂存区，通过 `git add` ）
- **版本库：**工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。（注：从暂存区到版本区，通过 `git commit` )

![工作区、版本库中的暂存区和版本库之间的关系](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

摘自：[RUNOOB - Git 工作区、暂存区和版本库](https://www.runoob.com/git/git-workspace-index-repo.html)

- **git log：**会按时间先后顺序列出所有的提交，最近的更新排在最上面。这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。
  - 其中一个比较有用的选项是 `-p` 或 `--patch` ，它会显示每次提交所引入的差异（按 **补丁** 的格式输出）。
  - 你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交
  - 你也可以为 `git log` 附带一系列的总结性选项。 比如你想看到每次提交的简略统计信息，可以使用 `--stat` 选项
  
- **git show：**此命令可以用于显示各种类型对象的相关信息



#### Git 设置添加一个新的远程 Git 仓库

语法：

```sh
# 在目录中创建新的 Git 仓库
git init
# 为远端的仓库地址 repo-url 起别名。如果有多个远端仓库，只需要使用多个 git remote add 即可
git remote add <shortname> <repo-url>
```

示例：

```sh
git remote add origin git@github.com:userName/projName.git
```

在提交（push）代码之后，如果发现提交的代码存在问题，这时可以输入如下命令，以修改提交。

```shell
git commit --amend --no-edit
```

在提交（push）代码之后，若发现提交代码的注释（-m message）存在问题，这时可以输入如下命令：

```shell
git commit --amend
```

这时候会通过 vim 打开「提交」的相关信息（如下），只需要对其进行修改即可

<img src="https://s1.ax1x.com/2020/06/26/NrRojI.png" style="zoom: 40%;" />

另外：使用 如下命令，相当于预设了 git push 的参数（比如：远端仓库 和 分支名），之后直接使用 git push 命令即可，无需使用参数( 如：git push \<repo> \<branch>， 比如 git push -u origin master )了（因为已经预设了）。另外，在 GitHub 创建项目的提示中也有这个（不知道为什么现在找不到了...）.

```sh
# 这里的 origin 就是上面 git remote add 设置的
git push -u origin master
```

以上 git push -u 的内容参考自：

> 就是你第一次使用 git push -u origin master 之后，第二次【下次，以后】可以直接使用 git pull 拉取代码，就不需要输入完整的命令 git pull origin master 来拉取代码了。即 第二次 使用 git pull 等同于执行 git pull origin master。然后第二次也可以用 git push推送代码而不用git push origin master。即第二次 使用 git push 等同于执行 git push origin master。
>
> 摘自：[git push -u 的含义和用法](https://blog.csdn.net/chenzz444/article/details/104408607) 的评论区

**补充：**
**git push [remote] --all：**推送所有分支到远程仓库

补充内容摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)

<font size=4>**其他 git remote 命令**</font>

- **git remove -v：**显示所有远程仓库

- **git remote show [remote]：**显示某个远程仓库的信息

- **git remote rm repoName：**删除远程仓库

- **git remote rename oldName newName：**修改仓库名

  摘自：[RUNOOB - git remote 命令](https://www.runoob.com/git/git-remote.html)

- **git remote add [shortname] [url]**：增加一个新的远程仓库，并命名

- **git remote set-url repoRename remoteUrl：**设置远程仓库地址（用于修改远程仓库地址）

  摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git add

- **git add .：**提交所有<font color=FF0000>修改的</font>和<font color=FF0000>新建的</font>数据暂存区

- **git add -u：**u 即 update，提交所有<font color=FF0000>被删除</font>和<font color=FF0000>修改</font>的文件到数据暂存区

  **使用场景：**文件删除后，在用git add -u后，就能看见文件已经被提交到暂存区了。这个就可以以备不时之需，假如文件删错了，还能恢复回来

- **git add -A：**A 即 all，提交所有<font color=FF0000>被删除、被替换、被修改和新增的</font>文件到数据暂存区

  **git add -A 与 git add . 和 git add -u 差异的地方：**具有替换的文件的功能，在 git 中，会将 内容相同的文件，视作是同一个文件；如果一个文件被删除，而一个文件被新增，且和被删除文件的内容一样；则会被认为是同一个文件，且是被替换了。在 `git add -A` 后，运行 `git status`，会有如下输出：

  <img src="https://s2.loli.net/2022/02/23/mfI6l51qQUZoLs9.png" alt="image-20220223161117243" style="zoom:50%;" />

  其中 `bar.txt -> bar2.txt` 表示替换。

  摘抄学习自：[git add -A /git add -u/git add .的用法](https://blog.csdn.net/dayewandou/article/details/78513578)

- **git add [dir]：**添加指定目录到暂存区，包括子目录

- **git add ./*.js：**支持正则表达式

- **git add -p：**添加每个变化前，都会要求确认。对于同一个文件的多处变化，可以实现分次提交

  摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git commit 相关命令

- **git commit [file1] [file2] ... -m msg：**提交暂存区的指定文件到仓库区
- **git commit -a：**提交工作区自上次commit之后的变化，直接到仓库区
- **git commit -v：**<font color=FF0000>**提交时显示所有diff信息**</font>
- **git commit --amend -m msg：**使用一次新的commit，替代上一次提交。如果代码没有任何新变化，则用来改写上一次commit的提交信息
- **git commit --amend [file1] [file2] ...：**重做上一次commit，并包括指定文件的新变化

摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git clone

<font size=4>**git clone 做了什么？**</font>

1. 自动将服务器默认命名为 origin（**注：**相当于 `git push -u origin master`）
2. 创建远程分支origin / branch（指向master分支的指针）
3. 创建名为 master 的本地分支

删除远程分支以及追踪分支的命令： `git push origin --delete <branch>`

摘自：[删除分支 git branch -d与git branch -D的区别](https://blog.csdn.net/qq_33592641/article/details/103871482)



#### git diff

git diff 命令比较文件的不同，即<font color=FF0000>比较文件在暂存区和工作区的差异</font>。git diff 命令显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别。

**git diff 的应用场景：**

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动： **git diff --cached**
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**
- 显示摘要而非整个 diff：**git diff --stat**

显示暂存区和工作区的差异：

```sh
git diff [file]
```

显示暂存区和上一次提交 ( commit ) 的差异：

```sh
git diff --cached [file]
# 或者
git diff --staged [file]
```

显示两次提交之间的差异：

```sh
git diff [first-branch]...[second-branch]
```

摘自：[runoob - git diff 命令](https://www.runoob.com/git/git-diff.html)



#### git help

git 内置了对命令非常详细的解释，可以供我们快速查阅

- **git help：**查找可用命令

- **git help -a：**查找所有可用命令

- **git help \<command>：**在文档当中查找特定的命令

摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git prune

The `git prune` command is an internal housekeeping（家务） utility that <font color=fuchsia>**cleans up unreachable or "orphaned"**</font> （孤儿的）<font color=fuchsia>**Git objects**</font>. Unreachable objects are those that are inaccessible by any refs. Any commit that cannot be accessed through a branch or tag is considered unreachable. <font color=red>`git prune` is generally not executed directly</font>. <font color=fuchsia>Prune is considered a **garbage collection** command and is a **child command of the `git gc` command**</font>.

摘自：[Atlassian git tutorials - git prune](https://www.atlassian.com/git/tutorials/git-prune)



#### git gc

// TODO



#### 获取某些文件，某些分支，某次提交等 git 信息

- **git log --stat：**显示commit历史，以及每次commit发生变更的文件
- **git log -S [keyword]：**搜索提交历史，根据关键词
- **git log [tag] HEAD --pretty=format:%s：**显示某个commit之后的所有变动，每个commit占据一行
- **git log [tag] HEAD --grep feature：**显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
- **显示某个文件的版本历史，包括文件改名**
  - git log --follow [file]
  - git whatchanged [file]
- **git log -p [file]：**显示指定文件相关的每一次diff
- **git log -5 --pretty --oneline：**显示过去5次提交
- **git shortlog -sn：**显示所有提交过的用户，按提交次数排序
- **git blame [file]：**<font color=FF0000 size=4>**显示指定文件是什么人在什么时间修改过**</font>
- **git diff：**<font color=FF0000 size=4>显示暂存区和工作区的差异</font>
- **git diff --cached [file]：**<font color=FF0000>**显示暂存区和上一个commit的差异**</font>
- **git diff HEAD：**<font color=FF0000>显示工作区与当前分支最新commit之间的差异</font>
- **git diff [first-branch]...[second-branch]：**显示两次提交之间的差异
- **git diff --shortstat "@{0 day ago}"：**显示今天你写了多少行代码
- **git diff --staged：**<font color=FF0000>**比较暂存区和版本库差异**</font>
- **git diff --cached：**<font color=FF0000>**比较暂存区和版本库差异**</font>
- **git diff --stat：**仅仅比较统计信息
- **git show [commit]：**显示某次提交的元数据和内容变化
- **git show --name-only [commit]：**显示某次提交发生变化的文件
- **git show [commit]:[filename]：**显示某次提交时，某个文件的内容
- **git reflog：**显示当前分支的最近几次提交
- **git branch -r：**<font color=FF0000>查看远程分支</font>。**注：**-r 等价于 --remotes
- **git branch <new_branch>：**创建新的分支
- **git branch -v：**<font color=FF0000>查看各个分支最后提交信息</font>
- **git branch --merged：**查看已经被合并到当前分支的分支
- **git branch --no-merged：**查看尚未被合并到当前分支的分支

​	摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### Git 标签

标签是为了<font color=FF0000>更方便地参考提交而给它标上易懂的名称</font>。

**Git可以使用2种标签：**<font color=FF0000>轻标签和注解标签</font>。<mark>打上的标签是固定的，不能像分支那样可以移动位置</mark>。

- **轻标签**
  - 添加名称
- **注解标签**
  - 添加名称
  - 添加注解
  - 添加签名

一般情况下，<font color=FF0000>发布标签是采用 **注解标签** 来 **添加注解或签名**</font>的；<font color=FF0000>**轻标签**是为了**在本地暂时使用或一次性使用**</font>。

![注解标签，轻标签](https://s2.loli.net/2022/02/22/FQAzCRtT8LcmbJY.png)

<font size=4>**标签命令**</font>

- **创建标签：**

  - 创建 <font color=FF0000 size=4>**轻标签**</font>：`git tag <name>`

  - 添加 `-a` 选项来创建一个带备注的 tag（即：上面所说的 <font color=FF0000 size=4>**注解标签 **</font>），备注信息由 `-m` 指定。如果你未传入 `-m ` 则创建过程系统会自动为你打开编辑器让你填写备注信息：`git tag -a tagName -m msg`

    也可以把 -a 和 -m 合起来写：`git tag -am <msg> <name>`

  - 给指定的某个 commit 号加 tag： `git tag -a tagName -m msg`

- **删除标签：**`git tag -d <tagname>`

- **显示 tag 列表：**`git tag`

  添加 `-n` 选项，可以显示标签的列表和注解（不加 `-n` 则只有 tag）

- **推送 tag 到 远程服务器：**

  - 将tag同步到远程服务器：`git push origin v1.0`
  - 推送所有：`git push origin --tags`

摘自：[git 创建标签 tag](https://www.cnblogs.com/smiler/p/11576773.html)



#### git revert / git reset / git restore

- <font size=4>**git revert**</font>

  <font color=FF0000 size=4>**在 revert 可以取消指定的提交内容**</font>。使用 `git rebase -i` 或 `git reset` 也可以删除提交；但是，<font color=FF0000>不能随便删除已经发布的提交，这时需要通过 revert 创建要否定的提交</font>。另外，revert 可以 <font color=FF0000 size=4>安全地取消过去发布的提交</font>。

  ![取消过去的提交](https://s2.loli.net/2022/02/22/W7fAJwoh5YLaZ69.png)

  摘自：[猴子都能懂的GIT入门 - 改写提交 - 取消过去的提交](https://backlog.com/git-tutorial/cn/stepup/stepup6_2.html)

  <font size=4>**git revert 的进阶使用**</font>

  - **git revert  + HEAD + ^ / ~**

  - **git revert commitHash**：完整的 commit 或者该 commit 前7位即可

  - **git revert HEAD [, HEAD + ^/ ~ , ... ]**：回退连续的几个commit

    比如回退最新的三个commit：`git revert HEAD,HEAD~1,HEAD~2`

  - **git revert commitX...commitY**：commitX...commitY 代表一个左开右闭区间 (commitX, commitY]，不包括 commitX，包括commitY。其中 commitY 为起点 commit，commitX 为终点 commit 的下一个 commit。

    **注：**看了下下面的示例，发现 commitX 是 seniorCommit，commitY 是 juniorCommit；不知道是不是规则。

    **示例：**

    - git revert HEAD~3...HEAD
    - git revert 6a2cc3d...6a43ed0

  摘自：[git revert 详解](https://www.jianshu.com/p/5c562c0790fd)

  根据：[猴子都能懂的GIT入门 - 教程3 改写提交！- 2. revert](https://backlog.com/git-tutorial/cn/stepup/stepup7_2.html) 的示例，键入 `git revert HEAD` 命令（**注：**根据上图所示，应该可以通过 HEAD 配合 ^ / ~ 进行选择 commit 节点，从而改变非 HEAD 的 commit 节点），让如下状态的 git 管理的文件

  ![数据库的历史记录](https://s2.loli.net/2022/02/22/iAGwOkx6gBt3eZL.png)

  变成：

  ![revert之後的数据库的历史记录](https://s2.loli.net/2022/02/22/8tabURMBpyxvIGH.png)

  **产生如下两个现象：**

  - 让最后一行的文字被撤销，即让最后一行的文字（也是 HEAD 节点添加的）被撤销

    ```diff
    连猴子都懂的Git命令
    add 把变更录入到索引中
    commit 记录索引的状态
    - pull 取得远端数据库的内容
    ```

  - 键入 `git log` 会发现 多了一条日志：

    <img src="https://s2.loli.net/2022/02/22/dvu927Reqrkz4x8.png" alt="image-20220222205722482" style="zoom:50%;" />
    
    即：添加了一条 comit 记录。

  **补充：图解如下：**

  假设 ec5be 添加了一个 index.js 文件。但之后我们发现其实我们再也不需要由这个提交引入的修改了。那就还原 ec5be 提交吧！

  ![img](https://s2.loli.net/2022/02/24/PFadH6tCj9nWxXT.gif)

  完美！提交 9e78i 还原了由提交 ec5be 引入的修改。在撤销特定的提交时，git revert 非常有用，同时也不会修改分支的历史。

  补充内容摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

- <font size=4>**git reset**</font>

  <font color=FF0000 size=4>**git reset 可以遗弃不再使用的提交**</font>。执行遗弃时，需要<font color=FF0000>**根据影响的范围而指定不同的模式**</font>，可以 <font color=FF0000>**指定是否复原 索引 或 工作树 的内容**</font>。（注：由上可知，<font color=FF0000>**git reset 必定会修改 HEAD 的位置**</font>；另外，也可以从下面表格看出）

  ![遗弃提交](https://s2.loli.net/2022/02/22/dxDEfWKPt3oOT2U.png)

  <font size=4>**除了 <font color=FF0000>默认的 mixed 模式</font>，还有 soft 和 hard 模式**</font>。欲了解受各模式影响的部分，请参照下面的表格。

  | 模式名称 | 是否修改 HEAD的位置 | 是否修改索引（暂存区） | 是否修改工作树 |
  | :------: | :-----------------: | :--------------------: | :------------: |
  |   soft   |        修改         |         不修改         |     不修改     |
  |  mixed   |        修改         |          修改          |     不修改     |
  |   hard   |        修改         |          修改          |      修改      |

  **注，**一些自己的总结：无论什么模式，HEAD 都会被修改；soft 模式，只有 HEAD 会被修改；hard 模式，三者都会被修改

  **git reset 主要使用的场合：**

  - 复原修改过的索引的状态 ( mixed )
  - <font color=FF0000>彻底取消最近的提交</font> ( hard )
  - <font color=FF0000>只取消提交</font> ( soft )

  具体示例参见：《玩转 Git 三剑客 》2.8、2.11；另外，[猴子都能懂的GIT入门 - 教程3 改写提交！- 3. reset](https://backlog.com/git-tutorial/cn/stepup/stepup7_3.html) 中也有类似的示例，由于和 《玩转 Git 三剑客 》2.8、2.11 很类似，这里略。

  ##### 《工作流一目了然，看小姐姐用动图展示10大Git命令》中关于 reset命令的补充

  - **软重置：**

    软重置会将 HEAD 移至指定的提交（或与 HEAD 相比的提交的索引），而不会移除该提交之后加入的修改！

    假设我们不想保留添加了一个 style.css 文件的提交 9e78i，而且我们也不想保留添加了一个 index.js 文件的提交 035cc。但是，我们确实又想要保留新添加的 style.css 和 index.js 文件！这是软重置的一个完美用例。

    ![img](https://s2.loli.net/2022/02/24/nNqOBR7pXSeViIu.gif)

    输入 git status 后，你会看到我们仍然可以访问在之前的提交上做过的所有修改。这很好，这意味着我们可以修复这些文件的内容，之后再重新提交它们！

  - **硬重置：**

    有时候我们并不想保留特定提交引入的修改。不同于软重置，我们应该再也无需访问它们。Git 应该直接将整体状态直接重置到特定提交之前的状态：这甚至包括你在工作目录中和暂存文件上的修改。

    ![img](https://s2.loli.net/2022/02/24/f5IWNtFjAQrl9pV.gif)

    Git 丢弃了 9e78i 和 035cc 引入的修改，并将状态重置到了 ec5be 的状态。

  补充内容摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

  ##### 《Git不要只会pull和push，试试这5条提高效率的命令》中关于 git reset 的补充

  > **git reset --soft 的<mark style="background: aqua">使用场景</mark>：**
  >
  > - 有时候手滑不小心把不该提交的内容 commit 了，这时想改回来，只能再 commit 一次，又多一条“黑历史”。
  >
  > - 规范些的团队，一般对于 commit 的内容要求职责明确，颗粒度要细，便于后续出现问题排查。本来属于两块不同功能的修改，一起 commit 上去，这种就属于不规范。这次恰好又手滑了，一次性 commit 上去。
  >
  > reset --soft 相当于后悔药，给你重新改过的机会 。<mark style="background: aqua">对于上面的场景</mark>，就可以再次修改重新提交，保持干净的 commit 记录（<font color=FF0000 size=4>**之前修改的内容，会被放入暂存区**</font>）。
  >
  > <mark>以上说的是还未 push 的 commit</mark>。<font color=FF0000>对于已经 push 的 commit，也可以使用该命令，不过，**再次 push 时，由于远程分支和本地分支有差异，<font size=4>需要强制推送 git push -f 来覆盖被 reset 的 commit</font>**</font>。
  >
  > 还有一点需要注意⚠️：<font color=FF0000>在 reset --soft 指定 commit 号时，会将该 commit 到最近一次 commit 的所有修改内容全部恢复</font>，而不是只针对该 commit。
  >
  > 摘自：[Git不要只会pull和push，试试这5条提高效率的命令](https://juejin.cn/post/7071780876501123085)

- <font size=4>**git restore**</font>

  **自己总结：**

  - **git restore \<file>...：**对于工作区中被修改的文件，进行还原

  - **git restore --staged \<file>...：**将从工作区被添加到 暂存区中的文件，回滚到工作区中；但是修改的内容不变（不还原）。即：`git add <file>...` 的 逆操作。

  **自己试验的示例如下：**

  对一个文件进行修改，这是运行 git status，会显示<font color=FF0000>类似</font>下面的内容：

  <img src="https://s2.loli.net/2022/02/23/IOZbsuxlwEPiRKk.png" alt="image-20220223200832317" style="zoom:50%;" />

  运行红框中的命令 `git restore <file>...`，本次修改的内容将会被消除。

  不运行上面的 `git restore <file>...` 命令，而将其添加到「暂存区」（staged，即：使用 git add），之后运行 `git status` 会显示类似下面的内容：

  <img src="https://s2.loli.net/2022/02/23/Y1gdt95UnlGuHJz.png" alt="image-20220223201730281" style="zoom:50%;" />

  这时候，运行 红框中的命令 `git restore --staged <file>...` ，刚刚添加到「暂存区」的文件将会被移出「暂存区」，回到「工作区」；同时，被修改的内容不变，还需要 `git restore <file>...` 将其复原。

  **以下关于 `git restore` 的内容摘自**：[GIT撤销修改 restore](https://www.jianshu.com/p/dcef204dba74)

  `git restore --staged [file]`  表示从暂存区将文件的状态修改成 unstage  状态。当然，也可以不指定确切的文件 ，例如：
  `git restore --staged *.java` 表示将所有暂存区的java文件恢复状态；`git restore --staged .` 表示将当前目录所有暂存区文件恢复状态。

  **另外，如果 有错误的文件已经被 commit ，有如下三种方法：**

  - **git restore -s HEAD~1 \<file> ：**将版本回退到当前快照的前一个版本
  - **git restore -s commitHash \<file> ：**指定明确的 commit id ，回退到指定的快照中
  - **git reset --soft HEAD^ ：**撤销 commit 至上一次 commit 的版本

  **注：**从上面的示例可以见，`git restore -s <commitHash | HEAD^/~> <file>` 可以让版本回退到某个版本。

<font size=4>**git revert 和 git reset 的区别**</font>

- **reset：**<font color=FF0000>回朔到指定的commit版本</font>，<font color=FF0000 size=4>**指定的 commit 版本之后的操作 commit 都重置了**</font>。
- **revert：**<font color=FF0000 size=4>删除指定的 commit 操作的内容</font>，<font color=FF0000>**指定的commit之前和之后commit操作都不受影响**</font>，与此同时 <font color=FF0000 size=4>这个操作也会作为一个 commit 进行提交</font>（即：会新增一条 commit）。更容易理解的说法：git revert是用一次新的commit来回滚之前的commit

摘自：[git revert 详解](https://www.jianshu.com/p/5c562c0790fd) ，[git revert 用法](https://www.cnblogs.com/0616--ataozhijia/p/3709917.html)



#### 分支

<font size=4>**使用背景 / 痛点**</font>

在开发软件时，<mark>可能有多人同时为同一个软件开发功能或修复 BUG，<font color=FF0000>可能存在多个 Release 版本</font>，并且<font color=FF0000>需要对各个版本进行维护</font></mark>。所幸，<font color=FF0000 size=4>Git 的分支功能可以 **支持同时进行多个功能的开发和版本管理**</font>

<font size=4>**分支是什么**</font>

分支是为了将修改记录的整体流程分叉保存。<font color=FF0000 size=4>分叉后的分支不受其他分支的影响</font>，所以在同一个数据库里可以同时进行多个修改。另外，<font color=FF0000 size=4>**分叉的分支可以合并**</font>。

![img](https://s2.loli.net/2022/02/19/Uf8y9ixZJSOaKrw.png)

<mark>为了不受其他开发人员的影响，您 **可以在主分支上建立自己专用的分支**</mark>。<font color=FF0000 size=4>完成工作后，将自己分支上的修改合并到主分支</font>。因为每一次提交的历史记录都会被保存，所以当发生问题时，定位和修改造成问题的提交就容易多了。

![img](https://s2.loli.net/2022/02/19/9q18C4XLoBNFe7n.png)

在数据库进行最初的提交后，Git会创建一个名为 master 的分支。因此之后的提交，在切换分支之前都会添加到 master 分支里。

<font size=4>**两种分支 的 运用规则**</font>

- **Merge 分支：**<font color=FF0000 size=4>Merge 分支是**为了可以随时发布 release 而创建的分支**</font>，它 <font color=FF0000 size=4>还能作为 Topic 分支的 **源分支** 使用</font>（注：下面的 Topic 分支 中也有提及）。<font color=FF0000 size=4>保持分支稳定的状态是很重要的，**如果要进行更改，通常先创建 Topic 分支**</font>（**注：**感觉可以理解为，使用更小的粒度进行操作）；而针对该分支，可以使用 Jenkins 之类的 CI 工具进行自动化编译以及测试。

  通常，大家会将 master 分支当作 Merge 分支使用。

- **Topic 分支：**<font color=FF0000 size=4>Topic 分支**是为了开发新功能或修复 Bug 等任务而建立的分支**</font>（**注：**通过 Topic 的名字，就可以看出该分支的意思和目的，方便理解和记忆 ）。若<font color=FF0000>要同时进行多个的任务，请创建多个的 Topic 分支</font>（注：和上面的类似，使用更小粒度的同时，粒度之间最好是解耦的）。

  <font color=FF0000 size=4>Topic 分支**是从稳定的 Merge 分支创建的**</font>。<font color=FF0000>完成作业后，要把 Topic 分支 <font color=FF0000 size=4>**合并回**</font> Merge 分支</font>。

摘自：[猴子都能懂的GIT入门 - 分支的运用](https://backlog.com/git-tutorial/cn/stepup/stepup1_2.html)



#### merge rebase 合并分支

合并分支有2种方法：使用 merge 或 rebase。使用这2种方法，合并后分支的历史记录会有很大的差别。

- <font size=4>**merge：**</font>

  使用 merge 可以合并多个历史记录的流程。 

  如下图所示，bugfix分支是从master分支分叉出来的。

  ![分支](https://s2.loli.net/2022/02/22/bzRpQgWOSfD6KoN.png)

  master 分支时，<font color=FF0000>如果 master 分支的状态没有被更改过（注：即 fast-forward ），那么这个合并是非常简单的</font>。 <font color=FF0000>**bugfix 分支的历史记录包含master 分支所有的历史记录，所以通过把 master 分支的位置移动到bugfix的最新分支上，Git 就会合并**</font>。 <font color=FF0000 size=4>**这样的合并被称为 fast-forward（快进）合并**</font>。（**注：**这里没有产生新的节点)

  ![fast-forward合并](https://s2.loli.net/2022/02/22/k2slWP59tGAhvIS.png)

  但是，<mark>master 分支的历史记录有可能在 bugfix 分支分叉出去后有新的更新</mark>。<font color=FF0000>这种情况下，要把 master 分支的修改内容和 bugfix 分支的修改内容汇合起来</font>。

  ![分叉分支後进行新的更新](https://s2.loli.net/2022/02/22/HIqePVoZEwQuRJO.png)

  因此，<font color=FF0000 size=4>**合并两个修改会生成一个提交**</font>。这时，master 分支的 HEAD 会移动到该提交上。

  ![结合了两个修改的合并提交](https://s2.loli.net/2022/02/22/GSL9qEsKy1Tmcod.png)

  **注意：**执行合并时，<font color=FF0000>如果设定了 non fast-forward 选项，即使在能够 fast-forward 合并的情况下也会生成新的提交并合并</font>。执行 non fast-forward 后，分支会维持原状。那么要查明在这个分支里的操作就很容易了。

  ![non fast-forward合并](https://s2.loli.net/2022/02/22/6iu5WpJUtR4KdmT.png)

  **补充：**

  Git 可执行两种类型的合并：fast-forward 和 no-fast-forward。

  - **Fast-forward ( --ff )：**

    <font color=FF0000>在当前分支相比于我们要合并的分支没有额外的提交（commit）时，可以执行 fast-forward 合并</font>。<font color=FF0000>Git</font> 很懒，<font color=FF0000>首先会尝试执行最简单的选项：fast-forward</font>！这类合并<font color=FF0000>不会创建新的提交</font>，而是<font color=FF0000>会将我们正在合并的分支上的提交直接合并到当前分支</font>。

    ![img](https://pic2.zhimg.com/v2-0a0431c992211561f14ee66f1cf0ea89_b.gif)

  - **No-fast-foward ( --no-ff )：**

    <mark>如果你的当前分支相比于你想要合并的分支没有任何提交，那当然很好，但很遗憾现实情况很少如此</mark>！如果我们<font color=FF0000>在当前分支上提交我们想要合并的分支不具备的改变，那么 git 将会执行 no-fast-forward 合并</font>。

    <font color=FF0000>**使用 no-fast-forward 合并时，Git 会在当前活动分支上创建新的 merging commit**</font>。这个提交的父提交（parent commit）即指向这个活动分支，也指向我们想要合并的分支！

    ![img](https://s2.loli.net/2022/02/23/1vajGPTt4wAy76D.gif)

  摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

  **补充：**

  **git merge 有一个 `--no-ff` 的选项：**<font color=FF0000>在合并时创建一个新的合并后的提交，不要 Fast-Foward 合并，这样可以生成 merge 提交</font>

  补充内容学习自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)

- <font size=4>**rebase**</font>

  跟merge的例子一样，如下图所示，bugfix分支是从master分支分叉出来的。

  ![分支](https://s2.loli.net/2022/02/22/W5J98HkwNEpRZor.png)

  如果 <font color=FF0000>使用 rebase 方法进行分支合并，会出现下图所显示的历史记录</font>（**注：**这和 merge 完全不一样。因为这使得 <font color=FF0000 size=4>commit 之间的关系，变成线性的了</font>。同时，使用 rebase 进行合并，没有产生新的 commit)。现在我们来简单地讲解一下合并的流程吧。

  ![使用rebase合并分支](https://s2.loli.net/2022/02/22/VZ96GN7vi5IhxAB.png)

  首先，rebase bugfix 分支到 master 分支， <font color=FF0000 size=4>**bugfix 分支的历史记录会添加在 master 分支的后面**</font>。如图所示，历史记录成一条线，相当整洁。<font color=FF0000>**这时移动提交 X 和 Y 有可能会发生冲突，所以需要修改各自的提交时发生冲突的部分**</font>。

  ![使用rebase合并分支](https://s2.loli.net/2022/02/22/JlObyNVtxmRc8Pw.png)

  <font color=FF0000 size=4>**rebase 之后，master 的 HEAD 位置不变**</font>。因此，要合并 master 分支和 bugfix 分支，即是<font color=FF0000>将 master 的 HEAD 移动到 bugfix 的 HEAD这里</font>。

  ![使用rebase合并分支](https://s2.loli.net/2022/02/22/ekvPxlNRAaLFdh1.png)
  
  <font size=4>**补充：**</font>
  
  git rebase 会将当前分支的提交复制到指定的分支之上。
  
  ![https://pic2.zhimg.com/v2-6b8427b4baf6cdfb08b852ab1cdb4941_b.gif](https://s2.loli.net/2022/02/24/PAu9XB4nV1wZ6Gp.gif)
  
  **变基与合并有一个重大的区别：**Git 不会尝试确定要保留或不保留哪些文件。我们执行 rebase 的分支总是含有我们想要保留的最新近的修改！这样我们不会遇到任何合并冲突，而且可以保留一个漂亮的、线性的 Git 历史记录。
  
  **交互式变基（Interactive Rebase）**
  
  在为提交执行变基之前，我们可以修改它们！我们可以使用交互式变基来完成这一任务。交互式变基在你当前开发的分支上以及想要修改某些提交时会很有用。
  
  **在我们正在 rebase 的提交上，我们可以执行以下 6 个动作：**
  
  - **reword：**修改提交信息。**注：**即编辑 commit 的 msg
  - **edit：**修改此提交；
  - **squash：**将提交融合到前一个提交中；
  - **fixup：**将提交融合到前一个提交中，不保留该提交的日志消息；
  - **exec：**在每个提交上运行我们想要 rebase 的命令；
  - **drop：**移除该提交。
  
  很棒！这样我们就能完全控制我们的提交了。如果你想要移除一个提交，只需 drop 即可。
  
  ![https://pic4.zhimg.com/v2-7189da3226d1fdedeb6a297fbc2b1177_b.gif](https://s2.loli.net/2022/02/24/S9KjiUEZNoRQB36.gif)
  
  如果你想把多个提交融合到一起以便得到清晰的提交历史，那也没有问题！
  
  ![img](https://s2.loli.net/2022/02/24/1mHqAysCPIY9drT.gif)
  
  交互式变基能为你在 rebase 时提供大量控制，甚至可以控制当前的活动分支。
  
  补充内容摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

<font size=4>**总结**</font>

**merge 和 rebase 都是合并历史记录，但是各自的特征不同。**

- **merge：**<font color=FF0000 size=4>**保持修改内容的历史记录，但是历史记录会很复杂**</font>。
- **rebase：**<font color=FF0000>历史记录简单，是在原有提交的基础上将差异内容反映进去</font>。因此，可能导致原本的提交内容无法正常运行。

**您可以根据开发团队的需要分别使用merge和rebase。例如，想简化历史记录，**

- <font color=FF0000>在 topic 分支中更新 merge 分支的最新代码</font>，请<font color=FF0000>使用 rebase</font>。
- <font color=FF0000>向 merge 分支导入 topic 分支</font>的话，<font color=FF0000>先使用 rebase，再使用merge</font>。

摘自：[猴子都能懂的GIT入门 - 分支的合并](https://backlog.com/git-tutorial/cn/stepup/stepup1_4.html)

**补充：**

**git rebase 与 git merge的最大区别是：**它会更改变更历史对应的 commit 节点。

如下图，当在 feature 分支中执行 rebase master 时，Git 会以 master 分支对应的 commit 节点为起点，新增两个**全新**的 commit 代替 feature分支中的commit节点。其原因是新的commit指向的parent变了，所以对应的SHA1值也会改变，所以没办法复用原feature分支中的commit。（这句话的理解需要[这篇文章](https://www.lzane.com/tech/git-internal/)的基础知识）

![image-20220424230558912](https://s2.loli.net/2022/04/24/4ldTCBLZgaxKSzp.png)

补充内容摘自：[这才是真正的GIT——分支合并](https://www.lzane.com/tech/git-merge/)



<font size=4>**git merge 命令**</font>

**git merge \<branch>：**将指定分支导入到HEAD指定的分支。

**注意，在实际开发时发现：**在运行 git merge 之前，需要将被合并分支的内容均 commited；否则无法将想要的内容<font color=FF0000 size=4>**完全**合并成功</font>。

**merge 有一个特殊选项：squash**（注：意为 拥挤( n )，压缩( verb ) ）。用这个选项指定分支的合并，就可以 <font color=FF0000 size=4>**把所有汇合的提交添加到分支上**</font>（注：如下图所示，将 X 和 Y 两个提交合并在一起，并通过 git merge，生成新的节点）。

![squash选项](https://s2.loli.net/2022/02/22/YXGWnJliQUC8RPL.png)

**主要使用的场合：**汇合主题分支的提交，然后合并提交到目标分支。

「squash 相关」摘自：[猴子都能懂的GIT入门 - 汇合分支上的提交，然后一同合并到分支](https://backlog.com/git-tutorial/cn/stepup/stepup6_6.html)

squash 选项示例：[猴子都能懂的GIT入门 - 教程3 改写提交！-  merge --squash](https://backlog.com/git-tutorial/cn/stepup/stepup7_7.html)，在 试了 `git merge --squash issue1` 和 `git merge issue1` 两种 merge 后，发现  `git merge --squash issue1`  是将 issue1 中 <font color=FF0000 size=4>所有**分支产生后添加的 commit 节点**都合并了</font>，并入了 master 分支中；因此多了一个 commit。而 `git merge issue1` 是不合并 issue1 中分支产生后添加的 commit 节点，将其完全并入 master 分支中；多了两个 commit 节点（应该是 覆盖掉了 master 分支中 head 位置的 commit，并入了 issue1 分支中的 两个 commit，并新生成了 一个合并后的 commit）

<font size=4>**git rebase**</font>

rebase 的时候，修改冲突后的提交不是使用 commit 命令，而是执行 rebase 命令指定 --continue 选项。若 <font color=FF0000 size=4>要取消 rebase，指定 --abort 选项</font>。



<font size=4>**git fetch 命令**</font>

执行 pull，远程数据库的内容就会自动合并。但是，<mark>有时只是想确认本地数据库的内容而不想合并。这种情况下，请使用 fetch</mark>。

<mark>执行 fetch 就可以取得远程数据库的最新历史记录</mark>。<font color=FF0000>**取得的提交 会导入到没有名字的分支，这个分支名为 FETCH_HEAD**</font>。

![在本地端数据库和远端数据库的origin，在从B进行提交的状态下执行fetch](https://s2.loli.net/2022/02/22/I5YkiFWzatSZcfs.png)

在这个状态下，若要把远程数据库的内容合并到本地数据库，可以合并 FETCH_HEAD，或者重新执行 pull。

![合并FETCH_HEAD](https://s2.loli.net/2022/02/22/mRhTzNADxVoPnX7.png)

**补充：**

- **git fetch 图解：**
  ![https://pic2.zhimg.com/v2-686ae54f78ea69b6c00cc8b159cf7369_b.gif](https://s2.loli.net/2022/02/24/xZMOgY6LENzCp3u.gif)

- **git pull 图解：**
  ![https://pic2.zhimg.com/v2-1298832b975cf9cf0ad6c399ec5da32d_b.gif](https://s2.loli.net/2022/02/24/L6tx2VHDyfsCArh.gif)

补充内容摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)



<font size=4>**git pull 和 git merge的区别**</font>

- **git pull = git fetch + git merge**

  - fetch 和 push 可以分别对远程分支进行 fetch 和 push 操作

  - pull 不是直接跟远程分支对话的

- **fetch 和 pull的区别在于：**

  - **git fetch：**是从远程获取最新版本到本地，不会自动 merge

  - **git pull：**是从远程获取最新版本并 merge 到本地仓库

从安全角度出发，git fetch 比 git pull更安全，因为我们可以先比较本地与远程的区别后，选择性的合并。

摘自：[segmentfault Salamander的回答 -- git pull和git merge 区别? ](https://segmentfault.com/q/1010000009076820)

**补充：**

git pull 有 `--no-ff` 选项，表示：抓取远程仓库所有分支更新并合并到本地，不要快进合并

补充内容摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git cherry-pick

使用 `git cherry-pick`，您可以 <font color=FF0000>从其他分支复制指定的提交，然后导入到现在的分支</font>。

![提取提交](https://s2.loli.net/2022/02/22/63rU5ZkLSEymOpi.png)

**主要使用的场合：**

- 把弄错分支的提交移动到正确的地方
- 把其他分支的提交添加到现在的分支

摘自：[猴子都能懂的GIT入门 - 提取提交](https://backlog.com/git-tutorial/cn/stepup/stepup6_4.html)

在 [猴子都能懂的GIT入门 - - 教程3 改写提交！- 4. cherry-pick](https://backlog.com/git-tutorial/cn/stepup/stepup7_4.html) 有示例。命令为 `git cherry-pick commitHash` 其实挺简单；大致意思就是，在一个分支（假设为 A）中，将另一个分支（假设为 B）的 commit （commitHash 所对应的 commit）中的内容导入该分支 ( A ) 中（只是倒入这个 commitHash 指定的 commit，他后面无论有什么变化，都不导入）；另外，该操作可能会导致冲突，如果出现，需要手动清除冲突（示例中就存在冲突）。

**补充：**

假设 dev 分支上的提交 76d12 为 index.js 文件添加了一项修改，而我们希望将其整合到 master 分支中。<mark>我们并不想要整个 dev 分支，而只需要这个提交</mark>！

![img](https://s2.loli.net/2022/02/24/yLq3CbzrYGjS1Mh.gif)

现在 master 分支包含 76d12 引入的修改了。

补充内容摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)

##### 《Git不要只会pull和push，试试这5条提高效率的命令》中关于 cherry-pick 的补充

> **应用场景**
>
> - 有时候版本的一些优化需求开发到一半，可能其中某一个开发完的需求要临时上，或者某些原因导致待开发的需求卡住了已开发完成的需求上线。这时候就需要把 commit 抽出来，单独处理。
>
> - 有时候开发分支中的代码记录被污染了，导致开发分支合到线上分支有问题，这时就需要拉一条干净的开发分支，再从旧的开发分支中，把 commit 复制到新分支。
>
> **相关命令选项**
>
> - 一次转移多个提交：
>   ```sh
>   git cherry-pick commit1 commit2
>   ```
>
> - 多个连续的commit，也可区间复制
>
>   ```sh
>   git cherry-pick commit1^..commit2
>   ```
>
> - **cherry-pick 代码冲突**
>
>   在 cherry-pick 多个commit时，<font color=FF0000>可能会遇到代码冲突，这时 cherry-pick 会停下来，让用户决定如何继续操作</font>。这时候可以：解决代码冲突，重新提交到暂存区。然后使用 `cherry-pick --continue` 让 cherry-pick 继续进行下去。
>
> - **放弃 cherry-pick：**<font color=FF0000>回到操作前的样子，就像什么都没发生过</font>。
>
>   ```sh
>   gits cherry-pick --abort
>   ```
>
> - **退出 cherry-pick：**<font color=FF0000>不回到操作前的样子</font>。即<font color=FF0000>保留已经 cherry-pick 成功的 commit，并退出 cherry-pick 流程</font>。
>
>   ```sh
>   git cherry-pick --quit
>   ```
>
> 摘自：[Git不要只会pull和push，试试这5条提高效率的命令](https://juejin.cn/post/7071780876501123085)



#### git <font color=FF0000>ref</font>log

注意上面的红色的字体，即 reflog 的意思为 ref + log。

键入 git reflog 显示的效果图示例如下：

<img src="https://s2.loli.net/2022/02/24/wU4PkXDcx2ftbaQ.png" alt="image-20220224200700730" style="zoom:45%;" />

git reflog 是一个非常有用的命令，<font color=FF0000 size=4>可以 **展示已经执行过的所有动作的日志**</font>。<font color=FF0000>包括合并、重置、还原，基本上包含你对你的分支所做的任何修改</font>。

![https://pic1.zhimg.com/v2-cda251045d9d8b2d5b65db533b6ba3cc_b.gif](https://s2.loli.net/2022/02/24/9jVnWPaielUhvuL.gif)

如果你犯了错，你<font color=FF0000>可以根据 reflog 提供的信息通过重置 HEAD 来轻松地重做</font>！

假设我们实际上并不需要合并原有分支。当我们执行 git reflog 命令时，我们可以看到这个 repo 的状态在合并前位于 HEAD@{1}。那我们就执行一次 git reset，将 HEAD 重新指向在 HEAD@{1} 的位置。

![https://pic3.zhimg.com/v2-4131550e150396f1dfc6a8242c6d103a_b.gif](https://s2.loli.net/2022/02/24/KeGSCd8XDB4EmhW.gif)

我们可以看到最新的动作已被推送给 reflog。

摘自：[工作流一目了然，看小姐姐用动图展示10大Git命令](https://zhuanlan.zhihu.com/p/132573100)



#### git read-tree

这是一个比较底层的命令，这里提一下，Pro Git  中的解释是：

> git-read-tree - Reads tree information into the index
>
> 即：读取树状的信息，将其放入索引区（暂存区）

摘自：[Pro Git - giit-read-tree](https://git-scm.com/docs/git-read-tree)



#### 分支命令补充

- **git branch -m oldBranchName newBranchName：**分支重命名
- **git checkout -：**切换到上一个分支
- **git branch --set-upstream [branch] [remote-branch]：**建立追踪关系，在现有分支与指定的远程分支之间
- **删除远程分支：**
  - git push origin --delete [branch-name]
  - git branch -dr [remote/branch]

摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### 其他 git 命令

- **git archive：**生成一个可供发布的压缩包
- **git apply ../sync.patch：**打补丁
- **git apply --check ../sync.patch**：测试补丁能否成功
- **git --version：**查看Git的版本

摘自：[给自己点时间再记记这200条Git命令](https://zhuanlan.zhihu.com/p/137194960)



#### git 具体需求的解决方法

- <font size=4>**找回丢失的commit节点或分支**</font>：使用 `git reflog` 获取 commitHash，再使用 `git reset --hard commitHash`，示例如下

  <img src="https://www.lzane.com/tech/git-tips/git_reflog.gif" alt="https://www.lzane.com/tech/git-tips/git_reflog.gif" style="zoom:80%;" />

  主要思路为：**找到要返回的commit object的哈希值，然后执行`git reset`恢复**。

  我们知道 Git 的出现就是为了尽量保证我们的操作不被丢失，在 [Git内部原理](https://www.lzane.com/tech/git-internal/) 中我们讲过，<font color=FF0000>git object ( commit、tree、blob ) 一旦被创建，就不可变更 ( immutable )</font>；所以只要找到它对应的哈希值，就能找回。但是 <font color=FF0000>ref</font> 呢？在 [Git内部原理](https://www.lzane.com/tech/git-internal/) 中我们也讲过，<font color=FF0000>它是一个可变的指针</font>，比如说你在 master 中提交了一个 commit，那当前的 master 这个 ref 就会指向新的 commit object 的哈希值。reflog 就是将这些可变指针的历史给记录下来，可以理解成 **ref的log**，也可以理解成 **版本控制的版本控制**。

- <font size=4>**提交一个文件中的部分修改**</font>：使用 `git add -i`，具体如下：

  <img src="https://s2.loli.net/2022/02/25/FeqsIg7XVDk38QZ.gif" alt="https://www.lzane.com/tech/git-tips/git_add_i.gif" style="zoom:80%;" />

- <font size=4>**从整个历史中删除一个文件**</font>

  代码要开源了，但发现其中包括密钥文件或内网ip怎么办？

  ```bash
  git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
  ```

  可以使用 filter-branch 命令，它的实现原理是将每个 commit checkout出来，然后执行你给它的命令，像上面的 `rm -f passwords.txt`，然后重新 commit 回去。

  ⚠️ 这个操作属于高危操作，会修改历史变更记录链，产生全新的commit object。所以执行前请通知仓库的所有开发者，执行后所有开发者从新的分支继续开发，弃用以前的所有分支。

- <font size=4>**查看git 分支之间的区别**</font>

  ```sh
  git show-branch
  ```

- <font size=4>**二分查找出现问题的变更节点**</font>

  当代码出现问题时，想要找到是哪一个变更导致了该问题的出现；此时，会很自然的想要使用二分法；而人工二分法非常麻烦，git 会自动做二分法，你只需要告诉 git 这个二分结果是好的还是坏的，从而定位到变更的地方。

  ```sh
  git bisect
  ```

  学习自：[[中文] 这才是真正的 Git——Git 内部原理揭秘！（freeCodeConf 2019 深圳站）](https://www.bilibili.com/video/av77252063) 



#### git 图解

<font size=4>**基本用法：**</font>

<img src="https://marklodato.github.io/visual-git-guide/basic-usage.svg" alt="img" style="zoom: 70%;" />

**上面的四条命令在工作目录、暂存目录（也叫做索引）和仓库之间复制文件：**

- **git add files：**把当前文件放入暂存区域。
- **git commit：**给暂存区域生成快照并提交。
- <font color=FF0000 size=4>**git reset -- files：**</font>用来<font color=FF0000>撤销最后一次 `git add files`</font>，你<font color=FF0000>也可以用 `git reset` 撤销所有暂存区域文件</font>。
- <font color=FF0000 size=4>**git checkout -- files：**</font><font color=FF0000>把文件从暂存区域复制到工作目录，用来丢弃本地修改</font>。

**也可以跳过暂存区域直接从仓库取出文件或者直接提交代码：**

<img src="https://marklodato.github.io/visual-git-guide/basic-usage-2.svg" alt="img" style="zoom:70%;" />

- **git commit -a：**相当于运行 `git add` 把所有当前目录下的文件加入暂存区域再运行 `git commit`
- **git commit files：**进行一次包含最后一次提交加上工作目录中文件快照的提交，并且文件被添加到暂存区域。
- **git checkout HEAD -- files：**回滚到复制最后一次提交。

<font size=4>**git diff**</font>

<img src="https://marklodato.github.io/visual-git-guide/diff.svg" alt="img" style="zoom: 65%;"/>

**注：**注意上图，git diff 的默认值，和不同参数之间 效果的区别。

<font size=4>**git commit**</font>

提交时，git用暂存区域的文件创建一个新的提交，并把此时的节点设为父节点。然后把当前分支指向新的提交节点。下图中，当前分支是main。 在运行命令之前，main 指向 ed489，提交后，main 指向新的节点 f0cec 并以 ed489 作为父节点。

<img src="https://marklodato.github.io/visual-git-guide/commit-main.svg" alt="img" style="zoom:65%;" />

<font color=FF0000>**即便当前分支是某次提交的祖父节点，git 会同样操作**</font>。下图中，在 main分支的祖父节点 stable分支进行一次提交，生成了 1800b。 这样，stable分支 就不再是 main分支的祖父节点。此时，合并 (或者 衍合) 是必须的。

<img src="https://marklodato.github.io/visual-git-guide/commit-stable.svg" alt="img" style="zoom:68%;" />

如果想更改一次提交，使用 `git commit --amend`。git 会使用与当前提交相同的父节点进行一次新提交，旧的提交会被取消。

<img src="https://marklodato.github.io/visual-git-guide/commit-amend.svg" alt="img" style="zoom:67%;" />

<font size=4>**git checkout**</font>

<font color=FF0000 size=4>**checkout命令 用于从历史提交（或者暂存区域）中拷贝文件到工作目录**</font>，<font color=FF0000>也可用于切换分支</font>。

<font color=FF0000>**当给定某个文件名**</font>（或者打开 -p选项，或者 文件名 和 -p选项 同时打开）时，<font color=FF0000 size=4>**git 会从指定的提交中拷贝文件到暂存区域和工作目录**</font>（**注：**<mark>注意，如下图，是同时拷入 暂存区 和 工作区；另外，这也是最上面所说的 `git add files` 的逆操作</mark>）。比如，<font color=FF0000>`git checkout HEAD~ foo.c` 会将提交节点 *HEAD~* （即当前提交节点的父节点）中的 `foo.c` 复制到工作目录并且加到暂存区域中</font>。（如果命令中没有指定提交节点，则会从暂存区域中拷贝内容）注意当前分支不会发生变化。

<img src="https://marklodato.github.io/visual-git-guide/checkout-files.svg" alt="img" style="zoom:67%;" />

<font color=FF0000>**如果既没有指定文件名，也没有指定分支名，而是一个标签、远程分支、SHA-1值或者是像 *main~3* 类似的东西，就得到一个匿名分支，称作 *detached HEAD*（被分离的 *HEAD*标识）**</font>。这样可以很方便地在历史版本之间互相切换。比如说你想要编译 1.6.6.1版本的 git，你可以运行 `git checkout v1.6.6.1`（<font color=FF0000>这是一个标签，而非分支名</font>），编译，安装，然后切换回另一个分支，比如说 `git checkout main`。然而，当提交操作涉及到“分离的HEAD”时，其行为会略有不同，详情见在[下面](https://marklodato.github.io/visual-git-guide/index-zh-cn.html#detached)。

<img src="https://marklodato.github.io/visual-git-guide/checkout-detached.svg" alt="img" style="zoom:67%;" />

<font size=4>**HEAD 标识处于分离状态时的提交操作**</font>

<font color=FF0000>**当 *HEAD* 处于分离状态（不依附于任一分支）时，提交操作可以正常进行，但是不会更新任何已命名的分支**</font>（你可以认为这是在更新一个匿名分支）

<img src="https://marklodato.github.io/visual-git-guide/commit-detached.svg" alt="img" style="zoom:67%;" />

一旦此后你切换到别的分支，比如说 main，那么这个提交节点（可能）再也不会被引用到，然后就会被丢弃掉了（**注：**可以理解为回收掉了( GC ) ）。注意这个命令之后就不会有东西引用 2eecb。

<img src="https://marklodato.github.io/visual-git-guide/checkout-after-detached.svg" alt="img" style="zoom:67%;" />

但是，如果你想保存这个状态，可以用命令`git checkout -b <name>`来创建一个新的分支。

<img src="https://marklodato.github.io/visual-git-guide/checkout-b-detached.svg" alt="img" style="zoom:67%;" />

<font size=4>**git reset**</font>

<font color=FF0000>**reset命令 把当前分支指向另一个位置，并且有选择的变动工作目录和索引**</font>（**注：**有选择的即 通过选项）。<font color=FF0000>也用来在从历史仓库中复制文件到索引，而不动工作目录</font>。

如果不给选项，那么当前分支指向到那个提交。如果用 `--hard` 选项，那么工作目录也更新，如果用 `--soft` 选项，那么都不变。

<img src="https://marklodato.github.io/visual-git-guide/reset-commit.svg" alt="img" style="zoom:67%;" />

如果<font color=FF0000>没有给出提交点的版本号，那么**默认用 *HEAD***</font>。这样，<font color=FF0000>分支指向不变，但是索引会回滚到最后一次提交</font>，如果用 `--hard` 选项，工作目录也同样。

<img src="https://marklodato.github.io/visual-git-guide/reset.svg" alt="img" style="zoom:67%;" />

如果给了文件名（或者 -p选项），那么工作效果和带文件名的 checkout 差不多，除了索引被更新。

<img src="https://marklodato.github.io/visual-git-guide/reset-files.svg" alt="img" style="zoom:67%;" />

<font size=4>**git merge**</font>

merge 命令把不同分支合并起来。合并前，索引必须和当前提交相同。如果另一个分支是当前提交的祖父节点，那么合并命令将什么也不做。 另一种情况是如果当前提交是另一个分支的祖父节点，就导致 *fast-forward* 合并。指向只是简单的移动，并生成一个新的提交。

<img src="https://marklodato.github.io/visual-git-guide/merge-ff.svg" alt="img" style="zoom:67%;" />

否则就是一次真正的合并。默认把当前提交（*ed489* 如下所示）和另一个提交( *33104* )以及他们的共同祖父节点( *b325c* )进行一次[三方合并](http://en.wikipedia.org/wiki/Three-way_merge)。结果是先保存当前目录和索引，然后和父节点 *33104* 一起做一次新提交。

<img src="https://marklodato.github.io/visual-git-guide/merge.svg" alt="img" style="zoom:67%;" />

<font size=4>**Cherry Pick**</font>

cherry-pick命令 "复制"一个提交节点并在当前分支做一次完全一样的新提交。

<img src="https://marklodato.github.io/visual-git-guide/cherry-pick.svg" alt="img" style="zoom:67%;" />

<font size=4>**Rebase**</font>

衍合（**即：**变基）是合并命令的另一种选择。合并把两个父分支合并进行一次提交，提交历史不是线性的。衍合在当前分支上重演另一个分支的历史，提交历史是线性的。 本质上，这是线性化的自动的 cherry-pick

<img src="https://marklodato.github.io/visual-git-guide/rebase.svg" alt="img" style="zoom:67%;" />

上面的命令都在 *topic* 分支中进行，而不是 *main* 分支，在 *main* 分支上重演，并且把分支指向新的节点。注意旧提交没有被引用，将被回收。

要限制回滚范围，使用 `--onto` 选项。下面的命令在 *main* 分支上重演当前分支从 *169a6* 以来的最近几个提交，即 *2c33a*。

<img src="https://marklodato.github.io/visual-git-guide/rebase-onto.svg" alt="img" style="zoom:67%;" />

摘自：[图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html)



#### git 的合并原理

在看怎么合并两个分支之前，我们先来看一下怎么合并两个文件，因为两个文件的合并是两个分支合并的基础。

大家应该都听说过“三向合并”这个词，不知道大家有没有思考过为什么两个文件的合并需要三向合并，只有二向是否可以自动完成合并。如下图

<img src="https://s2.loli.net/2022/02/25/jeQPNk6WspSnVtf.png" alt="title" style="zoom: 85%;" />

很明显答案是不能，如上图的例子，Git没法确定这一行代码是我修改的，还是对方修改的，或者之前就没有这行代码，是我们俩同时新增的。此时Git没办法帮我们做自动合并。

所以我们需要三向合并，<font color=FF0000 size=4>所谓三向合并，就是找到两个文件的一个合并base</font>（**注：**base 就是合并的依据），如下图，这样子Git就可以很清楚的知道说，对方修改了这一行代码，而我们没有修改，自动帮我们合并这两个文件为 Print(“hello”)。

![title](https://www.lzane.com/tech/git-merge/img_4.png)

接下来我们了解一下什么是冲突？<font color=FF0000 size=4>冲突简单的来说就是三向合并中的三方都互不相同</font>，即参考合并base，我们的分支和别人的分支都对同个地方做了修改。

<img src="https://s2.loli.net/2022/02/25/MXheTb8wdt4vz5G.png" alt="title" style="zoom:95%;" />

<font size=4>**合并策略**</font>

Git会有很多合并策略，其中常见的是 Fast-forward、Recursive 、Ours、Theirs、Octopus。<font color=FF0000 size=4>**默认Git会帮你自动挑选合适的合并策略**</font>  ，如果你需要强制指定，使用 `git merge -s <strategyName>`

- **Fast-forward：**<font color=FF0000>Fast-forward 是最简单的一种合并策略</font>，Git 只需要将 master 分支的指向移动到最后一个 commit 节点上。Fast-forward 是 Git 在合并两个没有分叉的分支时的默认行为，<font color=FF0000>如果不想要这种表现，想明确记录下每次的合并，可以使用 `git merge --no-ff`</font>。

- **Recursive：**<font color=FF0000 size=4>Recursive 是 Git 分支合并策略中 **最重要** 也是 **最常用** 的策略，是 Git 在 **合并两个有分叉的分支时** 的 **默认行为**</font>。其算法可以简单描述为：<font color=FF0000 size=4>递归寻找路径最短的唯一共同祖先节点，然后以其为 base节点进行递归三向合并</font>

  当 Git 在寻找路径最短的共同祖先节点的时候，是可能找到多个最短的共同祖先节点的；这样会带来：以哪一个节点作为 base，而使用不同的base 带来不同的结果（甚至是冲突）。<font color=FF0000>怎么保证Git能够找到正确的合并 base 节点，尽可能的减少冲突呢</font>？答案就是，<font color=FF0000 size=4>Git 在寻找路径最短的共同祖先节点时，**如果满足条件的祖先节点不唯一，那么 Git 会继续递归往下寻找直至唯一**</font>。Recursive 策略已经被大量的场景证明它是一个尽量减少冲突的合并策略。

  需要注意 Git 只是使用这些策略尽量的去帮你减少冲突，如果冲突不可避免，那 Git 就会提示冲突，需要手工解决。（也就是真正意义上的冲突）。

- **Ours & Theirs：**Ours 和 Theirs这两种合并策略也是比较简单的，简单来说就是保留双方的历史记录，但<font color=FF0000>完全忽略掉这一方的文件变更</font>（**注：**“这一方”是谁，是根据策略是 Our，还是 Theirs 决定的）。使用Our，则抛弃被合并的分支，使用 Theirs 则抛弃当前（可以立即为 master）分支

- **Octopus：**一般来说我们的合并节点都只有两个parent（即合并两条分支），而这种合并策略可以做两个以上分支的合并，这也是git merge两个以上分支时的默认行为。

**注：**以上内容经过一定的省略和整理，存在不理解的可以参见原文

以上内容摘自：[这才是真正的GIT——分支合并](https://www.lzane.com/tech/git-merge)



#### 更新一个文件的内容这个过程会发生什么事

原本的git状态：

<img src="https://s2.loli.net/2022/02/25/56tHkLrMvQ13JdG.png" alt="https://www.lzane.com/tech/git-internal/3area.png" style="zoom: 67%;" />

运行 `echo "333" > a.txt` 将 a.txt 的内容从 111 修改成 333，此时如下图可以看到，此时索引区域和git仓库没有任何变化。

<img src="https://s2.loli.net/2022/02/26/blek1d8GDrMN3RE.gif" alt="https://www.lzane.com/tech/git-internal/p2s1.gif" style="zoom:77%;" />

运行 `git add a.txt` 将 a.txt 加入到索引区域，此时如下图所示 ，git 在仓库里面新建了一个 blob object，储存了新的文件内容。并且更新了索引将 a.txt 指向了新建的 blob object。

<img src="https://s2.loli.net/2022/02/26/Kwxi7AShH9NLVFz.gif" alt="https://www.lzane.com/tech/git-internal/p2s2.gif" style="zoom:77%;" />

运行git commit -m 'update'提交这次修改。如下图所示：

<img src="https://s2.loli.net/2022/02/26/mlVvn9YJAP3N7H5.gif" alt="https://www.lzane.com/tech/git-internal/p2s3.gif" style="zoom:77%;" />

- Git 首先根据当前的索引生产一个 tree object  ，充当新提交的一个快照。
- 创建一个新的 commit object，将这次 commit 的信息储存起来，并且 parent 指向上一个 commit，组成一条链记录变更历史。
- 将 master 分支的指针移到新的 commit 结点。

摘自：[这才是真正的GIT——GIT内部原理](https://www.lzane.com/tech/git-internal/)



#### 一些 git 原理性的问题

- **为什么要把文件的权限和文件名储存在Tree object里面而不是Blob object呢？**

  想象一下修改一个文件的命名。如果将文件名保存在blob里面，那么Git 只能多复制一份原始内容形成一个新的blob object。而Git的实现方法只需要创建一个新的tree object将对应的文件名更改成新的即可，原本的blob object可以复用，节约了空间。

  **理解：**创建一个 blob 可能会很大，而 创建一个 tree 只需要一些索引（指针？）即可；相对而言（根据数学期望而言？），应该是创建 tree 更节约空间。

- **每次commit，Git储存的是全新的文件快照还是储存文件的变更部分？**

  由上面的例子我们可以看到，<font color=FF0000 size=4>**Git储存的是全新的文件快照，而不是文件的变更记录**</font>。也就是说，就算你只是在文件中添加一行，Git也会新建一个全新的blob object。那这样子是不是很浪费空间呢？

  这其实是Git在空间和时间上的一个取舍（**注：**即用空间换时间），思考一下你<mark>要 checkout 一个 commit，或对比两个commit 之间的差异</mark>。<font color=FF0000>如果Git储存的是问卷的变更部分，那么为了拿到一个commit的内容，Git都只能从第一个commit开始，然后一直计算变更，直到目标commit，这会花费很长时间</font>。而相反，Git采用的储存全新文件快照的方法能使这个操作变得很快，直接从快照里面拿取内容就行了。

  当然，<font color=FF0000>**在涉及网络传输或者Git仓库真的体积很大的时候，Git会有垃圾回收机制gc，不仅会清除无用的object，还会把已有的相似object打包压缩**</font>。

- **Git怎么保证历史记录不可篡改？**

  <font color=FF0000>通过SHA1哈希算法和哈系树来保证</font>。<mark>假设你偷偷修改了历史变更记录上一个文件的内容，那么这个问卷的blob object的SHA1哈希值就变了，与之相关的tree object的SHA1也需要改变，commit的SHA1也要变，这个commit之后的所有commit SHA1值也要跟着改变</mark>。又<font color=FF0000>由于Git是分布式系统，即所有人都有一份完整历史的Git仓库，所以所有人都能很轻松的发现存在问题</font>。

摘自：[这才是真正的GIT——GIT内部原理](https://www.lzane.com/tech/git-internal/)



#### `@commitlint/config-conventional` type 说明

| type     | 含义                                   |
| -------- | -------------------------------------- |
| feat     | 新功能                                 |
| fix      | 修复 bug                               |
| docs     | 修改文档                               |
| style    | 代码格式修改                           |
| refactor | 重构（即不是新增功能，也不是修复 bug） |
| perf     | 更改代码以提高性能                     |
| test     | 增加测试                               |
| build    | 构建过程或辅助工具的变动               |
| ci       | 修改项目持续集成流程                   |
| chore    | 其他类型的提交                         |
| revert   | 恢复上一次提交                         |

摘自：[git 奇淫巧技](https://www.yuque.com/docs/share/946f5537-0539-451a-aa50-1a41c2c0f12f)
