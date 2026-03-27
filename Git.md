# Git

**在已经安装好和配置好git的前提下，就可以直接开始下面的学习了**

## 创建版本库

如果我们当前是在本地进行一系列的操作，还没有涉及到远程仓库的操作，那么我们需要先创建版本库

**那么什么是版本库呢？**
就是.git文件，这个目录里面的所有的文件都被git管理，每个文件额增删改都能被git发现，还可以通过git进行还原，后面会提到

---

### 创建版本库/创建.git文件并且纳入git管理

根据你的需求，在对应的位置去打开gitbash
我在c盘下打开gitbash，那么执行`mkdir git-study`命令之后就会发现多了个文件夹`git-study`，然后执行`cd git-study`命令切换到该文件夹目录下，此时文件夹里面是什么都没有的，因为还没有纳入git的管理

~~~bash
mkdir git-study
cd git-study

# 可以查看当前所处的目录
pwd
~~~



由于新创建的文件夹是不受git管理的，所以要执行`git init`操作来纳入git的管理，执行完之后会发现多了一个.git文件，这个.git文件就是`版本库`，那么当前所处的这个文件夹下，即和.git所处的同一个文件夹下的所有文件都会受git的管理，变成了git可管理的仓库

**注意：**

- .git文件不要碰，不要修改，修改了这个git仓库就废了
- 当我们纳入git管理之后，git默认会自动生成一个分支master，这个分支是主分支，然后有一个指向master的指针HEAD

~~~bash
git init

# 有可能你的.git文件被隐藏了，可以执行下面这个指令去查看
ls -ah
~~~

---

### 实操练习

当上面的步骤都实现之后，我们可以创建几个文件，对这些文件进行增删改进行一下实操

我们可以手动去新建文件，也可以通过git指令`touch 文件名`来新建文件，然后对你自己生成的文件进行对应的编写，就可以体现出效果了

编写结束之后，我们要通过`git add 指定文件名`或者`git add .`把指定文件或者所有文件都添加到暂存区中，然后通过`git commit`操作从暂存区中提取文件提交到git本地仓库中

~~~bash
# touch 文件名
touch file.md

# 把指定文件file.md添加到暂存区中
# 把所有文件都添加到暂存区中 : git add .
git add file.md

# 把暂存区的文件提交到git本地仓库中
git commit -m "写注释的，每次commit之后查看log都会显示，所以要见名知意"
~~~

> 当commit结束之后，git会给出下面提示：
>
> `file changed`:多少个文件被修改了​
>
> `insertions`:插入了多少行内容
>
> `deletions`:删除了多少行内容
>
> ~~~bash
> [master (root-commit) 454c87c] 第一次提交
> 1 file changed, 3 insertions(+)
> create mode 100644 file.md
> ~~~

---

#### 对文件进行增删改操作

我们可以对文件进行增删改操作，进一步体现git的功能
因为每一次修改完文件之后我们都要重新提交到git本地仓库，才能成功修改，否则是查询不到的

~~~bash
# 查看当前git本地仓库的状态，当前所处的分支、哪些文件未提交，无法查看文件具体的修改内容
git status

# 可以查看文件具体的修改内容，绿色的内容就是insertions的内容，红色的内容就是deletions的内容
git diff file.md

#修改完之后还是一样的操作
git add file.md
git commit -m "写上对应的注释"
~~~



----

## 版本回退

在我们对文件进行增删改操作的时候，每修改到一定程度就会进行一次`commit`操作，如果发现某个文件乱了，或者修改错误了，我们就可以通过回退操作来放回到最近的`commit`进行恢复，不需要从头开始重新操作，就像打有存档的游戏一下，打boss失败了会回到最近的存档位置重新开始，所以这就印证了前面在`commit`的时候写注释是有必要的，能够清晰标记每一个文件或者每一个位置对应的内容

### 实操

假如我对我自己的file.md文件进行多次操作

> **注意**
>
> - 我们可以通过`git log`指令来查看我们所有的commit操作，还可以在log后面加上对应的参数来限制展示的值，具体参数自己网上查一查
>
> - commit后面的黄色那一串是commit id
> - HEAD：表示当前所处的版本，也就是最新提交的版本，即当前处于第四次提交之后的版本
> - HEAD^：上一个版本
> - HEAD^^：上上个版本
> - HEAD^100：上100个版本
>
> > **在回退的时候假如不同的参数是回退到对应版本的不同状态的，自己通过`git status`和`cat file.md`指令查看一下就懂了**
> >
> > --hard：回到指定回退版本的已提交状态
> >
> > --soft：回到指定回退版本的未提交状态
> >
> > --mixed：回到指定回退版本的已添加但未提交的状态
>
> 
>
> 假如我进行了四次的commit
>
> 
>
<img width="1127" height="640" alt="image" src="https://github.com/user-attachments/assets/7cf22185-cf68-40ed-85de-966fe2f88d44" />

**如果回退之后又想回去怎么办呢?**

- 如果你的gitbash这个命令行窗口没有关闭的话可以找到想穿越回去的版本的commit id进行回退，只需要输入commit的id的前四或五位即可

  ~~~bash
  git reset --hard c3c5b
  ~~~



git的版本回退是很快的，所以不用担心性能的问题，因为git内部是有HEAD指针指向当前所处版本的，每次回退只需要改变HEAD指针的指向即可，文件的内容没有进行任何的改变

~~~bash
┌────┐
│HEAD│
└────┘
   │
   └──▶ ○ 第四次commit
        │
        ○ 第三次commit
        │
        ○ 第二次commit
        │
        ○ 第一次commit
        
        进行回退，假如回退到第三次提交
        
┌────┐
│HEAD│
└────┘
   │
   │    ○ 第四次commit
   │    │
   └──▶ ○ 第三次commit
        │
        ○ 第二次commit
        │
        ○ 第一次commit
~~~



- 但是如果关闭了gitbash窗口或者电脑宕机了、重启了，还想穿越回去怎么办呢？

  ~~~bash
  # 通过下面指令就可以展示所有的commit id
  git reflog
  
  # 然后接下来的操作就是和和没有关闭gitbash进行穿越的操作一样
  git reset --hard commitId
  ~~~



---

### 工作区和版本库和暂存区

**工作区（working directory）：**
电脑看见的目录就是工作区，例如git-study文件夹就是工作区

**版本库（Repository）：**
git-study文件夹里面的.git文件就是git的版本库

**暂存区：**
.git文件里面的stage/index文件就叫暂存区

首先在工作区创建文件，对文件进行增删改操作，然后指向`git add`操作，文件就会被添加到版本库中的暂存区中，然后执行`git commit`指令，git就会从暂存区中拉取文件提交到当前所处的分支上，没有拉取分支的话当前所处的分支就是master分支

---

### 管理修改

前置：知道什么是工作区和暂存区

**git的管理是管理修改，即git管理的是文件的增删改操作，而不是管理文件**

==例如==
我对file.md文件进行了一次修改，然后`git add`但是没有进行`git commit`，所以此时这一次操作的文件被添加到了暂存区中，然后对file.md文件进行第二次修改，然后直接`git commit`，会发现，第二次修改的没有被提交，第一次修改的被提交了，是因为第一次修改的被添加到了缓存区中，但是第二次修改的没有被添加到缓存区中，而`git commit`命令是从暂存区中拉取文件进行提交到git仓库的，如果想把第二次的修改也提交，那只需要再执行一次`git add`然后再执行`git commit`即可完成

所以说明了：git管理的是文件的修改，而不是管理文件，如果管理文件的话第二次的修改也一样被提交的

---

### 撤销修改

如果在修改file.md文件的时候，发现有错误，我们可以手动区对这个文件包的错误位置进行修改，也可以通过git命令进行操作

- 如果修改后还没有添加到暂存区，就回到和版本库的一模一样，因为还没有添加到暂存区，说明此时版本库中的文件是上一次commit的，所有就是文件在工作区中进行的修改全部撤销
- 如果修改后文件已经添加到暂存区了，又做了修改，现在，撤销修改就回到添加到暂存区后的状态

==综上：其实无论是那种情况，只要是撤销就是放回到最近的一次commit或者add的状态==

~~~Bash
git checkout -- file.md
~~~

那如果文件被添加到了暂存区之后没有进行了修改，也没事，一样可以撤销
和上面的指令的效果是一样的

只要没有推送到远程仓库就没事

~~~bash
git reset HEAD 文件名
~~~

---

### 删除文件

可以手动删除，可以通过git指令删除

删除之后git知道你删除了文件，因此会有提示信息说：工作区和版本库的不一致，因为先删除的是工作区，`git status`指令会说哪些文件被删除了

- 如果你确定要删除，确定要从版本库中删除该文件，那么就执行	`git rm`命令删除， 并且`git commit`进行提交，就成功从版本库中也删除了，那么工作区和版本库就同步了
  - 如果是误触，删错了，因为版本库中还有这个文件，只是在工作区中删除了而已，所以可以通过`git checkout -- file.md`进行恢复

~~~bash
# 删除指定文件
rm file.md

# 恢复工作区中已删除的文件
git checkout -- 文件名

# 确定要删除，从版本库中也要进行删除
git rm
git commit
~~~

---

## 远程操作

上面讲的所有的操作都是基于本地git仓库来进行的一系列操作，但是远程仓库的操作更能体现git的强大

### 创建远程仓库

1. 首先在git中创建ssh key，因为git本地仓库和github远程仓库是通过ssh密钥来进行加密的，在自己电脑的主目录下查看是否有.ssh文件，如果有就点进去看是否有id_rsa文件和id_rsa.pub文件，如果有，直接跳过这个步骤去创建github远程仓库的ssh密钥即可，如果没有，那就执行`ssh-keygen -t rsa -C "youremail@example.com"`命令去进行创建，不懂得b站找找看，一直回车直到出现类似栈的图形化界面即成功，密码可以不设置，不重要

   ~~~bash
   ssh-keygen -t rsa -C "youremail@example.com"
   ~~~

   

2. 然后登录github，settings中的SSH和GPG keys中进行添加SSH密钥即可。title随意，然后把id_rsa.pub文件的内容粘贴到key文本中即可

---

### 添加github远程仓库

就直接在github上创建即可，但是注意如果是已经有了本地git仓库，然后再去创建github远程仓库的话，最好不要初始化的时候创建README.md文件，否则在推送去远程仓库的时候会报错，如果是先有远程仓库，然后clone到本地仓库的话无所谓

让本地git仓库和github远程仓库产生关联

origin是默认的远程仓库的名字，可以修改，但是无所谓

~~~bash
git remote add origin https://github.com/JunLa-0/git-study.git
或者
git remote add origin git@github.com:JunLa-0/git-study.git
# 推荐使用下面这种
~~~

把本地git仓库的内容推送到远程仓库

~~~bash
git push -u origin master
~~~



实际上是把当前分支master推送到远程，因为默认情况下git会自动给我们生成一个master分支，这个是主分支

第一次推送的时候远程仓库是空的，第一次推送master分支时需要加上-u参数，git不但会把本地的master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或拉去时就可以简化命令了

如果推送异常的话，可能是你在创建仓库的同时生成了ReadMe.md这个文件，就会导致在远程仓库中有得文件但是本地仓库没有，就会报错，你要么强制提交，要么先拉取再提交

那么后续本地仓库进行了一系列的修改之后，只要本地的文件进行了提交，就可以通过命令：

~~~bash
# 进行提交
# 把本地的master分支的最新修改推送到GitHub
git push origin master
~~~



==SSH警告==
如果你是第一次使用Git的clone或push命令去连接github时，可能会得到一个错误
输入yes回车即可

~~~bash
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
~~~



然后就会出现一个警告

~~~bash
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
~~~



这个警告只会出现一次，这个警告就是告诉你已经把github的key添加到本机的一个信任列表里了

---

### 删除远程仓库

如果你在添加github远程仓库的时候地址写错了，或者想删除github远程仓库，可以使用下面指令，但是建议先用`git remote -v`去查看远程库的信息

~~~bash
# 查看github远程仓库的信息
git remote -v

# 然后根据github远程仓库的名字进行删除即可
# 注意：这个删除不是物理意义上的删除远程仓库，而是解除本地git仓库和github远程仓库的绑定关系，github远程仓库时没有被删除的
# 如果需要删除github远程仓库的话需要登录github，然后进行删除远程仓库
git remote rm origin
~~~



---

### 从远程仓库进行克隆

前面我们讲过先有本地git仓库，然后再有远程仓库的时候，如何关联远程仓库

那么现在是：从零开发，最好的方式是现有远程仓库，然后从远程仓库clone

那么我们可以重新创建一个远程仓库来进行操作，我们在创建远程仓库的时候要勾选上生成README.md文件，然后进行创建，那么创建成功之后

那么下一步就是使用命令`git clone`去clone一个本地仓库

根据你想要clone到的位置，在该位置打开gitbash输入这个指令即可，然后就会出现一个git-skills文件夹，那么就成功了

~~~bash
git clone git@github.com:JunLa-0/git-skills.git
~~~



然后切换到该文件夹目录下即可进行上面所述的全部操作

~~~Bash
# 切换到git-skills目录
cd git-skills
~~~



所以这个操作就很方便，你想想，你往github上开源一个项目，别人想参与，那么只需要通过git指令直接就能clone到它自己的电脑上进行开发

---

## 分支管理

**不同分支之间是没有影响的，所以当出现bug或者需要修改功能、需要新添加功能的时候可以通过拉取新分支操作来进行**

---

### 创建与合并分支

如果在不拉取分支的情况下，git本地仓库会自己生成一个分支，默认是master分支，或者从github远程仓库上拉取下来的项目默认是main分支，但是都是只有一个分支，前面也讲过分支是根据时间线串成一条的

~~~
                  HEAD
                    │
                    ▼
                 master
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
例如
这里就只有一个分支master，然后有一个指针HEAD，这个指针HEAD永远是指向当前分支的，下面的方形就表示一次次的提交/版本，master是指向提交的
只要有一个新的提交，master就会往下移动一步，但是HEAD指针不一定，如果有多个分支的话，所以master的分支会越来越长
假设我们在当前的情况下新创建了一个分支，假设叫dev分支，会指向master相同的提交，然后再把HEAD指向dev这个分支，就表示当前分支再dev上
                 master
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘
                    ▲
                    │
                   dev
                    ▲
                    │
                  HEAD
git创建一个分支是很快的，因为除了增加一个dev指针，只需要改一下HEAD的指向即可，工作区的文件没有发送任何的变化
但是由于现在HEAD指向了dev分支，那么如果接下来我们对工作区的文件进行修改和提交的话，是针对dev分支进行操作的，那么新的一次提交dev分支会往下移动一步，但是master分支不会动
                 master
                    │
                    ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                            dev
                             ▲
                             │
                            HEAD
                            
如果我们在dev分支上的工作完成了， 那么我们就可以把dev分支合并到master分支上，然后可以把dev分支给删除掉
那么dev和master分支就会同步到同一个提交的位置
                           HEAD
                             │
                             ▼
                          master
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘
                             ▲
                             │
                            dev
git的合并也是很快的，只需要修改一下指针的指向即可，工作区的内容依然是不会发生变化的
如果给dev分支删除的话就只剩下master一条分支了
                           HEAD
                             │
                             ▼
                          master
                             │
                             ▼
┌───┐    ┌───┐    ┌───┐    ┌───┐
│   │───▶│   │───▶│   │───▶│   │
└───┘    └───┘    └───┘    └───┘

~~~



==实操一下==

~~~bash
# 创建dev分支并且从当前分支切换到dev分支上
# 那么此时HEAD就会指向dev分支
# 如果不想一条指令执行的话，可以拆分成两条指令
# git branch dev
# git checkout dev
git checkout -b dev

# 查看当前分支
# 绿色的就是表示当前所处的分支，前面还会带一个*
git branch

# 我们可以对文件进行修改
git add file.md
git commit -m "分支dev"

# 假设在dev下的工作已经完成了，那么就可以合并分支了，同步master
# 注意，此时操作完命令之后只是把分支切换回了master，但是还没有同步操作，所以master并不能查看到新更新的内容，
git checkout master

# 进行同步
git merge dev

# 删除无用分支
git branch -d dev
~~~



==switch==
其实使用switch来进行切换分支更科学一点

~~~bash
# 创建新分支dev，并且切换到dev分支上
git switch -c dev

# 直接切换到master分支，前提是这个分支得存在
git switch master
~~~

----

### 解决冲突

人生不如意之事十之八九，合并分支往往也不是一帆风顺的。

准备新的`feature1`分支，继续我们的新分支开发：

```plain
$ git switch -c feature1
Switched to a new branch 'feature1'
```



修改`readme.txt`最后一行，改为：

```plain
Creating a new branch is quick AND simple.
```



在`feature1`分支上提交：

```plain
$ git add readme.txt

$ git commit -m "AND simple"
[feature1 14096d0] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```



切换到`master`分支：

```plain
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```



Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。

在`master`分支上把`readme.txt`文件的最后一行改为：

```plain
Creating a new branch is quick & simple.
```



提交：

```plain
$ git add readme.txt 
$ git commit -m "& simple"
[master 5dc6824] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```



现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

```
                            HEAD
                              │
                              ▼
                           master
                              │
                              ▼
                            ┌───┐
                         ┌─▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘
│   │───▶│   │───▶│   │──┤
└───┘    └───┘    └───┘  │  ┌───┐
                         └─▶│   │
                            └───┘
                              ▲
                              │
                          feature1
```

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```bash
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```



果然冲突了！Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```



我们可以直接查看readme.txt的内容：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```



Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
```



再提交：

```bash
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```



现在，`master`分支和`feature1`分支变成了下图所示：

```
                                     HEAD
                                       │
                                       ▼
                                    master
                                       │
                                       ▼
                            ┌───┐    ┌───┐
                         ┌─▶│   │───▶│   │
┌───┐    ┌───┐    ┌───┐  │  └───┘    └───┘
│   │───▶│   │───▶│   │──┤             ▲
└───┘    └───┘    └───┘  │  ┌───┐      │
                         └─▶│   │──────┘
                            └───┘
                              ▲
                              │
                          feature1
```

用带参数的`git log`也可以看到分支的合并情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```



最后，删除`feature1`分支：

```bash
$ git branch -d feature1
Deleted branch feature1 (was 14096d0).
```

**小结**

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

-----

### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```plain
$ git switch -c dev
Switched to a new branch 'dev'
```



修改readme.txt文件，并提交一个新的commit：

```plain
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```



现在，我们切换回`master`：

```plain
$ git switch master
Switched to branch 'master'
```



准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```plain
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```



因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```



可以看到，不使用`Fast forward`模式，merge后就像这样：

```
                                 HEAD
                                  │
                                  ▼
                                master
                                  │
                                  ▼
                                ┌───┐
                         ┌─────▶│   │
┌───┐    ┌───┐    ┌───┐  │      └───┘
│   │───▶│   │───▶│   │──┤        ▲
└───┘    └───┘    └───┘  │  ┌───┐ │
                         └─▶│   │─┘
                            └───┘
                              ▲
                              │
                             dev
```

**分支策略**

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://liaoxuefeng.com/books/git/branch/policy/branches.png)

**小结**

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

----

### BUG分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```plain
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```



并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```plain
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```



现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```plain
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```



现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```plain
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```



修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```plain
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```



太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```plain
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```



工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```plain
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```



工作现场还在，Git把`stash`内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把`stash`内容也删了：

```plain
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```



再用`git stash list`查看，就看不到任何`stash`内容了：

```plain
$ git stash list
```



你可以多次`stash`，恢复的时候，先用`git stash list`查看，然后恢复指定的`stash`，用命令：

```plain
$ git stash apply stash@{0}
```

在`master`分支上修复了bug后，我们要想一想，`dev`分支是早期从`master`分支分出来的，所以，这个bug其实在当前`dev`分支上也存在。

那怎么在`dev`分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在`dev`上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到`dev`分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个`master`分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```plain
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```



Git自动给`dev`分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于`master`的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在`dev`分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在`master`分支上修复bug后，在`dev`分支上可以“重放”这个修复过程，那么直接在`dev`分支上修复bug，然后在`master`分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从`dev`分支切换到`master`分支。

**小结**

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在`master`分支上修复的bug，想要合并到当前`dev`分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

----

### Feature分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。

于是准备开发：

```plain
$ git switch -c feature-vulcan
Switched to a new branch 'feature-vulcan'
```



5分钟后，开发完毕：

```plain
$ git add vulcan.py

$ git status
On branch feature-vulcan
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   vulcan.py

$ git commit -m "add feature vulcan"
[feature-vulcan 287773e] add feature vulcan
 1 file changed, 2 insertions(+)
 create mode 100644 vulcan.py
```



切回`dev`，准备合并：

```plain
$ git switch dev
```



一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是！

就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```plain
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```



销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

```plain
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```



终于删除成功！

开发一个新功能，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

-----

### 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```plain
$ git remote
origin
```



或者，用`git remote -v`显示更详细的信息：

```plain
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```



上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```plain
$ git push origin master
```



如果要推送其他分支，比如`dev`，就改成：

```plain
$ git push origin dev
```



但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

#### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```plain
$ git clone git@github.com:michaelliao/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 40, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
Receiving objects: 100% (40/40), done.
Resolving deltas: 100% (14/14), done.
```



当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

```plain
$ git branch
* master
```



现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```plain
$ git checkout -b dev origin/dev
```



现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```plain
$ git add env.txt

$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   f52c633..7a5e5dd  dev -> dev
```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```plain
$ cat env.txt
env

$ git add env.txt

$ git commit -m "add new env"
[dev 7bd91f1] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```



推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```plain
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```



`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```plain
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```



再pull：

```plain
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```



这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](https://liaoxuefeng.com/books/git/branch/merge/index.html)完全一样。解决后，提交，再push：

```plain
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

因此，多人协作的工作模式通常是这样：

1. 首先，可以尝试用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

**小结**

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

-----

### Rebase

在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。

每次合并再push后，分支变成了这样：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
* d1be385 (HEAD -> master, origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
| |/  
* |   12a631b merged bug fix 101
|\ \  
| * | 4c805e2 fix bug 101
|/ /  
* |   e1e9c68 merge with no-ff
|\ \  
| |/  
| * f52c633 add merge
|/  
*   cf810e4 conflict fixed
```



总之看上去很乱，有强迫症的童鞋会问：为什么Git的提交历史不能是一条干净的直线？

其实是可以做到的！

Git有一种称为`rebase`的操作，有人把它翻译成“变基”。

先不要随意展开想象。我们还是从实际问题出发，看看怎么把分叉的提交变成直线。

在和远程分支同步后，我们对`hello.py`这个文件做了两次提交。用`git log`命令看看：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
* 582d922 (HEAD -> master) add author
* 8875536 add comment
* d1be385 (origin/master) init hello
*   e5e69f1 Merge branch 'dev'
|\  
| *   57c53ab (origin/dev, dev) fix env conflict
| |\  
| | * 7a5e5dd add env
| * | 7bd91f1 add new env
...
```



注意到Git用`(HEAD -> master)`和`(origin/master)`标识出当前分支的HEAD和远程origin的位置分别是`582d922 add author`和`d1be385 init hello`，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```plain
$ git push origin master
To github.com:michaelliao/learngit.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```



很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先pull一下：

```plain
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
   d1be385..f005ed4  master     -> origin/master
 * [new tag]         v1.0       -> v1.0
Auto-merging hello.py
Merge made by the 'recursive' strategy.
 hello.py | 1 +
 1 file changed, 1 insertion(+)
```



再用`git status`看看状态：

```plain
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```



加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

用`git log`看看：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
|\  
| * f005ed4 (origin/master) set exit=1
* | 582d922 add author
* | 8875536 add comment
|/  
* d1be385 init hello
...
```



对强迫症童鞋来说，现在事情有点不对头，提交历史分叉了。如果现在把本地分支push到远程，有没有问题？

有！

什么问题？

不好看！

有没有解决方法？

有！

这个时候，rebase就派上了用场。我们输入命令`git rebase`试试：

```plain
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: add comment
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
Applying: add author
Using index info to reconstruct a base tree...
M	hello.py
Falling back to patching base and 3-way merge...
Auto-merging hello.py
```



输出了一大堆操作，到底是啥效果？再用`git log`看看：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master) add author
* 3611cfe add comment
* f005ed4 (origin/master) set exit=1
* d1be385 init hello
...
```



原本分叉的提交现在变成一条直线了！这种神奇的操作是怎么实现的？其实原理非常简单。我们注意观察，发现Git把我们本地的提交“挪动”了位置，放到了`f005ed4 (origin/master) set exit=1`之后，这样，整个提交历史就成了一条直线。rebase操作前后，最终的提交内容是一致的，但是，我们本地的commit修改内容已经变化了，它们的修改不再基于`d1be385 init hello`，而是基于`f005ed4 (origin/master) set exit=1`，但最后的提交`7e61ed4`内容是一致的。

这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过push操作把本地分支推送到远程：

```plain
Mac:~/learngit michael$ git push origin master
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To github.com:michaelliao/learngit.git
   f005ed4..7e61ed4  master -> master
```



再用`git log`看看效果：

```plain
$ git log --graph --pretty=oneline --abbrev-commit
* 7e61ed4 (HEAD -> master, origin/master) add author
* 3611cfe add comment
* f005ed4 set exit=1
* d1be385 init hello
...
```



远程分支的提交历史也是一条直线。

-----

## 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

------

### 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```plain
$ git branch
* dev
  master
$ git checkout master
Switched to branch 'master'
```



然后，敲命令`git tag <name>`就可以打一个新标签：

```plain
$ git tag v1.0
```



可以用命令`git tag`查看所有标签：

```plain
$ git tag
v1.0
```



默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```plain
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```



比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```plain
$ git tag v0.9 f52c633
```



再用命令`git tag`查看标签：

```plain
$ git tag
v0.9
v1.0
```



注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```plain
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```



可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```plain
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```



用命令`git show <tagname>`可以看到说明文字：

```plain
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
...
```



 注意

标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

-----

### 操作标签

如果标签打错了，也可以删除：

```plain
$ git tag -d v0.1
Deleted tag 'v0.1' (was f15b0dd)
```



因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```plain
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v1.0 -> v1.0
```



或者，一次性推送全部尚未推送到远程的本地标签：

```plain
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
 * [new tag]         v0.9 -> v0.9
```



如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```plain
$ git tag -d v0.9
Deleted tag 'v0.9' (was f52c633)
```



然后，从远程删除。删除命令也是push，但是格式如下：

```plain
$ git push origin :refs/tags/v0.9
To github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```



要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

-----

## 使用github

我们一直用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。

在GitHub出现以前，开源项目开源容易，但让广大人民群众参与进来比较困难，因为要参与，就要提交代码，而给每个想提交代码的群众都开一个账号那是不现实的，因此，群众也仅限于报个bug，即使能改掉bug，也只能把diff文件用邮件发过去，很不方便。

但是在GitHub上，利用Git极其强大的克隆和分支功能，广大人民群众真正可以第一次自由参与各种开源项目了。

如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

```plain
git clone git@github.com:michaelliao/bootstrap.git
```



一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

Bootstrap的官方仓库`twbs/bootstrap`、你在GitHub上克隆的仓库`my/bootstrap`，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

```
┌─ GitHub ────────────────────────────────────┐
│                                             │
│ ┌─────────────────┐     ┌─────────────────┐ │
│ │ twbs/bootstrap  │────▶│  my/bootstrap   │ │
│ └─────────────────┘     └─────────────────┘ │
│                                  ▲          │
└──────────────────────────────────┼──────────┘
                                   ▼
                          ┌─────────────────┐
                          │ local/bootstrap │
                          └─────────────────┘
```

如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

如果你没能力修改bootstrap，但又想要试一把pull request，那就Fork一下我的仓库：https://github.com/michaelliao/learngit，创建一个`your-github-id.txt`的文本文件，写点自己学习Git的心得，然后推送一个pull request给我，我会视心情而定是否接受。

### 小结

- 在GitHub上，可以任意Fork开源仓库；
- 自己拥有Fork后的仓库的读写权限；
- 可以推送pull request给官方仓库来贡献代码。

-----

## 使用Gitee

----

## 自定义Git

在[安装Git](https://liaoxuefeng.com/books/git/install-git/index.html)一节中，我们已经配置了`user.name`和`user.email`，实际上，Git还有很多可配置项。

比如，让Git显示颜色，会让命令输出看起来更醒目：

```plain
$ git config --global color.ui true
```



这样，Git会适当地显示不同的颜色，比如`git status`命令：

文件名就会标上颜色。

我们在后面还会介绍如何更好地配置Git，以便让你的工作更高效。

### 忽略特殊文件

有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次`git status`都会显示`Untracked files ...`，有强迫症的童鞋心里肯定不爽。

好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

 注意

`.gitignore`文件本身应该提交给Git管理，这样可以确保所有人在同一项目下都使用相同的`.gitignore`文件。

不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[GitHub/gitignore](https://github.com/github/gitignore)

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有`Desktop.ini`文件，因此你需要忽略Windows自动生成的垃圾文件：

```plain
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini
```



然后，继续忽略Python编译产生的`.pyc`、`.pyo`、`dist`等文件或目录：

```plain
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build
```



加上你自己定义的文件，最终得到一个完整的`.gitignore`文件，内容如下：

```plain
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```



最后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了：

```plain
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.
```



如果你确实想添加该文件，可以用`-f`强制添加到Git：

```plain
$ git add -f App.class
```



或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

```plain
$ git check-ignore -v App.class
.gitignore:3:*.class	App.class
```



Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

还有些时候，当我们编写了规则排除了部分文件时：

```plain
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class
```



但是我们发现`.*`这个规则把`.gitignore`也排除了，并且`App.class`需要被添加到版本库，但是被`*.class`规则排除了。

虽然可以用`git add -f`强制添加进去，但有强迫症的童鞋还是希望不要破坏`.gitignore`规则，这个时候，可以添加两条例外规则：

```plain
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore和App.class:
!.gitignore
!App.class
```



把指定文件排除在`.gitignore`规则外的写法就是`!`+文件名，所以，只需把例外文件添加进去即可。

可以通过[GitIgnore Online Generator](https://gitignore.puppylab.org/)在线生成`.gitignore`文件并直接下载。

最后一个问题：`.gitignore`文件放哪？答案是放Git仓库根目录下，但其实一个Git仓库也可以有多个`.gitignore`文件，`.gitignore`文件放在哪个目录下，就对哪个目录（包括子目录）起作用。

```
myproject          <- Git仓库根目录
├── .gitigore      <- 针对整个仓库生效的.gitignore
├── LICENSE
├── README.md
├── docs
│   └── .gitigore  <- 仅针对docs目录生效的.gitignore
└── source
```

**小结**

- 忽略某些文件时，需要编写`.gitignore`；
- `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

-----

### 配置别名

有没有经常敲错命令？比如`git status`？`status`这个单词真心不好记。

如果敲`git st`就表示`git status`那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后`st`就表示`status`：

```plain
$ git config --global alias.st status
```



好了，现在敲`git st`看看效果。

当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

```plain
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```



以后提交就可以简写成：

```plain
$ git ci -m "bala bala bala..."
```



`--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

在[撤销修改](https://liaoxuefeng.com/books/git/time-travel/restore/index.html)一节中，我们知道，命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个`unstage`别名：

```plain
$ git config --global alias.unstage 'reset HEAD'
```



当你敲入命令：

```plain
$ git unstage test.py
```



实际上Git执行的是：

```plain
$ git reset HEAD test.py
```



配置一个`git last`，让其显示最后一次提交信息：

```plain
$ git config --global alias.last 'log -1'
```



这样，用`git last`就能显示最近一次的提交：

```plain
$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py
```



甚至还有人丧心病狂地把`lg`配置成了：

```plain
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

#### 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

```plain
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1
```



别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

```plain
$ cat .gitconfig
[alias]
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com
```



配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置，或者直接删掉配置文件错误的那一行。

**小结**

给Git配置好别名，就可以输入命令时偷个懒。我们鼓励偷懒。

----

### 搭建Git服务器

在[远程仓库](https://liaoxuefeng.com/books/git/remote/index.html)一节中，我们讲了远程仓库实际上和本地仓库没啥不同，纯粹为了7x24小时开机并交换大家的修改。

GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

假设你已经有`sudo`权限的用户账号，下面，正式开始安装。

第一步，安装`git`：

```plain
$ sudo apt install git
```



第二步，创建一个`git`用户，用来运行`git`服务：

```plain
$ sudo adduser git
```



第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

```plain
$ sudo git init --bare sample.git
```



Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

```plain
$ sudo chown -R git:git sample.git
```



第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

```plain
git:x:1001:1001:,,,:/home/git:/bin/bash
```



改为：

```plain
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```



这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：

```plain
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```



剩下的推送就简单了。

#### 管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的`/home/git/.ssh/authorized_keys`文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

这里我们不介绍怎么玩[Gitosis](https://github.com/res0nat0r/gitosis)了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

#### 管理权限

有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。[Gitolite](https://github.com/sitaramc/gitolite)就是这个工具。

这里我们也不介绍[Gitolite](https://github.com/sitaramc/gitolite)了，不要把有限的生命浪费到权限斗争中。

**小结**

- 搭建Git服务器非常简单，通常10分钟即可完成；
- 要方便管理公钥，用[Gitosis](https://github.com/res0nat0r/gitosis)；
- 要像SVN那样变态地控制权限，用[Gitolite](https://github.com/sitaramc/gitolite)。

-----

## 使用SourceTree

