<!doctype html>
<html>

<head>
    <meta charset="utf-8">
    <title>前端啊轩的旋转相册</title>
    <link rel="shortcut icon" href="http://xz.jvtc.jx.cn/JVTC_XG/Download/ClassPhone/202115184831bf763eb3c8d1453893da2295bdbd5bcc.jpg"type="image/x-icon">
    <script src="https://libs.baidu.com/jquery/1.11.3/jquery.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            font-size: 0;
        }

        html,
        body {
            width: 100%;
            height: 100%;
            display: flex;
            display: -webkit-flex;
            background: -webkit-radial-gradient(#490338 10%, #000);
            perspective: 900px;
            overflow: hidden;
        }
        #xuanz{
            width: 133px;
            height: 200px;
            margin: auto;
            position: relative;
            transform-style: preserve-3d;
            animation: move 10s linear infinite alternate;
        }
        #album {
            width: 133px;
            height: 200px;
            margin: auto;
            position: relative;
            transform-style: preserve-3d;
            transform: rotateX(-20deg);
        }

        #album img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            /* 反射倒影 距离下面5px  */
            -webkit-box-reflect: below 5px -webkit-linear-gradient(top, rgba(0, 0, 0, 0) 40%, rgba(0, 0, 0, 0.5));
        }

        #album p {
            position: absolute;
            left: calc(133px/2 - 800px/2);
            top: calc(200px/2 - 800px/2);
            width: 800px;
            height: 800px;
            background: rgba(255, 255, 255, 0.1);
            transform: translateY(100px) rotateX(90deg);
            border-radius: 50%;
        }

        @keyframes move {
            from {
                transform: rotateX(0deg) rotateY(0deg);
            }

            to {
                transform: rotateX(0deg) rotateY(360deg);
            }
        }
    </style>
</head>

<body>
    <div id="album">
        <div id="xuanz">
            <img src="img/mmexport1518702509785.jpg">
            <img src="img/mmexport1518702514104.jpg">
            <img src="img/mmexport1518703885243.jpg">
            <img src="img/mmexport1518703888670.jpg">
            <img src="img/mmexport1518703898665.jpg">
            <img src="img/mmexport1518703906282.jpg">
        </div>
        <p></p>
    </div>

    <script>
        /*旋转分散*/
        window.onload = function () {
            var album = document.getElementById('album'),
                aImg = document.querySelectorAll('img');
            for (var i = 0; i < aImg.length; i++) {
                // 图片旋转分散 36°
                aImg[i].style.transform = 'rotateY(' + i * 360 / aImg.length + 'deg) translateZ(300px)';
                aImg[i].style.transition = 'transform 1s ' + (aImg.length - i) * 0.1 + 's';
            }
            var lastX = 0, // 前一次的坐标X
                lastY = 0,
                nowX = 0, // 当前的坐标X
                nowY = 0,
                desX = 0,
                desY = 0,
                rotX = -30,
                rotY = 0,
                timer; // 时间间隔
            document.onmousedown = function (e) {
                var e = e || event;
                lastX = e.clientX;
                lastY = e.clientY;
                this.onmousemove = function (e) {
                    var e = e || event;
                    nowX = e.clientX;
                    nowY = e.clientY;
                    desX = nowX - lastX;
                    desY = nowY - lastY;
                    // 更新album的旋转角度，拖拽越快-> des变化大 -> roY变化大 -> 旋转快
                    rotX -= desY * 0.1;
                    rotY += desX * 0.2;
                    album.style.transform = 'rotateX(' + rotX + 'deg) rotateY(' + rotY + 'deg)';
                    lastX = nowX;
                    lastY = nowY;
                }
                document.onmouseup = function () {
                    this.onmousemove = this.onmouseup = null;
                    timer = setInterval(function () {
                        desX *= 0.95;
                        desY *= 0.95;
                        rotX -= desY * 0.1;
                        rotY += desX * 0.2;
                        album.style.transform = 'rotateX(' + rotX + 'deg) rotateY(' + rotY + 'deg)';

                        if (Math.abs(desX) < 0.5 && Math.abs(desY) < 0.5) {
                            clearInterval(timer);
                        }
                    }, 13)
                }
                // 阻止默认行为
                return false;
            }
        }
    </script>

</body>

</html>
