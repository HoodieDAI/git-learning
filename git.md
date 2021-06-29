# git操作

## 配置

* 全局配置：

  git config --global

* 查看全局配置

  git config --global --list

* 普通配置

  git config

* 查看普通配置

  git config --list

* 设置用户名

  git config user.name 'Gavin'

* 查看用户名

  git config user.name

* 配置代理服务器

  git config --global https.proxy https://ip:port

## git的三种状态

* 已提交（commited）：已保存到本地数据库中
* 已修改（modified）：修改了文件没有保存进数据库中
* 已暂存（staged）：对修改文件的当前版本做了标记，使之包含在下次提交快照中

## git的三个区域

* 工作区：某个版本的内容
* 暂存区：保存了下次要提交的文件列表信息
* git目录：项目的原数据库和对象数据库

## git状态与区域的关系

commit 提交暂存区的文件 -> 版本库（已提交）

add -> 暂存区（已暂存） 

修改了暂存区的文件 -> 已修改

工作的版本和目录 -> 工作区

## 基础操作

### 初始化本地仓库

`git init`

### 添加

`git add 文件`

`git add .`添加此目录

### 提交

`git commit -m '提交消息'`

### git状态

`git status` 

### 日志

`git log`

```shell
commit c5a962d339a53b58f65c12ea1b7ea01ac0aca48d (HEAD -> master)
Author: Gavin <codedai@CodeDAIdeMacBook-Pro.local>
Date:   Mon Jun 28 22:03:49 2021 +0800

    第二次提交

commit 4c1d9b57acc271b2ee31c3b37b6136f8d0e47982
Author: Gavin <codedai@CodeDAIdeMacBook-Pro.local>
Date:   Mon Jun 28 21:57:28 2021 +0800

    第一次提交
```

* commit 后面是根据文件 SHA-1 算出来的串 代表唯一版本
* Author 作者 可以用 `git config --global user.name '修改'`
* Date 提交日期

`git log --pretty=online` 输出一行

`git log --pretty=oneline --graph`

## 回退

### 快照

每次提交就是一个版本，这个版本就是快照

### 回滚快照

`git reset HEAD～`回退一个版本

`git reset HEAD~~`回退两个版本

`git reset HEAD~10`回退10个版本

* HEAD 当前指向版本

#### 参数选择

`--hard` 回退 版本库，暂存区，工作区。（代码会消失 因为工作区消失了）

`--mixed`回退 版本库，暂存区（默认 工作区还在之前提交的代码还在，但是修改的内容没了）

`--soft`回退 版本库 只移动HEAD指向 相当于撤销提交

### 指定快照回退

`git reset --hard hash值`

hash通过 `git log` 查看

### ‘前滚’

`git reflog` 查看之前之后的提交记录

`get reset '之前的版本号'`

### 远程分支下的回退

因为远程中reset无效，所以操作的逻辑就是先将当前commit的内容变为需要回退到的快照，然后再做一次提交

这些功能被 `git revert`实现了

#### 回退到上一个版本

`git revert HEAD`

注意：这里会新增一个commit而不是减少一个commit 原因如上



## 版本对比

`git diff`

```shell
diff --git a/git.md b/git.md
index c864e0d..66e627d 100644
--- a/git.md
+++ b/git.md
@@ -74,4 +74,68 @@ add -> 暂存区（已暂存）
 
 ### 日志
 
-`git log`
\ No newline at end of file
+`git log`
+
+```shell
+commit c5a962d339a53b58f65c12ea1b7ea01ac0aca48d (HEAD -> master)
+Author: Gavin <codedai@CodeDAIdeMacBook-Pro.local>
+Date:   Mon Jun 28 22:03:49 2021 +0800
```



含义说明：

* 第一行：`diff --git a/git.md b/git.md`
  * a：暂存区域文件
  * b：工作区域文件
* 第二行：`index c864e0d..66e627d 100644`
  * C8xxx 和 66exxx 文件id
  * 100644 文件类型和权限

* 第三行：`--- a/git.md`
  * ---表示旧文件（暂存区中）
* 第四行：`+++ b/git.md`
  * +++表示新文件（工作区中）
* 第五行：`@@ -74,4 +74,68 @@ add -> 暂存区（已暂存）`
  * -：旧文件
  * +：新文件
  * xx，xx：开始行号，显示行数

### 比较与head指向快照相比

`git diff head`

### 两个历史快照相比

`git diff hash hash`

### 比较仓库和暂存区

`git diff --cached hash`

## 删除文件

### 恢复一个删除掉的文件

`git checkout --b 被删除文件`

### 删除文件

1. 外面删除 -> add -> commit
2. git删除：`git rm 文件` （此操作，删除工作区和暂存区，提交后才算删除版本库的

### 删除暂存区

`git rm --cached 文件名`

### 重命名

`git mv 被重命名文件 目标文件`

## 忽略文件

在 `.gitignore`中添加文件即可（可以用通配符）

## 分支操作

### 创建分支

`git branch 分支名`

### 切换分支

`git checkout 分支名`

### 创建同时切换分支

`git checkout -b 需要创建的分支`

### 合并分支

#### 使用merge合并

首先要切换到需要合并到的分支

`git merge 需要合并的分支`

#### 使用rebase合并

rebase是变基，在同一分支下rebase可以合并几条commit快照然后生成新的快照

rebase同样可以用作合并，这使合并后的分支树更为简洁

假如有两个分支 master 和 bugFix

![image-20210629214946994](/Users/codedai/git.assets/image-20210629214946994.png)

现在需要使用rebase将 c2和c3 合并到 c4下

1.  首先切换到需要变基的分支（将bugFix的基迁到master下）
2. `git rebase 目标base`（master）

`git checkout bugFix; git rebase master;`

![image-20210629215252977](/Users/codedai/git.assets/image-20210629215252977.png)

这样提交树就变成一条了

### 删除分支

`git branch -d 分支名`

### HEAD 操作

之前的commit revert reset checkout rebase 等等一系列操作都是在HEAD的基础上进行操作的

首先：切换分支就是将HEAD指向分支所在最终节点

* 例如 git commit 其实是在HEAD所在节点进行一次提交
* revert、reset也是基于head所在位置进行调整
* rebase也是基于head所在位置进行变基，若rebase不属于任何有名字的分支，则将其head所在的一条线变基
* checkout 就是切换HEAD所在位置

#### 利用checkout分离HEAD

综上所诉：HEAD一般来说就是在分支的最后一个节点上，checkout 相当于切换HEAD到某个分支末尾处

所以，checkout也可以直接切换到某个快照（commit）上，具体用法如下：

`git checkout 节点HASH` （测试这里的时候吓死我了，我写的笔记全没了，恢复用 `git reset --hard`）

利用分离的HEAD可以让我们很灵活的操作提交树

### 强制移动分支

`git branch -f 需要移动的分支 HEAD`| `git branch -f 需要移动的分支 HEAD的相对位置`



### 自由修改提交树 `cherry-pick`

可以选择某个或者多个分支上的一些快照，合并到一个分支

![image-20210629221659188](/Users/codedai/Desktop/笔记/GIt/git.assets/image-20210629221659188.png)

例如我们需要将 c2 和 c7 合并到 master 下

使用 `git cherry-pick c2 c7` 

![image-20210629221949822](/Users/codedai/Desktop/笔记/GIt/git.assets/image-20210629221949822.png)







# 远程仓库操作

## 代码克隆

`git clone`

## 更新仓库

### `git fetch`

注意和 pull 区分

* fetch 会下载本地中缺失远程提交数据
* fetch 会更新本地的远程指针（origin/master）

但是：

* fetch 不会修改你工作区的文件，也就是当前看到的文件不会发生变化
* 本地的指针不会发生变化

使用 git fetch 后更新工作区的方法

由于fetch后文件其实都下载下来了，但是由于本地分支并不会发生变化，所以只用调整本地分支即可

![image-20210629224653397](/Users/codedai/Desktop/笔记/GIt/git.assets/image-20210629224653397.png)

- `git cherry-pick o/master`
- `git rebase o/master`
- `git merge o/master`

![image-20210629224617608](/Users/codedai/Desktop/笔记/GIt/git.assets/image-20210629224617608.png)

### `git pull`

其实 `git pull` 就是来解决上面那么麻烦的操作的。。。

