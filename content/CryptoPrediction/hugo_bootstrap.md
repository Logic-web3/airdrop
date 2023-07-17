---
title: "Hugo_bootstrap"
date: 2023-07-15T07:17:23+08:00
draft: false
ShowToc: true # 开启 Table Of Content 功能
TocOpen: true # 展开 Table Of Content
---

# 新建主題

```bash

hugo new theme daobage
#創建一個名叫 daobage的主題模板

```

在```layouts/partials```路徑下創建一個```archive.html```文件，用於```index.html```和```list.html```兩個文件之間的共享,在```index.html```和```list.html```內輸入以下代碼

```bash
{{ define "main" }}
    {{- partial "archive.html" . - }}
{{ end }}
```

## 配置文件

1. 點擊網站鏈接：https://getbootstrap.com/docs/5.2/getting-started/introduction/，複製<head></head>代碼片段,粘貼到```head.html```

```bash
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Bootstrap demo</title>
</head>
```

2. 在```layouts/partials```路徑下創建一個```script-footer.html```,

3. 複製網站鏈接：https://getbootstrap.com/docs/5.2/getting-started/introduction/ 裡面的```script```文件

```bash
 <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
```

4. 將```baseof.html```代碼修改

```html
<html>
    {{- partial "head.html" . -}}
    <body>
        {{- partial "header.html" . -}}
        <div class="container">
            {{- block "main" . }}
            {{- end }}
            {{- partial "footer.html" . -}}
            {{- partial "script-footer.html" . -}}
    </div>
    </body>
</html>
```
6. 將```hugo.toml```，改成```hugo.yaml```，並且修改代碼：

```bash
baseURL: 'http://example.org/'
languageCode: 'en-us'
title: 'My New Hugo Site'
theme: 'daobaoge'
```

# Hugo 導航欄

1. 登錄網站：https://getbootstrap.com/docs/5.2/components/navbar/，點擊，右上角複製html內容，如下

```html
<nav class="navbar navbar-expand-lg bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled">Disabled</a>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>

```
2. 把複製好的html，粘貼到主題文件下，路徑如下```themes\daobaoge\layouts\partials\header.html```


3. 把```<nav class="navbar navbar-expand-lg bg-light">```改成```<nav class="navbar navbar-expand-xl navbar-dark bg-dark">```

代碼解釋：

  這兩個參數是用於定義網頁中的導航欄（Navbar）的樣式和佈局。以下是它們之間的區別：

  ```html
  <nav class="navbar navbar-expand-lg bg-light">：
  ```

  ```navbar-expand-lg``` 表示導航欄在螢幕寬度達到大型（large）尺寸時才會展開，通常用於桌面瀏覽器。
  ```bg-light``` 表示導航欄使用淺色（light）背景，通常是白色或淺色系。

  ```html
  <nav class="navbar navbar-expand-xl navbar-dark bg-dark">：
  ```

  ```navbar-expand-xl``` 表示導航欄在螢幕寬度達到超大型（extra-large）尺寸時才會展開，這個選項通常用於更大尺寸的螢幕或特殊需求。
  ```navbar-dark``` 表示導航欄使用深色主題
  ```bg-dark``` 則定義了導航欄的背景顏色。

4. 把代碼片段```<div class="container-fluid">```改成```<div class="container">```
   
   代碼解釋
   ```<div class="container-fluid">```,固定寬度的頁面

   ```<div class="container">```,寬度有限制，並且根據不同熒幕尺寸自動調整寬度。

5. 對url模塊進行修改，修改如下：
   
   ```html
    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            {{ $currentPage := . }}
            {{ range .Site.Menus.main }}
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="{{ .Permalink }}">{{ .Name }}</a>
                </li>
            {{ end }}
            </ul>
   ```

  代碼解讀：

    ```{{ $currentPage := . }}```：这行代码将当前上下文的值赋给 ```$currentPage``` 变量，以便在后续的代码块中使用。这样做的目的是为了方便在循环中引用当前页面的上下文数据。

    ```{{ range .Site.Menu.main }}```：这行代码开始一个循环，遍历 ```.Site.Menu.main``` 中的每个菜单项。

    在循环块内部，以下是生成导航菜单项的 HTML 结构：

    ```<li class="nav-item">```：表示一个列表项，使用 ```nav-item``` 类作为样式类。

    ```<a class="nav-link active" aria-current="page" href="{{ .Permalink }}">```：表示一个链接，使用 ```nav-link``` 类作为样式类，```active``` 类表示当前活动页面。```aria-current="page"``` 是一个辅助功能属性，用于指示当前所在的页面。```href="{{ .Permalink }}"``` 表示链接的目标地址，使用 ```.Permalink``` 作为链接的URL，该值应该是循环中当前菜单项的属性或变量。

    ```{{ .Name }}```：用于插入当前菜单项的名称。

    这段代码会根据 ```.Site.Menu.main``` 中的菜单项生成多个导航菜单项的 ```HTML``` 结构，其中每个菜单项的链接目标地址使用 ```.Permalink``` 属性，菜单项的显示文本使用 ```.Name``` 属性。循环会遍历所有菜单项，为每个菜单项生成一个导航菜单项的 HTML 结构。

6. 配置hugo.yaml
   增加導航參數代碼：
   ```json
    menu:
      main:
      - name: Home
        pageRef: /
        weight: 10
      - name: About
        pageRef: /About
        weight: 20
      - name: Gihub
        url: https://github.com/mcdaobage/
        weight: 30   
    ``` 

# 配置頁面模板

1. 在```C:\Users\gusao\Desktop\fuck\themes\daobaoge\layouts\page```路徑下，創建```about.html```文件。定義模塊，輸入如下代碼：
   ```html
   {{ define "main" }}
    <div>
    <h1>{{ .Title }}</h1>
    <p>{{ .Content }}</p>
    </div>

    {{ end }}
   ```

2. 在```C:\Users\gusao\Desktop\fuck\content\```創建一個```about.md```,增加```layout```參數
   
   ```bash
   ---
    title: "About"
    date: 2023-07-16T17:44:23+08:00
    draft: False
    layout: "about"
    ---

    # layout: "about" 指定了该内容应该使用名为 "about" 的布局模
   ```

3. 在```C:\Users\gusao\Desktop\fuck\themes\daobaoge\static\css```路徑下，創建```main.css```文件,增加以下代碼：
   
   ```html

    <!-- local css -->
    <link rel="stylesheet" href="/css/main.css" />

   ```
  
  代碼解釋：

  ```<link>```：是 HTML 的元素标签，用于定义外部资源的关系，比如 CSS 样式表。

  ```rel="stylesheet"```：是 <link> 元素的一个属性，用于指定被链接资源的类型为样式表（stylesheet）。

  ```href="/css/main.css"```：是 <link> 元素的另一个属性，用于指定所链接的样式表文件的路径。在这个例子中，路径是 /css/main.css，表示该样式表文件位于名为 "css" 的文件夹下，文件名是 "main.css"

4. 在```C:\Users\gusao\Desktop\fuck\themes\daobaoge\static\css```路徑下創建文件夾```fronts```，用於存放字體樣式

5. 去網站 https://www.1001fonts.com/metropolis-font.html 下載字體，並且放在fronts文件夾內
   
6. 編輯```main.css```文件，代碼如下
   
   ```css
    @font-face {
        font-family: metropolis;
        src: url("/fronts/metropolis.regular.otf") format("opentype");
    }

    body {
        font-family: metropolis, sans-serif;
        font-size: 1.1rem;
        line-height: 1.5;
        font-kerning: normal;
    }

    /** bootstrap styles**/
    .navbar {
        background-color: #002538 !important;
    }

    .nav-link {
        color: white !important;
    }

    .active {
        background-color: rgba(255,255,255,0.05);
        border-radius: 0.5rem;
    }

    .nav-item {
        padding: 0.5rem 1rem
    }
   ```
7. 在```C:\Users\gusao\Desktop\fuck\themes\daobaoge\layouts\partials\header.html```路徑下，對header.html文件進行修改
   ```html
   <li class="nav-item">
                    <a class="nav-link {{ if $currentPage.IsMenuCurrent "main" . }} active {{ end }}" aria-current="page" href="{{ .URL }}">{{ .Name }}</a>
                </li>

    <!-- 點擊導航按鈕會高亮 -->
   ```

# 創建博客佈局

## 下載主頁卡片模塊

1. 在```C:\Users\gusao\Desktop\fuck\content```路徑下，創建```posts```文件夾。並且創建一些markdown格式的內容

2. 點擊網站： https://getbootstrap.com/docs/5.2/components/card/，複製```card```項目的代碼，如下圖，粘貼到以下路徑```C:\Users\gusao\Desktop\fuck\themes\daobaoge\layouts\partials\archive.html```的文件中
   
   ```html
    <div class="card" style="width: 18rem;">
      <img src="..." class="card-img-top" alt="...">
      <div class="card-body">
        <h5 class="card-title">Card title</h5>
        <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
        <a href="#" class="btn btn-primary">Go somewhere</a>
      </div>
    </div>
   ```
## 修改主頁卡片模塊

```html

```

### 代碼解釋

```go
{{ $pages := where site.RegularPages "Type" "in" site.Params.mainSections }}
```
```{{ $pages := ... }}```：这行代码声明了一个名为 ```$pages``` 的变量，并将后续的表达式结果赋值给它。在这个例子中，我们将筛选和过滤得到的页面存储在 $pages 变量中。

```where```：这是 ```Hugo``` 内置的函数之一。它用于在页面集合中进行条件筛选。在这里，我们将使用 ```where``` 函数来筛选 ```site.RegularPages``` 集合中的页面。

```site.RegularPages```：这是一个 ```Hugo`` 内置的变量，表示整个站点的常规页面（即非部分页面和其他特殊页面）的集合。它包含了网站中的所有常规页面。

```"Type"```：这是一个条件，指定了我们要在哪个字段上进行筛选。在这个例子中，我们将根据页面的 ```"Type"``` 字段进行筛选。

```"in"```：这是 ```where``` 函数的操作符，用于指定要匹配的条件类型。在这里，我们使用 "in" 来表示我们希望匹配的条件是在指定的集合中。

```site.Params.mainSections```：这是一个 Hugo 内置的变量，用于表示配置文件中定义的主要部分（main sections）的集合。它可以是一个数组，包含了配置文件中指定的主要部分的名称。

综上所述，这段代码的作用是筛选和过滤站点的常规页面（site.RegularPages），并将结果存储在 $pages 变量中。筛选的条件是页面的 "Type" 字段的值必须在配置文件中指定的主要部分的集合（site.Params.mainSections）中。换句话说，它筛选出符合特定 "Type" 值的页面，并将这些页面存储在 $pages 变量中供后续使用。

```go

{{ range (.Paginate $pages).Pages }}

```

这段代码使用 ```range``` 函数和 ```.Paginate``` 方法来迭代并显示分页列表中的每个页面。

```.Paginate``` 方法是 ```Hugo``` 的内置方法之一，用于对页面进行分页处理。它接收一个页面集合作为参数，并返回一个分页对象。在这里，我们将 ```$pages``` 变量作为参数传递给 ```.Paginate``` 方法，生成一个分页对象。

```(.Paginate $pages).Pages``` 表示分页对象中的页面列表。通过 ```.Pages```，我们可以访问到当前页的页面列表。

```range``` 函数用于迭代并循环遍历给定的列表或范围。在这里，我们使用 ```range``` 函数来遍历分页对象的页面列表，以便在模板中处理每个页面。

所以，第二段代码会对满足条件的页面进行分页处理，并在每一页中循环迭代和处理每个页面。

```go

{{ template "_internal/pagination.html" . }}

```

```{{ template ... }}```：这是 ```Hugo``` 的模板语法，用于调用其他模板文件并进行渲染。

```"_internal/pagination.html"```：这是调用的模板文件路径。在这里，我们调用了 ```Hugo``` 内部的 ```_internal/pagination.html``` 模板文件，该模板文件是 ```Hugo``` 内置的分页模板。

```.```：这是传递给被调用模板的上下文数据（context）。在这个例子中，```.``` 表示当前页面的上下文数据。

通过调用 ```"_internal/pagination.html"``` 模板文件，并将当前页面的上下文数据作为参数传递给它，```Hugo``` 将渲染并生成分页导航链接的 ```HTML```。

```Hugo``` 内置的 ```_internal/pagination.html``` 模板文件负责根据传入的上下文数据生成分页导航链接的 ```HTML``` 结构，并提供了一些默认样式和配置选项。你可以在项目的主题文件夹中找到该模板文件，并根据需要进行自定义和样式调整。

通过将这段代码放置在合适的位置，你可以在你的网站或页面中生成带有分页导航链接的分页效果，让用户可以方便地浏览和导航到其他分页的内容。

```html
{{ with .Params.tumbnail }}
    <div calss="col-md-4">
        <img src="{{.}}" class="img-fluid rounded-start" alt="...">
    </div>
{{ end }}
```

```{{ with .Params.tumbnail }} ... {{ end }}```：这是 ```Hugo``` 模板语法中的 ```with``` 语句，它用于创建一个作用域，在这个作用域中可以访问指定的变量或字段。

```.Params.tumbnail```：这是一个变量或字段的引用，它表示当前页面的 ```Params``` 字段中的 ```tumbnail``` 字段。```Params``` 字段用于访问当前页面的参数（参数是你在页面的 Front Matter 中定义的字段）。

在这个例子中，```with .Params.tumbnail``` 的作用是判断是否存在 ```tumbnail``` 参数。如果存在（即非空），则进入 ```with``` 语句的作用域，否则忽略 ```with``` 语句的内容。

在进入作用域后，将会生成一个包含图片的 <div> 元素。<div> 的类名为 ```"col-md-4"```，意味着它将占据父容器的 4 列（在使用 Bootstrap 框架的情况下）。<img> 元素的 ```src``` 属性指向 ```tumbnail``` 参数的值，```class``` 属性应用了一些样式类以实现图片的适应性和边框圆角化。

通过这段代码的使用，你可以根据页面的参数（在 Front Matter 中定义）来判断是否显示一个带有图片的 <div> 元素。这提供了一种根据具体情况动态生成内容的方式。



### 配置

1. 在```hugo.yaml```文件，增加參數```paginate: 5```，用於展示每頁的卡片數量。並且自動分頁展示