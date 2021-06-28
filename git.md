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