# 常见轮播效果及其实现原理

>说明： 本项目只做最基础的原理分析与最简单的应用场景。

## 1. 简单做法：css定位 + js滑动动画

    思路： 在同一个父容器内，
    1. 通过改变父容器的`left` 值实现---视图切换，
    2. 通过补充边界视图的过渡状态，外观上实现---无缝滚动。

核心代码：``` custom-carsoul.html || simple-slide.html```

```
//css
 #container {
        width: 600px;
        height: 400px;
        border: 3px solid #333;
        overflow: hidden;
        position: relative;
    }

    #list {
        width: 3000px;
        height: 400px;
        position: absolute;
        z-index: 1;
    }

 #list div {
        float: left;
        display: flex;
        justify-content: center;
        background: #ccc;
        width: 600px;
        height: 300px;
    }

//js

var go = function() {
                if ((speed > 0 && parseInt(list.style.left) < left) || (speed < 0 && parseInt(list.style.left) > left)) {
                    list.style.left = parseInt(list.style.left) + speed + 'px';
                    setTimeout(go, inteval);
                } else {
                    list.style.left = left + 'px';
                    if (left > -200) {
                        list.style.left = -600 * len + 'px';
                    }
                    if (left < (-600 * len)) {
                        list.style.left = '-600px';
                    }
                    animated = false;
                }
            }

```


## 2. css渐变：css + js滑动动画

    思路： 在同一个父容器内，
    1. 通过改变父容器的`left` 值实现---视图切换，
    2. 通过补充边界视图的过渡状态，外观上实现---无缝滚动。

核心代码：

```
//css

//js

```

## 3. css动画：css-animate

    思路： 在同一个父容器内，
    1. 通过改变父容器的`left` 值实现---视图切换，
    2. 通过补充边界视图的过渡状态，外观上实现---无缝滚动。

核心代码：``` css-animate.html```

```
//css

@keyframes slide {
        0% {
            background-position: 0 0;
        }
        10%, 25% {
            background-position: -600px 0;
        }
        35%, 50% {
            background-position: -1200px 0;
        }
        60%, 75% {
            background-position: -1800px 0;
        }
        85%, 100% {
            background-position: 0 0;
        }
    }

```

## 4. css3D旋转：css3 + js旋转

    思路： 在同一个父容器内，
    1. 将各个视图围成正棱柱体，
    2. 改变正棱柱体旋转角度及视口距离，而实现无限滚动轮播

核心代码：```3d-transform.html```

```
//css
 .container {
        width: 150%;
        height: 100px;
        transition: transform 1s;
        transform-style: preserve-3d;
        position: absolute;
    }

//js

function transform(element, value) {
            return element.style['transform'] = value;
        }
//50 为调整值
        transZ = 50/ Math.tan((rotate / 2 / 180) * Math.PI);

```

## 5. bootstrap：css3（transform：translate3d） + js

    思路： 在同一个父容器内，
    1. 活动元素显示，非活动元素过渡切换状态

核心代码：```bootstrap-carsoul.html```

```
//css
    /* 默认情况下 .item 元素隐藏*/
    .carousel-inner>.item {
        position: relative;
        display: none;
        transition: transform .6s ease-in-out;
    }
    /* 活动状态的元素才显示 */
    .carousel-inner>.active,
    .carousel-inner>.next,
    .carousel-inner>.prev {
        display: block;
    }
    /* 切换状态，显示目标元素 */
    .carousel-inner>.item.active,
    .carousel-inner>.item.next.left,
    .carousel-inner>.item.prev.right {
        left: 0;
        transform: translate3d(0, 0, 0);
    }
    /* 切换过程，前后两个元素的过度 */
    .carousel-inner>.item.active.left,
    .carousel-inner>.item.prev {
        left: 0;
        transform: translate3d(-100%, 0, 0);
    }
    .carousel-inner>.item.active.right,
    .carousel-inner>.item.next {
        left: 0;
        transform: translate3d(100%, 0, 0);
    }
//js

```

## 6. css变换：css3（transform)

    思路： 在同一个父容器内，
    1. 活动元素显示，非活动元素过渡切换状态

核心代码：```bootstrap-carsoul.html```

```
//css
    /* 默认情况下 .item 元素隐藏*/
    .carousel-inner>.item {
        position: relative;
        display: none;
        transition: transform .6s ease-in-out;
    }
    /* 活动状态的元素才显示 */
    .carousel-inner>.active,
    .carousel-inner>.next,
    .carousel-inner>.prev {
        display: block;
    }
    /* 切换状态，显示目标元素 */
    .carousel-inner>.item.active,
    .carousel-inner>.item.next.left,
    .carousel-inner>.item.prev.right {
        left: 0;
        transform: translate3d(0, 0, 0);
    }
    /* 切换过程，前后两个元素的过度 */
    .carousel-inner>.item.active.left,
    .carousel-inner>.item.prev {
        left: 0;
        transform: translate3d(-100%, 0, 0);
    }
    .carousel-inner>.item.active.right,
    .carousel-inner>.item.next {
        left: 0;
        transform: translate3d(100%, 0, 0);
    }
//js

```