---
layout: post
title: "Sass media queries"
comments: true
description: "Responsive Web Design in Sass: Using media queries in Sass 3.2"
keywords: "Sass”
---

## queries 变量



```scss
$break-small: 320px;
$break-large: 1200px;

.profile-pic {
  float: left;
  width: 250px;
  @media screen and (max-width: $break-small) {
    width: 100px;
    float: none;
  }
  @media screen and (min-width: $break-large) {
    float: right;
  }
}
```

会被编译成

```css
profile-pic {
  float: left;
  width: 250px;
}
@media screen and (max-width: 320px) {
  .profile-pic {
    width: 100px;
    float: none;
  }
}
@media screen and (min-width: 1200px) {
  .profile-pic {
    float: right;
  }
}
```



## Variables as full query

```scss
$information-phone: "only screen and (max-width : 320px)";

@media #{$information-phone} {
  background: red;
}
```

