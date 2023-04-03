# Java Web

## jQuery

### 核心函数 $()

- 传入参数为【函数】时，页面加载完成后执行该函数，相当于window.onload()。
- 传入参数为【html字符串】时，会创建这个html标签对象。
- 传入参数为【选择器字符串】时，会返回DOM对象。
- 传入参数为【DOM对象】时，会包装成jQuery对象返回。

### jQuery对象

- 本质是若干个DOM对象的数组
- jQuery对象不能使用DOM对象的属性和方法
- jQuery对象与DOM对象互转
  - `var $obj = $(domObj)`
  - `var domObj = $obj[index]`

- 