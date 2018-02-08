---
layout: post
---

### 变量
让我们观察一下分页控件的样子。一系列的页码排成一行。每一个页码都有内边距，还有边框，但看不到外边距。有active和disabled两种状态，当鼠标悬浮上面有另一个效果。还提供了三个大小尺寸。  
综上我们应该有如下的一些变量：
- padding
- font-size
- border
- active
- disabled
- focus
- sm lg
- line-height
- 背景色和前景色

{% highlight scss %}
// Pagination

$pagination-padding-y:              .5rem !default;
$pagination-padding-x:              .75rem !default;
$pagination-padding-y-sm:           .25rem !default;
$pagination-padding-x-sm:           .5rem !default;
$pagination-padding-y-lg:           .75rem !default;
$pagination-padding-x-lg:           1.5rem !default;
$pagination-line-height:            1.25 !default;

$pagination-color:                  $link-color !default;
$pagination-bg:                     $white !default;
$pagination-border-width:           $border-width !default;
$pagination-border-color:           $gray-300 !default;

$pagination-focus-box-shadow:       $input-btn-focus-box-shadow !default;

$pagination-hover-color:            $link-hover-color !default;
$pagination-hover-bg:               $gray-200 !default;
$pagination-hover-border-color:     $gray-300 !default;

$pagination-active-color:           $component-active-color !default;
$pagination-active-bg:              $component-active-bg !default;
$pagination-active-border-color:    $pagination-active-bg !default;

$pagination-disabled-color:         $gray-600 !default;
$pagination-disabled-bg:            $white !default;
$pagination-disabled-border-color:  $gray-300 !default;

{% endhighlight %}

竟然还有box-shadow效果。  

上面的变脸看似多，其实都很容易理解，首先重头的是控制元素尺寸的变量，包括padding,border,line-height，然后是颜色，包括前景背景色。再然后就是元素处于不同状态时的颜色值。大致上就能总结完了。  

### 样式
{% highlight scss %}
.pagination {
    display: flex;
    @include border-radius();
    @include list-unstyled();
}

.page-link {
    position: relative;
    display: block;
    padding: $pagination-padding-y $pagination-padding-x;    
    margin-left: -$pagination-border-width;
    line-height: $pagination-line-height;
    text-align: center;
    color: $pagination-color;
    background-color: $pagination-bg;
    border: $pagination-border-width solid $pagination-border-color;

    &:hover {
    color: $pagination-hover-color;
    text-decoration: none;
    background-color: $pagination-hover-bg;
    border-color: $pagination-hover-border-color;
  }

  &:focus {
    z-index: 2;
    outline: 0;
    box-shadow: $pagination-focus-box-shadow;
  }

  // Opinionated: add "hand" cursor to non-disabled .page-link elements
  &:not([disabled]):not(.disabled) {
    cursor: pointer;
  }

}

.page-item {
    &:first-child {
        .page-link {
            margin-left: 0;
            @include border-left-radius($pagination-border-radius);
        }
    }

    &:last-child {
        .page-link {
            @include border-right-radius($pagination-border-radius);
        }
    }

    &.active .page-link {
    z-index: 1;
    color: $pagination-active-color;
    background-color: $pagination-active-bg;
    border-color: $pagination-active-border-color;
  }

  &.disabled .page-link {
    color: $pagination-disabled-color;
    pointer-events: none;
    // Opinionated: remove the "hand" cursor set previously for .page-link
    cursor: auto;
    background-color: $pagination-disabled-bg;
    border-color: $pagination-disabled-border-color;
  }
}

.pagination-lg {
  @include pagination-size($pagination-padding-y-lg, $pagination-padding-x-lg, $font-size-lg, $line-height-lg, $border-radius-lg);
}

.pagination-sm {
  @include pagination-size($pagination-padding-y-sm, $pagination-padding-x-sm, $font-size-sm, $line-height-sm, $border-radius-sm);
}

{% endhighlight %}

还要创建一个mixin文件在scss/mixins/_pagination.scss  
{% highlight scss %}
@mixin pagination-size($padding-y, $padding-x, $font-size, $line-height, $border-radius) {
  .page-link {
    padding: $padding-y $padding-x;
    font-size: $font-size;
    line-height: $line-height;
  }

  .page-item {
    &:first-child {
      .page-link {
        @include border-left-radius($border-radius);
      }
    }
    &:last-child {
      .page-link {
        @include border-right-radius($border-radius);
      }
    }
  }
}

{% endhighlight %}