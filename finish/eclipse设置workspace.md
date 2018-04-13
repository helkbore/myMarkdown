# eclipse设置workspace

## 更改workspace
windows->perferences->General->Startup and Shutdown->workspaces;
勾选“Prompt for workspace on startup”然后确定即可

## 复制workspace
1. 使用eclipse新建workspace。 
2. 将新建的workspace下的.metadata.plugins内容全部删除。 
3. 将需要拷贝的workspace下的.metadata.plugins内容除了org.eclipse.core.resources文件夹的其他文件夹全部拷贝到新workspace的.metadata.plugins目录下。 
4. 重启eclipse（可直接在eclipse菜单中点击File->Restart）。
