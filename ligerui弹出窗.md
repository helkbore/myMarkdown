```javascript
$.ligerDialog.confirm(" 验证成功 "+",是否继续操作","提示信息", function(rtn) {
    if(rtn){
        this.close();
        // window.location.reload();
        // window.location.href = window.location.href;
    }else{
        window.location.href = "${ctx}/platform/system/sysUser/list.ws";
    }
});
```

```javascript
$.ligerDialog.alert('验证失败','出错了');
```