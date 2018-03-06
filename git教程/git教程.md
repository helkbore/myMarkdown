# git教程

[TOC]




## git理解

### git与svn区别
### 工作流程
#### 一般工作流程
+ 克隆 Git 资源作为工作目录。
+ 在克隆的资源上添加或修改文件。
+ 如果其他人修改了，你可以更新资源。
+ 在提交前查看修改。
+ 提交修改。
+ 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

![Alt text](img/1.png)

### 工作区, 暂存区, 版本库
工作区: 就是你在电脑里能看到的目录。
暂存区: ：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
版本库: 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

### 其他干货

#### git配置
+ Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。
+ 系统的配置文件:
+ 用户的配置文件
   在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录，一般都是 C:\Documents and Settings\$USER。(我的在 C:\Users\Administrator\.gitconfig)
+ 项目的配置文件
   在.git/config中
   
##### 用户信息
##### 文本编辑器
##### 差异分析工具
##### 查看配置信息


## git使用





### 安装
### 配置
### 场景
#### 创建仓库, 初始化
```
git init
```

指定目录和文件名
```
git init 文件名/文件名/文件名
```

### 使用工作流程步骤
#### 在github上创建一个版本库, 与本地同步, 上传下载文件
#### 在github上看到一个好的版本库想要下载下来

## git命令


### 配置
1. 用户信息: 配置个人的用户名称和电子邮件地址
```
git config --global user.name "runoob"
git config --global user.email test@runoob.com
```
如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。
如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

2. 文本编辑器
```
git config --global core.editor emacs
```

3. 差异分析工具: 在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话
```
git config --global merge.tool vimdiff
```
4. 查看配置信息
```
git config --list
```
有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 /etc/gitconfig 和 ~/.gitconfig），不过最终 Git 实际采用的是最后一个。

也可以直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可
```
git config user.name
```
### 库操作
1. 建库, 初始化
```
git init
```
2. git clone.  从现有 Git 仓库中拷贝项目（类似 svn checkout）
```
git clone <repo>
git clone <repo> <directory>
git clone git://github.com/schacon/grit.git
git clone <repo> <directory> <newname>
```
参数说明: 
  + repo:Git 仓库。
  + directory:本地目录。

实例: 建一个test库, 分别练习以上集中操作.
test文件夹下: 
```
git init test1
git clone test1 test2
git clone test1 test3/test1
```

## git 服务器搭建