---
layout: post
---

面包屑还是挺简单的

### 变量
{% highlight scss %}
$breadcrumb-padding-y:              .75rem !default;
$breadcrumb-padding-x:              1rem !default;
$breadcrumb-item-padding:           .5rem !default;

$breadcrumb-margin-bottom:          1rem !default;

$breadcrumb-bg:                     $gray-200 !default;
$breadcrumb-divider-color:          $gray-600 !default;
$breadcrumb-active-color:           $gray-600 !default;
$breadcrumb-divider:                "/" !default;
{% endhighlight %}

### 样式
{% highlight scss %}
.breadcrumb {
    display: flex;
    flex-wrap: wrap;
    padding: $breadcrumb-padding-y $breadcrumb-padding-x;
    margin-bottom: $breadcrumb-margin-bottom;
    list-style: none;
    background-color: $breadcrumb-bg;
    @inlucde border-radius($border-radius);
}

.breadcrumb-item {
    + .breadcrumb-item::before {
        display: inline-block;
        padding-left: $breadcrumb-item-padding;
        padding-right: $breadcrumb-item-padding;
        color: $breadcrumb-divider-color;
        content: "#{$breadcrumb-divider}";
    }


  // IE9-11 hack to properly handle hyperlink underlines for breadcrumbs built
  // without `<ul>`s. The `::before` pseudo-element generates an element
  // *within* the .breadcrumb-item and thereby inherits the `text-decoration`.
  //
  // To trick IE into suppressing the underline, we give the pseudo-element an
  // underline and then immediately remove it.
    + .breadcrumb-item:hover::before {
        text-decoration: underline;
    }

    + .breadcrumb-item:hover::before {
        text-decoration: none;
    }

    &.active {
        color: $breadcrumb-active-color;
    }
}
{% endhighlight %}