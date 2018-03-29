```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p id="clock"></p>

<script>
    (function () {
        function initArray(){
            this.length=initArray.arguments.length
            for(var i=0;i<this.length;i++)
                this[i+1]=initArray.arguments[i]
        }
        var d=new initArray("星期日","星期一","星期二","星期三","星期四","星期五","星期六");

        var setClock = function() {
            // console.log(0);
            today = new Date();
            var hours = today.getHours();
            var minutes = today.getMinutes();
            var seconds = today.getSeconds();

            if (hours < 10) {
                hours = "0" + hours;
            }

            if (minutes < 10) {
                minutes = "0" + minutes;
            }



            if (seconds < 10) {
                console.log(1);
                seconds = "0" + seconds;
            }

            console.log(seconds);

            month = today.getMonth() + 1;


            var time = today.getFullYear()
                + "-"+ month
                + "-" + today.getDate() + " "
                + d[today.getDay()+1] + " "
                + hours + " : "
                + minutes + " : "
                + seconds;
            document.getElementById('clock').innerHTML = time;

            // console.log(time);
            // console.log(new Date());
            today = null;
            time = null;

        }
        setClock();
        setInterval(setClock, 1000);
    })();

</script>
</body>
</html>
```