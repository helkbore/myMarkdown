#js禁止鼠标右键
## jQuery
```javascript
$(document).ready(function(){  
    $(document).bind("contextmenu",function(e){   
          return false;   
    });
});
```

## js
```javascript
<script language="JavaScript">
     document.oncontextmenu=new Function("event.returnValue=false;");
     document.onselectstart=new Function("event.returnValue=false;");
</script>
```

##禁止右键弹出的突破方法
地址栏中输入：`javascript:alert($(document).unbind("contextmenu",""));`

