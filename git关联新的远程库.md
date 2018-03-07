# git关联新的远程库
1. 在本地建库 `git init`
2. 添加远程库
```
git remote add origin git@github.com:helkbore/myJavaTest.git
```
3. 如果存在分支则先pull主分支, 本地建分支, pull远程分支
```
git pull origin master
git checkout -b dev
git pull origin dev
```
3. pull远程库
```
git pull origin master
```
4. push远程库
```
git push origin master
```