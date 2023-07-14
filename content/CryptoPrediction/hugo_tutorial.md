---
title: "Hugo_tutorial"
date: 2023-07-10T09:42:33+08:00
draft: false
ShowToc: true # 开启 Table Of Content 功能
TocOpen: true # 展开 Table Of Content
---


# hugo 文件夾解釋

dirctory Structure

```bash
C:.
├─archetypes
├─assets
├─content
├─data
├─layouts
├─static
└─themes
```
## archetypes 文件夾

用於定義網頁`內容`模板，也就是markdown格式，例如文章目錄，文章作者署名，等等

## content 文件夾

用於存放網頁文件，所有創建的網頁都在這個文件夾中。

## Date 文件夾

用於存儲Jason文件，用於公告欄的監控，採集其他網站的數據，用於動態監控

## layouts

用於網頁的`前端`佈局設置，例如網頁的頁眉頁腳，批量設置，相關推薦，側邊欄，導航欄

## static

用於存放文章裡面的配圖，網站logo，cs，js等文件

## themes 文件夾

用於存放主題模板

## hugo.toml（config.toml）

網站的主設置頁面：定義網站的標題，網站的語言，網站的url，網站的分類目錄

# 安裝hugo模板

## 官方hugo模板GitHub地址

https：//themes.gohugo.io

## 教程hugo模板GitHub地址

https://github.com/giraffeacademy/ga-hugo-theme

## 安裝模板

使用命令：
```bash
cd themes
#移動到主題文件夾themes

git clone https://github.com/giraffeacademy/ga-hugo-theme.git
#下載主題

mv ga-hugo-theme ga-hugo
#修改文件夾名字

```

# 創建內容

## 創建頁面

使用命令：
```bash
hugo new content/a.md

hugo new dir1/b.md

#更改conent字段，會自動在content文件夾下，創建一個新的文件夾,第二個命令就會在content文件夾創建一個dir1文件夾，裡面包含一個b.md的文件

#也是創建分類目錄的方式，在分類dir1的目錄下，有網頁b.md
```

## 預覽頁面

使用命令：
```bash
hugo server -D

```
## 錯誤修正

### 修改1：

```bash
<a href="{{.URL}}">{{.Title}}</a>

<a href="{{.Permalink}}">{{.Title}}</a>

```
对于所有 Hugo 新手和刚刚安装的用户，请将主题文件夹中的 list.html 从 .URL 更改为 .Permalink，否则文件将无法加载。

### 修改2：

在```hugo.toml```文件中，增加字段

```bash
theme = 'ga-hugo'
#設置模板路徑
```

## 創建二級目錄URL

需要再二級目錄文件下創建一個```_index.md```的索引文件，也就是目錄頁，才會顯示二級目錄的url，同時也會自動生成一個列表頁

例如：http://localhost:1313/dir1/dir2
則需要在dir2的文件夾中創建一個```_index.md```，才能出現這條url

# Markdown 頭部文件設置

用於設置文章的標題，文章章節列表，作者所有權

## 嘗試用的配置

title:
author: 作者設置
ShowToc: true 开启 Table Of Content 
TocOpen: true 展开 Table Of Content

可以在```archetypes```下的```default.md```統一配置好後，所有的文章都會自動配置

# 短代碼

## 網頁中嵌入短視頻

{{<youtube 視頻ID>}}

## 錨文本

[链接文本](链接URL)

# 分類法

## 類目分類法

參考上方創建二級目錄URL的方法

## 關鍵詞分類法

在文章頭部設置輸入
```bash
tags: ["tag1","tag2","tag3"]
```
```bash
categories: ["cat1"]
```
## 自定義分類法

在```hugo.toml```中增加字段
```bash
[taxonomies]
    tag = "tags"
    category = "categories"
    mood = "moods"
#tag和category是默認的字段，mood是增加的字段，但是需要一起寫出來
```

## 錯誤更正

錯誤提示：標籤頁，分類頁，顯示 page not found

解決辦法：使用命令
```bash
Ctrl+C
#退出預覽

hugo server -D
#啟動預覽
```

# hugo 模板

一般包含兩個模板頁，單頁和列表頁，這些建立好後，所有的頁面就執行這兩個模板來建立

## 列表頁模板

只是在分類文件中，創建的```_index.md```文件，它調用的模板一般是```themes/layouts/_default```路徑下的```list.html```文件

### hugo變量

```bash
{{ .Content }}
{{ .title }}
{{ .date }}

{{ .Params.myVar}}

#如果是自定義的參數myVar，需要在前面加上.Parmas
```
讀取```_index.md```文件，的內容部分，展示在列表頁中

### hugo函數

```bash
{{.Content}}
#讀取 _index.md 文件的內容部分，展示在列表頁中

{{ range .Pages }}
    {{.Title}} <br>
{{end}}
#遍歷當前目錄的所有標題，如果當前的目錄包括文件夾，那麼也會讀取文件夾名
```

```bash
{{.Content}}
#讀取 _index.md 文件的內容部分，展示在列表頁中

{{ range .Pages }}
    <ul>
        <li><a href="{{.Permalink}}">{{.Title}}</a></li>
    </ul>
{{end}}
#{{ range .Pages }}{{end}}是循環體
#<ul><ul>部分是提取url和標題，構造超鏈接
#教程中使用的是{{.url}},要改成{{.Permalink}}
```

## 單頁模板

就是平常創建的單頁，它調用的模板一般是```themes/layouts/_default```路徑下的```single.html```文件

常用的就是H1標籤，和H2標籤

```bash

<body>
    <h1>{{.Title}}</h1>
    <hr>
    {{.Content}}
    <hr>
    <h2>footer</h2>

</body>
#<h1></h1>讀取title的變量
```

## 首頁模板

首頁模板實際上就是列表模板，在```layout/_defalt```路徑下創建```index.html```文件即可

## 多模板應用

APP下載站，站中站，養其他站，開發其他的站的應用

只需要在```layout/_defalt```路徑下創建一個文件夾，名字要和```congtent```下的文件夾名相同，那麼該文件夾下的所有網頁，都會讀取新的模板。

例如我在```layout/_defalt/dri1```路徑下，創建了一個文件```single.html```,並且使用新的模板，則```congtent/dir1```路徑下的所有```single.html```，都會使用新的模板，此路徑下的列表頁不會變。

## 基本模板和模塊

所有頁面，包括首頁，單頁，列表頁的共同模塊設置，例如：導航欄，網頁底部的友情鏈接區域

邏輯是首先在```baseof.html```會分成兩部分，一個是公共模塊，就是所有的頁面都會呈現的模塊，另一個就是自定義區域，這個區域的內容，由各個頁面自定義生成，這樣子每一個頁面既有公用的模塊，也有自身頁面的獨特的內容。

### 定義模塊

```baseof.html```文件代碼

```bash
<html>
<head>
     <meta charset="UTF-8">
     <title>Document</title>
</head>
<body>
    Top of baseof
    <hr>
    {{ block "main" . }}
    
    {{end}}
    <hr>
    bottom of baseof
</body>
</html>
#這裡的 top of baseof和 bottom of baseof 其他頁面都會出現，包括單頁和列表頁

#{{ block "main" . }} {{end}},就是自定義模塊，其他的頁面可以通過對這個模塊賦值，從而呈現不同的內容

```

### 模塊複製

```single.html```文件代碼，通過對 ```mian```的賦值，則在頁面呈現其他不一樣的內容

```bash
{{ define "main" }}
this is the single template，哈哈哈哈

{{end}}

#這裡就是通過{{ define "main" }}，對 baseof.html 裡面 {{ block "main" . }} 模塊進行賦值，賦值的內容就是：this is the single template，哈哈哈哈
```

# Hugo函數

詳細的函數指令，訪問:https://gohugo.io/functions

函數的基本構造格式，函數名，後面是參數

{{ funName param1 param2 }}，例如：

```bash
{{ truncate 10 "this is a really long string"}}

#只是一個截斷函數的表達式，截斷把 "this is a really long string"語句，截斷前10個字符
```

```bash

{{ range .Pages }}
    {{ .Title }} <br>
{{ end }}

#遍歷頁面上的所有標題
```

# 判斷語句

```bash
{{ $var1 := "dog" }}
{{ $var2 := "cat" }}

{{ if eq $var1 $var2 }}
    True
{{else}}
    False
{{end}}

#eq 是運算規則，判斷 $var1是否等於$var2
```

```bash
{{ $var1 := 1 }}
{{ $var2 := 2 }}
{{ $var3 := 3 }}

{{ if and (lt $var1 $var2) (lt $var1 $var3)}}
    True
{{else}}
    False
{{end}}
#小括號內的內容優先預算，那麼這條的函數的意思就是，如果 $var1小於$var2，和，$var1小於$var3，輸出True
```

```bash
{{ $title := .title}}
{{ range .Site.Pages }}
    <li><a href="{{.Permalink}}" style="{{ if eq .title $title}} background-color:yellow;{{end}}">{{.title}}</a></li>
{{end}}

#{{ range .Site.Pages }} 遍歷整個網站的頁面的意思
```

# hugo 數據文件

用於保存json文件，通過命令調用

```bash
{{ range .Site.Data.states }}
    {{.name}} <br> {{.capital}} <br>

{{end}}

#{{ range .Site.Data.states }} 遍歷數據庫中的states.jason
#{{.name}} <br> {{.capital}} 調出數據庫中的name，和capital標籤

```

# 局部模板

用於導航模塊，頁眉頁腳，例如：

1. 在```layouts/partials```路徑下，建立文件```header.html```，在```header.html```文件建立模塊：
```bash
    <h1>{{.Title}}</h1>
    <p>{{.Date}}</p>
    <hr>
    <br>

    #模塊中引用hugo常量，markdown文件的title，和日期
```



2. 在```single```文件引用模塊
```bash
    {{ partial "header" .}}
    <h1>Single Template</h1><hr><br>
```

# 短代碼
1. 在```layouts/shortcodes```路徑下，建立文件```myshortcode.html```
   
2. 直接在文件使用命令```{{< myshortcode >}}```,即可直接引用```myshortcode.html```裡面的內容

3. 在文件```myshortcode.html```，輸入代碼
   
```bash
   <p style="color:{{.Get`color`}}">This is the frameworks text</p>

   #風格顏色設置，使用短代碼:{{.Get`color`}},發出請求，可以根據輸入的參數，輸出需要的顏色
```

4. 直接在文件使用命令```{{< myshortcode color="blue">}}```,即可直接引用```myshortcode.html```裡面的內容,同時輸出的字體顏色是藍色


## 視頻錯誤修正
在引用CSS的時候，不能使用"",代碼修正後：
```bash
<p style='color: {{ .Get "color" }}'>This is the frameworks text</p>

```






# 進貨技能

## 垂直領域信息源（收集整理）
採集：火車頭，八爪魚，愛站api，5118api

有些項目是不需要採集頭部賬號，直接批量上外鏈
no deposit casino

## 域名來源
## 服務器來源
## 對應的工具使用

# 變現渠道

## 統計廣告
## Google聯盟
## 其他營銷聯盟