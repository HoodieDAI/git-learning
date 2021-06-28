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

commit -> 版本库（已提交）

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