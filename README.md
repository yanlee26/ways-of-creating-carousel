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


## 2. css渐变：css-transition

    思路： 在同一个父容器内，
    1. 通过改变各个仕途的过渡渐变，实现

核心代码：```css3-transition.html```

```
//css

#slideshow img {
        width: 800px;
        position: absolute;
        top: 0;
        left: 0;
        opacity: 0;
        transition: opacity 1s linear;
    }

    #slideshow img.active {
        opacity: 1;
    }

```

## 3. css动画：css-animate

    思路： 在同一个父容器内，
    1. 通过各个视图切换动画实现

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
    1. 重点难点在于css3的过渡与渐变的应用；
    2. 临界状态过渡处理；
    3. 活动元素class：active->active left||right->'',下一个活动元素class：next left || prev right->active；
    4. 同一次状态只改变两个元素的class属性；

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
//js(mocking up)
    window.onload = function() {
      var items = document.querySelectorAll('.carousel-inner .item');
      var prev = document.querySelector('#prev');
      var next = document.querySelector('#next');
      var animated = false;
      var cur = 0;
      var duration = 600;

      function moveTo(i) {
        animated = true;
        if (i > 0) {
          cur = cur === 2 ? 0 : cur + i;
          var cLeft = cur - i === -1 ? 2 : cur - i;
          transition(cur, cLeft, 'next', 'left')
        } else {
          cur = cur === 0 ? 2 : cur + i;
          var cRight = cur - i === 3 ? 0 : cur - i;
          transition(cur, cRight, 'prev', 'right')
        }

        function transition(cur, cx, order, direction) {
          items[cx].classList.add(direction);
          items[cur].classList.add(order, direction);
          setTimeout(() => {
            items[cx].classList.remove('active', direction);
            items[cur].classList.remove(order, direction);
            items[cur].classList.add('active');
            animated = false;
          }, duration);
        }
      }
      prev.onclick = function() {
        if (animated) {
          return
        }
        moveTo(-1);
      }
      next.onclick = function() {
        if (animated) {
          return
        }
        moveTo(1);
      }
    }

```

## 6. css变换：css3（transform)||css3 transition

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
