---
title: single-transition-timing-function-单个过渡式时序函数
date: 2017-11-06 16:14:11
categories:
 - 技术
tags:
 - CSS
 - transition
 - 动画
---
> [引用英译<single-transition-timing-function>](https://developer.mozilla.org/en-US/docs/Web/CSS/single-transition-timing-function)

这个单调式时序过渡函数是一种 CSS [数据类型](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Types)，用于表示在整个一维动画期间，相应的值变化多快的数学函数。可以让你在整个持续时间内通过建立各种不同的数学函数来控制数值的速度。

“平滑”定时功能通常被称为 easing 函数。 他们将时间比率与输出比率相关联，均表示为<Number>。 对于这些值，0.0代表初始状态，1.0代表最终状态。
<!--more-->
{% img https://developer.mozilla.org/files/3434/TF_with_output_gt_than_1.png %}
{% img https://developer.mozilla.org/files/3435/TF_with_output_gt_than_1_clipped.png %}

根据所使用的具体功能，在动画过程中，计算的输出有时可能会增大到大于1.0或小于0.0。 这会导致动画比最终状态更远，然后返回。 对于某些属性，如左或右，这会产生一种“弹跳”效果。

但是，某些属性会超出允许的范围限制输出。 例如，大于255或小于0的颜色分量将被剪裁到最接近的允许值（分别为255和0）。 一些 cubic-bezier()  曲线显示此属性。

# Timing functions

CSS支持两种定时功能：作为函数的三次贝塞尔曲线的子集，以及阶梯函数。 这些函数中最有用的是给出一个关键字，使他们可以很容易地引用。

## The cubic-bezier() class of timing functions

{% img https://developer.mozilla.org/files/3433/cubic-bezier,%20example.png %}

cubic-bezier() 函数表示法定义了一个三次贝塞尔曲线。 如这些曲线是连续的，它们通常用于平滑向下的开始和动画的端，因此有时被称为缓和功能。

三次贝塞尔曲线由四个点P0，P1，P2和P3限定。 P0和P3是曲线的开始和结束，在CSS中，这些点是固定的，因为坐标是比率（横坐标是时间的比率，纵坐标是输出范围的比率）。 P0为（0,0），表示初始时间和初始状态，P3为（1,1），表示最终时间和最终状态。

不是所有的三次贝塞尔曲线适合作为计时功能，因为不是所有的数学函数; 即对于给定的横坐标具有零个或一个值的曲线。 与P0和P3固定由CSS如所定义的，三次贝塞尔曲线是一个函数，因此是有效的，当且仅当P1和P2的横轴都在[0，1]的范围内。

P1或P2纵坐标在[0,1]范围以外的三次贝塞尔曲线可能会产生弹跳效果。

当你指定一个无效的三次贝塞尔曲线时，CSS会忽略整个属性。

Syntax
```js
  cubic-bezier(x1, y1, x2, y2)
```

**x1, y1, x2, y2**

<number>值代表横坐标，P1和P2点的纵坐标定义三次Bézier曲线。 x1和x2必须在[0，1]范围内，否则该值无效。

### Examples

These cubic Bézier curves are valid for use in CSS :

```js
/* The canonical Bézier curve with four <number> in the [0,1] range. */
cubic-bezier(0.1, 0.7, 1.0, 0.1)

/* Using <integer> is valid as any <integer> is also a <number>. */
cubic-bezier(0, 0, 1, 1)

/* Negative values for ordinates are valid, leading to bouncing effects.*/
cubic-bezier(0.1, -0.6, 0.2, 0)

/* Values > 1.0 for ordinates are also valid. */
cubic-bezier(0, 1.1, 0.8, 4)
```

These cubic Bézier curves definitions are invalid :

```js
/* Though the animated output type may be a color, 
   Bézier curves work w/ numerical ratios.*/
cubic-bezier(0.1, red, 1.0, green)

/* Abscissas must be in the [0, 1] range or 
   the curve is not a function of time. */
cubic-bezier(2.45, 0.6, 4, 0.1)

/* The two points must be defined, there is no default value. */
cubic-bezier(0.3, 2.1)

/* Abscissas must be in the [0, 1] range or 
   the curve is not a function of time. */
cubic-bezier(-1.9, 0.3, -0.2, 2.1)
```

## The steps() class of timing functions

steps() 函数表示法定义了一个step函数，以等距离的步骤分割输出值的域。这个阶梯函数的子类有时也被称为阶梯函数。

{% img https://developer.mozilla.org/files/3436/steps(2,start).png steps(2, start) %}

{% img https://developer.mozilla.org/files/3437/steps(4,end).png steps(4, end) %}

**Syntax**

```js
steps(number_of_steps, direction)
```

where:

`number_of_steps`

是一个严格的正数<整数>，表示构成步进函数的等距踏板的数量。

`direction`

是一个关键字，指示函数是左连续的还是右连续的：
start表示一个左连续函数，所以动画开始的时候是第一步;
end表示一个右连续函数，以便动画结束时发生最后一步。
结束是默认的。

### Examples

These timing functions are valid :

```js
/* There is 5 treads, the last one happens 
   right before the end of the animation. */
steps(5, end)

/* A two-step staircase, the first one happening 
   at the start of the animation. */
steps(2, start)

/* The second parameter is optional. */
steps(2)
```

These timing function are invalid :
```js
/* The first parameter must be an <integer> and 
   cannot be a real value, even if it is equal to one. */
steps(2.0, end)

/* The amount of steps must be non-negative. */
steps(-3, start)

/* There must be at least one step.*/
steps(0, end)
```
