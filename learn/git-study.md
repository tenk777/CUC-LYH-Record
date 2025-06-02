# Git
GIt 是一个分布式版本控制工具，通常用来对软件开发过程中的源代码文件进行管理。通过 Git 仓库进行储存和管理这些文件，Git 仓库分为两种：

* **本地仓库**：开发人员自己电脑上的 Git 仓库
  
* **远程仓库**：远程服务器上的 Git 仓库
  
  * commit ：提交，将本地文件和版本信息保存到本地仓库
  
  * push ：推送，将本地仓库和版本信息上传到远程仓库
  
  * pull ：拉取，将远程仓库文件和版本信息下载到本地仓库

## 1. **Git 全局配置**

用户使用 Git 提交信息时使用的需要设置身份，以后每次提交都用这个身份。

*  **设置用户信息** 
  (用户名和邮箱仅作标识与远程仓库平台账号无关)

```bash
git config --global user.name "Tenk"
git config --global user.email "1418023531@qq.com"
```

* **查看配置信息**

```bash
git config --list
```

## 2. **Git 常用命令**

要对代码进行版本控制，首先要获得 Git 仓库。获取 Git 仓库通常有两种方式：

* **在本地当前目录初始化一个 git 仓库**

```bash
git init 
```

* **从远程克隆一个仓库**

```bash
git clone <仓库的.git文件夹的 url 地址>
```
### **基本概念-git仓库的三个结构**

接下来，在说管理文件前，先了解 git 中的三个**基本概念**：

* **版本库：** .git 隐藏文件夹就是版本库，里面有配置信息、日志信息、文件版本信息等

* **工作区：** 包含 .git 文件夹的目录就是工作区，也称为工作目录，主要用于存放开发的代码。

* **暂存区：** 在 .git 文件夹中，有一个 index 文件就是暂存区，也可以叫做 stage。暂存区是一个临时保存修改文件的地方

<img src="image\git-frame.png" alt="windows" style="zoom:63%;" />

**示例代码如下**：

```bash
git add 1.txt
git commit -m "我增加了一个1.txt文件" 1.txt 
# "-m"是--message的短命令格式，代表：“注释信息”
# 末尾指定从暂存区中提交文件的地址“1.txt”，不写就默认全部提交
```

**在每次提交完后，都会产生一个版本号**：

<img src="image\git_version.png" alt="windows" style="zoom:77%;" />

### **基本概念-工作区文件状态**

**git 工作区中的文件存在两种状态：**

* untracked 未跟踪（未被纳入版本控制, 不在暂存区）

* tracked 已跟踪 （被纳入版本控制， 在暂存区）

    * Unmodified 未被修改状态（版本库中有，新文件无修改）
  
    * Modofied 已修改状态（版本库中有，新文件有修改）

    * Staged 已缓存状态 （在暂存区）
   
文件显示红色是在工作区中，显示绿色是在暂存区中

### **本地仓库操作**

**本地仓库常用命令如下：**

```bash 
git status                查看文件状态
git add                   将文件的修改加入暂存区
git reset                 将暂存区的文件取消暂存
git reset --head <版本号>  切换到指定版本
git commoit               将暂存区的文件修改提交到版本库
git log                   查看日志：每次提交的版本号、日期、用户的信息等
```

### **远程仓库操作**

**远程仓库常用命令如下：**

```bash 
git remote -v                       查看远程仓库（fetch为拉取地址，push是推送地址）
git remote add <shortname> <url>    关联远程仓库（shortname:仓库别称） 
git clone                           从远程仓库克隆
git pull <shortname> <分支>         从远程仓库拉取
git push                            推送到远程仓库
```

 ### **分支操作**

 分支是Git使用过程中非常重要的概念。使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。同一个仓库可以有多个分支，各个分支相互独立，互不干扰。通过git init 命令创建本地仓库时会默认创建一个master分支。

**分支操作的常用命令如下：**

```bash 
git branch                          查看本地所有分支
git branch -r                       列出远程所有分支
git branch -a                       列出所有本地与远程的分支
git branch <name>                   创建分支 
git checkout <name>                 切换分支
git push <shortname> <name>         推送至远程仓库的指定分支
git merge <name>                    将指定分支合并到当前分支
git branch -d <name>                删除分支
```

 ### **标签操作**

 Git中的标签，指的是**某个分支**、**某个特定时间点**的状态。通过标签，可以很方便的切换到标记时的状态。比较有代表性的是人们会使用这个功能来标记发布结点（v1.0、 v1.2等）。

```bash
git tag                                列出所有标签
git tag <name>                         创建标签
git push <shortname> <name>            将标签推送至远程仓库
git checkout -b <branch> <name>        检出标签，恢复标签状态到某个分支上
```
 
 ## 3. **Git 冲突处理**

 ### 提交版本冲突

 这种冲突一般是由于：提交者的版本库 < 远程库的版本

 解决方法：由git直接合并，且期间git会自动创建一次合并提交到本地仓库

 ```bash
 git pull 
 或者： git fetch + git merge
 ```

 ### 内容冲突

 这种一般是团队合作时由于分工不明确，导致对同一内容都进行修改，需要人工取舍的冲突
 
方式一：

```bash
git push            # 由于冲突，push失败
git pull
# 拉取最新代码，进入merging冲突合并状态，由git对冲突进行检测并进行标注处理，git会列出有冲突的文件，接下来需要人工一个一个打开文件修改。
修改步骤：对冲突内容进行修改，决定最终留存版本，最后删除git的标记
git diff <文件>      # 查看工作区未修改前的 与 已修改的后的冲突的内容对比
git log --oneline    # 查看提交历史
                     # --oneline单行格式（哈希缩写 + 提交信息）。
git add .
git commit -m “合并了内容”
git push
```

方式二，使用stash暂存数据：

```bash
git stash     # 临时保存未提交的修改，恢复到最后一次提交的状态
git pull 
git stash pop # 恢复之前的修改，如果有冲突，解决冲突
git add .   
git coimmit
git push
```


 ## 参考资料

* 链接： [黑马程序员git教程](https://www.bilibili.com/video/BV1UV4y117k6?spm_id_from=333.788.videopod.episodes&vd_source=28d60f5e2808768fba4601bee623b2e1)

* 链接：[b站up主迷斯特航的git教程](https://www.bilibili.com/video/BV1BA41117Qb/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=28d60f5e2808768fba4601bee623b2e1)

* 链接：[CSDN如何处理冲突](https://blog.csdn.net/u013257321/article/details/146979730?fromshare=blogdetail&sharetype=blogdetail&sharerId=146979730&sharerefer=PC&sharesource=qq_61151489&sharefrom=from_link)