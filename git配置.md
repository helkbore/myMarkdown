# git配置

1. 下载git
2. 安装
3. 本地设置的git文件夹(自己建, 应为本地仓库的位置)右键进入git bash
4. 输入账号密码用户名
    ```
  git config --global user.name "helkbore"
  git config --global user.email "helkbore@163.com"
  ```

6. SSH KEY
```
ssh-keygen -t rsa -C "helkbore@163.com"
```
7. 在系统用户的主目录下找到.ssh文件夹(我的是在C:\Users\whell)里面有`id_rsa`和`id_rsa.pub`两个文件
8. 登陆GitHub，(点击头像 -> settings 左边选择SSH and GPG keys)打开“Account settings”，“SSH Keys”页面：
9. 点“Add SSH Key”/ new SSH，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容
10. 回到git bash, 把远程的仓库拉下来
```
git clone git@github.com:helkbore/mycode.git
```
11. 查看远程分支, checkout出dev分支
```
git branch -r
git checkout origin/dev
```