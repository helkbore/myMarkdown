一. 搭建中的坑
1. 建项目名要建成和用户名/邮箱同名的. 比如我的是 `helkbore`, 建程度就是 `helkbore.github.io` 不能起别的名了
2. 我安的时候next主题正在维护, 不知道出了什么问题不能自动生成index.html
3. 域名绑定的时候一定要用 `https` (可能可以用 `http` 但我不知道, 而且网上有文章说只允许 `http` )

二. 找到入口: hexo -v 
1. 在npm下找到 hexo.cmd
我的电脑里的位置是: C:\Users\username\AppData\Roaming\npm
```cmd
@IF EXIST "%~dp0\node.exe" (
  "%~dp0\node.exe"  "%~dp0\node_modules\hexo\bin\hexo" %*
) ELSE (
  @SETLOCAL
  echo %PATHEXT:;.JS;=;%
  @SET PATHEXT=%PATHEXT:;.JS;=;%
  node  "%~dp0\node_modules\hexo\bin\hexo" %*
)

```
+ 关于%
  变量引用
  上面引用了PATHEXT
  输出PATHEXT为
  > .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JSE;.WSF;.WSH;.MSC    
  
  参考了 http://www.jb51.net/article/32866.htm
+ 关于%dp0
  %0代表批处理本身
  ~dp是变量扩充
  d既是扩充到分区号
  p就是扩充到路径
  所以%dp0是该bat/cmd的完整路径(绝对路径)
+ 关于PATHEXT
  确切概念没查到, 好像是一个系统变量, 用来定义默认的扩展名的.
  
2. "%~dp0\node_modules\hexo\bin\hexo" :
`require('hexo-cli')();`

路径如下:
npm\node_modules\hexo\node_modules\hexo-cli\package.json 中的 "main": "lib/hexo"
找到
npm\node_modules\hexo\node_modules\hexo-cli\lib\hexo.js
