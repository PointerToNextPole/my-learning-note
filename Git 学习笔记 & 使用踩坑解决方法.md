# Git 学习笔记 & 使用踩坑解决方法

#### 所有关于git的内容都可以参考

https://git-scm.com/book/zh/v2

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

#### From 1.5

```bash
$ git add -u # 对git已经跟踪了的文件，将其一起提交到暂存区
```

另外，本讲 讲了“Git 的工作区、暂存区和版本库”；由于第一次没有记录，第二次重学时，下面已经记录了，这里略。

#### From 1.6

```bash
git add file_name  # 将文件加入git管理
git rm file_name  # 将文件从git管理中移除（注：就是 git add 的逆向操作）

git mv orgin_file_name target_file_name # 使用 git 将原始文件改名为目标文件名，类似于先 mv 再 git add 
```

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
git branch -v  # 查看本地有多少分支
git checkout -b branchName commitHash # 创建一个新的分支，并**切换到新分支上**。其中：branchName 为分支名，commitHash 为提交的SHA hash（即：Secure Hash Algorithm hash）；表示根据「哪一个提交」创建一个分支。另外，commitHash 可以通过 git log 查到。
# 上面 hash 相关的内容了解自：careerkarma.com/blog/git-log
# TODO 这里 -b 的作用虽然可以猜到一些，但是还是要查一下...
```

可以使用 git log branchName，查看某一个分支的日志。同样，也可以使用 git log --all 命令，查看所有分支的信日志。如下所示，输入 git log --all，会输出所有的（共两个）分支，master 和 testBranch：

<img src="https://s2.loli.net/2022/02/19/DaPWZ8uFi7B5voy.png" alt="image-20220219222624280" style="zoom:50%;" />

另外，由于上面的 git log --all 在很多分支的情况下，看起来很累，可以加上 --graph 参数，使得以图形化的方式展现；可以看出分支之间的关系：

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

  它当前指向的分支。即：HEAD 文件记录的是：当前工作在哪一个分支上。

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

    运行命令 `git cat-file -p hashVal` 可以打印出对应的内容（-p 参数就是查看对应内容的），这些信息包含 tag 的指向（指向的是 object）。如下：

    <img src="https://s2.loli.net/2022/02/20/KQcgz9PkRY7xuGB.png" alt="image-20220220162811336" style="zoom:50%;" />

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

#### From 1.10

commit、tree、blob 三者之间的关系

![image-20220220174718938](https://s2.loli.net/2022/02/20/TkZIqPmWVj5lv2o.png)

- **commit**

  每一次执行 `git commit` 操作都会创建一个 commit 对象。

  一个 commit 对应一棵树 tree（如图上 commit 部分（蓝色部分）tree 键值对所示），这棵树代表了：当前 commit 的视图，视图中存放了一个快照，快照中存放了当前 commit 对应的本项目仓库的所有文件的快照。即：在每次提交中，这个项目长什么样就是通过这棵树展示出来

- **tree**

  tree 就是文件夹，比如上图 tree 的部分（黄色部分），tree 中也可以包含 tree（即：子文件夹），如 tree 中的 images 文件夹上，键值对 就是显示的类型为 tree；同时，下面也展示了 images 文件夹的 tree。

- **blob**

  blob 与文件名没有关系，只要文件内容是一样的，则 git 会认为这是同一个文件；git 也只会存一份，可以大大节约存储空间。

这里说明了 commit、tree、blob 之间的关系，至于上面 使用 `git cat-file -t` 和 `git cat-file -p` 用的有点懵，对文件结构不了解；看完了这个，清楚了很多。

#### From 1.11

根据实验表明：commit、tree、blob 三者都会被加入到 objects 文件夹下。如下文件结构

![image-20220220184824301](https://s2.loli.net/2022/02/20/6DAd1YGfOB9vJ7H.png)

通过如下命令 `find .git/objects -type f` ，打印出的结果为：

![image-20220220185214559](https://s2.loli.net/2022/02/20/5IYaEUyPr7lNDSt.png)

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

提示中也提及了，保存分离头指针为一个分支的方法：`git branch branchName detachedHEADHash`

#### From 1.12

`git diff commitHash1 comitHash2`：比较两个 commit 之间的区别。

另外，这里的 commitHash 可以用 HEAD 进行指代，同时也可以用 HEAD 进行其他指代，比如：HEAD\^ 表示HEAD 的父亲，同理：HEAD\^\^1 可以表示为 HEAD 父亲的父亲  

#### // TODO 这里听的有点懵，搜一下 ^ ~ 的用法。

#### From 2.1

删除分支，使用 `git branch -d branchName` ，这里要补充下 git branch -d 和 -D 的区别：

#### // TODO

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

课程在这里提了一下 `git stash` 命令，作用是：将修改的内容保存至堆栈区，以方便你临时切出去修改其他东西之后再回来，修改内容依然存在。

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

<font color=FF0000 size=4>开发中临时加塞了紧急任务，该如何处理？</font>即，对代码做了修改；但是突然来的任务不需要这里的修改，需要切出去；这时候需要使用 `git stash` 命令。在使用 `git stash` 命令之后，可以使用 `git stash list` 查看 stash 列表。如下：

<img src="https://s2.loli.net/2022/02/21/Ohz13RoFg9fqUVs.png" alt="image-20220221210139366" style="zoom:50%;" />

在处理完任务后，可以使用 `git stash pop` 或者 `git stash apply` 命令进行恢复 stash

- `git stash apply`：环境将会恢复，stash 中的数据，将不会被清除。
- `git stash pop`：环境会恢复，stash 中的数据将会被清理

#### From 2.15

<font color=FF0000 size=4>使用 .gitignore文件</font>，一般格式为：

- **[*.]\<file>[.ext]：**忽略的文件
- **foder/：**忽略文件夹

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

在使用 `git clone` 命令时，可以加上 `--bare` 参数，表示clone的仓库不带工作区；以后在 push 时可以方便些

这里还提及了 `git remote` 相关的 命令，是将本地 git 和 远端的git 仓库建立联系的命令。下面有说，这里略



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

使用命令查看本地git安装地址

```bash
where git
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

---



***

#### 用户名和邮箱

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

**需要使用参数 **`--replace-all`

```bash
$  git config --global --replace-all user.email "输入你的邮箱" 
$  git config --global --replace-all user.name "输入你的用户名"
```

**补充：查看git配置列表**

```bash
$  git config --list
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

  

#### Git 工作区、暂存区和版本库

- **工作区：**就是你<mark>在电脑里能看到的目录</mark>。
- **暂存区：**英文叫stage, 或index。<mark>一般存放在 ".git目录下" 下的index文件（.git/index）中</mark>，所以我们把暂存区有时也叫作索引（index）。（注：将文件从工作区到暂存区，通过 `git add` ）
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
> 摘自：[git push -u的含义和用法](https://blog.csdn.net/chenzz444/article/details/104408607) 的评论区

<font size=4>**其他 git remote 命令**</font>

- **git remove -v：**显示所有远程仓库
- **git remote show [remote]：**显示某个远程仓库的信息
- **git remote rm name：**删除远程仓库
- **git remote rename old_name new_name：**修改仓库名

​	以上摘自：[RUNOOB - git remote 命令](https://www.runoob.com/git/git-remote.html)



#### git pull 和 git merge的区别

**git pull = git fetch + git merge**

- fetch 和 push 可以分别对远程分支进行 fetch 和 push 操作

- pull 不是直接跟远程分支对话的

**fetch 和 pull的区别在于：**

- **git fetch：**是从远程获取最新版本到本地，不会自动 merge
- **git pull：**是从远程获取最新版本并 merge 到本地仓库

从安全角度出发，git fetch比git pull更安全，因为我们可以先比较本地与远程的区别后，选择性的合并。

git push 默认推送到 master

摘自：[segmentfault Salamander的回答 -- git pull和git merge 区别? ](https://segmentfault.com/q/1010000009076820)



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

- **Merge 分支：**<font color=FF0000 size=4>Merge 分支是**为了可以随时发布 release 而创建的分支**</font>，它<font color=FF0000 size=4>还能作为 Topic 分支的 **源分支** 使用</font>（注：下面的 Topic 分支 中也有提及）。<font color=FF0000>保持分支稳定的状态是很重要的，**如果要进行更改，通常先创建 Topic 分支**</font>（**注：**感觉可以理解为，使用更小的粒度进行操作）；而针对该分支，可以使用 Jenkins 之类的 CI 工具进行自动化编译以及测试。

  通常，大家会将 master 分支当作 Merge 分支使用。

- **Topic 分支：**<font color=FF0000 size=4>Topic 分支**是为了开发新功能或修复 Bug 等任务而建立的分支**</font>（**注：**通过 Topic 的名字，就可以看出该分支的意思和目的，方便理解和记忆 ）。若<font color=FF0000>要同时进行多个的任务，请创建多个的 Topic 分支</font>（注：和上面的类似，使用更小粒度的同时，粒度之间最好是解耦的）。

  <font color=FF0000 size=4>Topic 分支**是从稳定的 Merge 分支创建的**</font>。<font color=FF0000>完成作业后，要把 Topic 分支 <font color=FF0000 size=4>**合并回**</font> Merge 分支</font>。







