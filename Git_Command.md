# Git Command

## 1. Git 是什么？

> Git 是用 C 编写的分布式版本控制系统，可以用来管理文件（代码、word等）的多个版本

## 2. Git 和 SVN 有什么区别？
* Git 是分布式的，每一台主机都保存着完整的版本库信息，个人工作的情况下无需联网

    ![](Assets/Images/2019-03-22-14-51-29.png)
* SVN 是集中式的，版本库信息只保存在中央服务器中，每一台主机需要联网才能访问版本信息

    ![](Assets/Images/2019-03-22-14-52-02.png)

* Git 和 SVN 最大的区别是维护版本信息的位置不同。
    * Git 中，每个主机都有完整的版本信息，如果充当中央服务器的 Git 仓库挂了，那么可以从一个主机中获取完整的版本可信息，重建中央服务器
    
    * SVN 中，只有中央服务器记录了完整的版本信息，每个主机需要在联网的环境下从中央服务器获取，如果中央服务器挂了，主机们将无法工作

* Git 的 `commit id` 是一串十六进制数，SVN 的 `commit id` 是由 1 递增的值

## 3. Git 的使用

* 安装后，需要打开 `Git Bash` 进行使用前的一些设置。因为 Git 是分布式版本控制系统，所以，每个主机都必须设置用户名和邮箱用来标识主机。命令为 `git config --global user.name "xxx"`，`git config --global user.email "yyy"`
    * 其中的 `--global` 表示本机的所有 Git 库都采用同样的用户名和邮箱

     ![image_1d4v8jg2g1stdn5n8tctbauar13.png-7.4kB][3]

* 将某个目录设置为一个 repository（本地 Git 仓库）。设置为 repository 意味这该目录下的所有文件的增删改都会被 Git 记录追踪，用于以后的还原。命令为 `git init`
    * 下图将 `E:\Codes\Python\AutoMail` 目录设置为 Git 仓库
    * 设置完成后，`E:\Codes\Python\AutoMail` 目录下会自动生成隐藏的 `.git\` 文件夹，用于管理 repository
    
     ![image_1d4v92u561lnjcjb9ot1iikea71g.png-5.8kB][4]


* 添加文件到 **\*本地 Git 仓库\*** 。添加文件分为两个步骤：
    * 将文件从本地工作区添加到本地 Git 缓存区。命令为  `git add "filename"`
    * 将文件从 Git 缓存区提交到本地 Git 仓库。命令为  `git commit -m "comment"`，`-m "comment"` 即本次提交的备注，方便后续查阅

        ![image_1d4vibqop161nsef1amh1sca1uln6p.png-48.3kB][5]


    * 下图将 `AutoGmMail.py` 添加到了 Git 仓库（`E:\Codes\Python\AutoMail`）

        ![image_1d4v9u9401mmdg8d1fe7jek1ots1t.png-13.4kB][6]

* 查看 Git 仓库中的文件改动。
    * 若仓库内的文件在上次 `commit` 后无改动，执行 `git status` 命令查看当前 Git 仓库的状态，显示仓库是 clean 的
    
        ![image_1d4vassdd5d1ad2tho14991dh32a.png-5.5kB][7]
    * 若仓库内的文件在上次 `commit` 后有了改动，但未添加到本地 Git 缓存区，执行 `git status` 命令后，会提示本次哪些文件有了改动，需要 **先添加再提交**

        ![image_1d4vb0p501rlt17iqbm81tc01k2b2n.png-12.3kB][8]
        
    * 若仓库内的文件在上次 `commit` 后有了改动，且已添加到本地 Git 缓存区，但未提交到本地 Git 仓库，执行 `git status` 命令后，会提示本次哪些文件有了改动，且 **已准备好提交**

        ![image_1d4vb63vu1l6mkpf1klv9b15gn34.png-7.8kB][9]

    * 使用 `git diff "filename"` 命令可查看文件较上次 `commit` 后具体的改动
        `git diff` 比较的是 工作区 和 缓存区 的差异
        `git diff --cached` 比较的是 缓存区 和 仓库区 的差异
        `git diff HEAD` 比较的是 工作区 和 仓库区 的差异
        （需要注意的是，`commit` 后，缓存区的内容并不是清空，而是和仓库区保持一致，所以未 `add` 时，`git diff` 会给人一种 “比较的是 缓存区 和 仓库区” 的错觉）
        （可以这样理解：`add` 操作是让 **缓存区** 与 **工作区** 一致，`commit` 操作是让 **仓库区** 与 **缓存区** 一致，因此未 `add` 时，缓存区仍是上次 `add` 之后的版本，`git diff` 比较出了 **工作区** 与 **缓存区** 不一致）
        ![image_1d4vf74f81ifpb701fug17h5138p3u.png-11.4kB][10]
    
    * `commit` 后再次使用 `git status` 命令查看当前 Git 仓库的状态，显示仓库是 clean 的

         ![image_1d4vfd8l916n1t3c1vrh3o312sh4b.png-12.1kB][11]
         
    * Git 管理的是文件的修改，而不是文件本身
        比如，新建一个文件 `text.txt`，先在内容中新增一行 `first`，然后执行 `add` 命令，再在内容中新增一行 `second`，最后执行 `commit` 命令。
        此时，工作区的版本包含了 `second`，缓存区和仓库区的版本只包含 `first`
         
         ![image_1d522cfrsrkg1vn918nl13uke2r76.png-26.9kB][12]
         
* 版本回退。
    * 查看当前 Git 仓库的提交记录，命令为 `git log`，显示 **最近** 到 **最远** 的提交记录

        ![image_1d4vfmh5rpr5ffh17vv1k891eif4o.png-17.5kB][13]
        
    * 本地仓库维系一个 HEAD 指针指向当前版本，回退版本时，Git 只是将 HEAD 指针移到目标版本。
        回退的命令为 `git reset --hard HEAD~n`，其中 `n` 表示前面的第 n 个版本
        
        ![image_1d4vglm8vrhajg5jl8dsbf35i.png-16.8kB][14]
        
    * 除了 `HEAD~n` 的回退方式，还可以指定 `commit_id` 进行回退，命令为 `git reset --hard commit_id`
        版本号不必写全（最少前 4 位），Git 会根据提供的版本号前缀进行查找
        
        ![image_1d4vgt89c5glj8ff45t2op745v.png-23.2kB][15]
        
    * 查看当前 Git 仓库的命令记录，命令为 `git reflog`，显示 **最近** 到 **最远** 的命令记录，其中包含 `commit_id` 信息
    
         ![image_1d4vhr2bl14ib1bfffnh8b81n006c.png-15.6kB][16]
         
    * 回退到之前的版本时，可用 `git log` 命令查询 `commit_id`
        更新到之后的版本时，可用 `git reflog` 命令查询 `commit_id`
        
    * 如果 Git 仓库目录下的某个文件只 `add` 过而未 `commit`，在进行版本回退时，会造成该内容永久消失！！!


* 撤销修改
    * 撤销 **工作区** 的修改
        命令为 `git checkout -- filename`，将工作区的版本还原到上次执行 `add` 操作的时候

        ![image_1d54gb4ks52o1q851ihb9o31j787j.png-19.1kB][17]


    * 撤销 **缓存区** 的修改
        命令为 `git reset HEAD filename`，将
    
    * 撤销 **仓库区** 的修改
        即回退版本，见上一节内容


  [1]: http://static.zybuluo.com/ys16514/4rce4kzzamgy0pfbijfe1sng/image_1d4uqj18n1vi05f1cl11j9iaunm.png
  [2]: http://static.zybuluo.com/ys16514/0ouvqescj4c0uetg21lya93e/image_1d4uqiafvr971s4l10jq1t89ahh9.png
  [3]: http://static.zybuluo.com/ys16514/iat5emaymc5mhocrkfojwxco/image_1d4v8jg2g1stdn5n8tctbauar13.png
  [4]: http://static.zybuluo.com/ys16514/ut7o53vnjsb2vcv2kvt6uin0/image_1d4v92u561lnjcjb9ot1iikea71g.png
  [5]: http://static.zybuluo.com/ys16514/vadi1awdabr8yysz5kelklsb/image_1d4vibqop161nsef1amh1sca1uln6p.png
  [6]: http://static.zybuluo.com/ys16514/ui8vi3b4qtulwveq3751k3nn/image_1d4v9u9401mmdg8d1fe7jek1ots1t.png
  [7]: http://static.zybuluo.com/ys16514/8p16dodgs7hh6588itfy86ch/image_1d4vassdd5d1ad2tho14991dh32a.png
  [8]: http://static.zybuluo.com/ys16514/e0ec1j0fsnr1otknc9nnksfr/image_1d4vb0p501rlt17iqbm81tc01k2b2n.png
  [9]: http://static.zybuluo.com/ys16514/1eh9hyhv3l18anc7lpxav04a/image_1d4vb63vu1l6mkpf1klv9b15gn34.png
  [10]: http://static.zybuluo.com/ys16514/320hc3lw8no4ap9vmdrp4ajm/image_1d4vf74f81ifpb701fug17h5138p3u.png
  [11]: http://static.zybuluo.com/ys16514/wevur7u35ateaxwl4tkotvd8/image_1d4vfd8l916n1t3c1vrh3o312sh4b.png
  [12]: http://static.zybuluo.com/ys16514/kp0kxtkb18gqa75ofmcpcjh5/image_1d522cfrsrkg1vn918nl13uke2r76.png
  [13]: http://static.zybuluo.com/ys16514/dtlazfs1n896m2p55p0qpcgc/image_1d4vfmh5rpr5ffh17vv1k891eif4o.png
  [14]: http://static.zybuluo.com/ys16514/veatp1ay163z52a58pe54z4j/image_1d4vglm8vrhajg5jl8dsbf35i.png
  [15]: http://static.zybuluo.com/ys16514/gcdbgrvkn7lqxacyy7qqf3jl/image_1d4vgt89c5glj8ff45t2op745v.png
  [16]: http://static.zybuluo.com/ys16514/em3pvktfffehk1zmblw1c6qq/image_1d4vhr2bl14ib1bfffnh8b81n006c.png
  [17]: http://static.zybuluo.com/ys16514/3820l7d2zdmgss4z10aqs2aq/image_1d54gb4ks52o1q851ihb9o31j787j.png

`git init` ：将当前文件夹设为本地 Git 仓库