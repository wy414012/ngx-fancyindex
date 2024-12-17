# 变更日志
所有对本项目的重要变更都将记录在此文件中。

## [未发布]

## [0.5.2] - 2021-10-28
### 修复
- 正确转义文件名以确保文件名不会被渲染为HTML。（补丁由 Anthony Ryan <<anthonyryan1@gmail.com>> 提供，[#128](https://github.com/aperezdc/ngx-fancyindex/pull/128).）

## [0.5.1] - 2020-10-26
### 修复
- 正确处理 `fancyindex_header` 和 `fancyindex_footer` 的可选第二个参数
  ([#117](https://github.com/aperezdc/ngx-fancyindex/issues/117))。

## [0.5.0] - 2020-10-24
### 增加
- 新选项 `fancyindex_show_dotfiles`。（路径由 Joshua Shaffer <<joshua.shaffer@icmrl.net>> 提供。）
- `fancyindex_header` 和 `fancyindex_footer` 选项现在支持通过 `local` 标志正确使用本地文件。（补丁由 JoungKyun Kim <<joungkyun@gmail.com>> 和 Adrián Pérez <<aperez@igalia.com>> 提供。）

### 改变
- 改进目录条目排序的性能，对于包含数千个文件的目录应该有显著提升。（补丁由 [Yuxiang Zhang](https://github.com/z4yx) 提供。）
- 模块支持的最低 Nginx 版本现在是 0.8.x。

### 修复
- 在使用旧版本 Nginx 构建模块时，正确转义目录条目名称中的方括号。（补丁由 Adrián Pérez <<aperez@igalia.com>> 提供。）
- 修复使用 [nginx-auth-ldap](https://github.com/kvspb/nginx-auth-ldap) 模块时目录条目列表不显示的问题。（补丁由 JoungKyun Kim <<joungkyun@gmail.com>> 提供。）

## [0.4.4] - 2020-02-19
### 增加
- 新选项 `fancyindex_hide_parent_dir`，禁用在列表中生成父目录链接。（补丁由 Kawai Ryota <<admin@mail.kr-kp.com>> 提供。）

### 改变
- 每个表格行现在用新行（实际上是 `CRLF` 序列）分隔，这使得使用简单文本工具解析输出更加容易。（补丁由 Anders Trier <<anders.trier.olesen@gmail.com>> 提供。）
- 对 `README` 文件进行了一些修正和补充。（补丁由 Nicolas Carpi <<nicolas.carpi@curie.fr>> 和 David Beitey <<david@davidjb.com>> 提供。）

### 修复
- 在模板中使用正确的字符引用表示 `&` 字符。（补丁由 David Beitey <<david@davidjb.com>> 提供。）
- 正确编码用作 URI 组件的文件名。

## [0.4.3] - 2018-07-03
### 增加
- 表格单元格现在有类名，允许更好的 CSS 样式化。（补丁由 qjqqyy <<gyula@nyirfalvi.hu>> 提供。）
- 测试套件现在可以解析并检查模块返回的 HTML 元素，感谢 [pup](https://github.com/EricChiang/pup) 工具。

### 修复
- 按文件大小排序现在工作正常。（补丁由 qjqqyy <<gyula@nyirfalvi.hu>> 提供。）

## [0.4.2] - 2017-08-19
### 改变
- 默认模板生成的 HTML 现在是标准的 HTML5，并且应该可以通过验证 (#52)。
- 当使用 `fancyindex_exact_size off` 时，文件大小现在有小数位。（补丁由 Anders Trier <<anders.trier.olesen@gmail.com>> 提供。）
- 多次更新 `README.rst`。（补丁由 Danila Vershinin <<ciapnz@gmail.com>>、Iulian Onofrei、Lilian Besson 和 Nick Geoghegan <<nick@nickgeoghegan.net>> 提供。）

### 修复
- 按文件大小排序现在也适用于包含大于 `INT_MAX` 的文件大小的目录。（#74，修复建议由 Chris Young 提供。）
- 定制的头部未声明 UTF-8 编码不再导致浏览器错误地渲染表头箭头 (#50)。
- 修复打开包含空文件的目录时发生段错误 (#61，补丁由 Catgirl <<cat@wolfgirl.org>> 提供。）

## [0.4.1] - 2016-08-18
### 增加
- 新的 `fancyindex_directories_first` 配置指令（默认启用），允许设置目录是否在其他文件之前排序。（补丁由 Luke Zapart <<luke@zapart.org>> 提供。）

### 修复
- 修复当使用 fancyindex 模块时索引文件不起作用的问题 (#46)。

## [0.4.0] - 2016-06-08
### 增加
- 模块现在可以作为 [动态模块](https://www.nginx.com/resources/wiki/extending/converting/) 构建。（补丁由 Róbert Nagy <<vrnagy@gmail.com>> 提供。）
- 新的配置指令 `fancyindex_show_path`，允许隐藏包含当前路径的 `<h1>` 标题。（补丁由 Thomas P. <<tpxp@live.fr>> 提供。）

### 改变
- 列表中的目录和文件链接现在有 `title="..."` 属性。（补丁由 `@janglapuk` <<trusdi.agus@gmail.com>> 提供。）

### 修复
- 修复与 `ngx_pagespeed` 模块一起使用时请求挂起的问题。（补丁由 Otto van der Schaaf <<oschaaf@we-amp.com>> 提供。）

## [0.3.6] - 2016-01-26
### 增加
- 新功能：允许使用 `fancyindex_hide_symlinks` 配置指令过滤掉符号链接。（想法和原型补丁由 Thomas Wemm 提供。）
- 新功能：允许使用 `fancyindex_time_format` 配置指令指定时间戳格式。（想法由 Xiao Meng <<novoreorx@gmail.com>> 提出。）

### 改变
- 顶级目录的列表将不再生成“父目录”链接作为列表的第一个元素。（补丁由 Thomas P. 提供。）

### 修复
- 修复 `fancyindex_css_href` 设置在嵌套位置内的传播和覆盖问题。
- 对代码进行了一些小的更改，以便在 Windows 下使用 Visual Studio 2013 清洁构建。（补丁由 Y. Yuan <<yzwduck@gmail.com>> 提供。）

## [0.3.5] - 2015-02-19
### 增加
- 新功能：允许使用 `fancyindex_default_sort` 配置指令设置默认排序标准。（补丁由 Алексей Урбанский 提供。）
- 新功能：允许使用 `fancyindex_name_length` 配置指令更改文件名的最大长度。（补丁由 Martin Herkt 提供。）

### 改变
- 将 `NEWS.rst` 重命名为 `CHANGELOG.md`，遵循 [Keep a Change Log](http://keepachangelog.com/) 的推荐。
- 如果没有 `http_addition_module` 配置 Nginx，将在配置期间生成警告，因为 `fancyindex_footer` 和 `fancyindex_header` 指令需要它。

## [0.3.4] - 2014-09-03
### 增加
- 生成的 HTML 中定义了视口，这在移动设备上表现更好。

### 改变
- 使用 :nth-child() 将偶数-奇数行样式移至 CSS，这使得客户端接收的 HTML 更小。

## [0.3.3] - 2013-10-25
### 增加
- 新功能：默认模板中的表头现在可点击以设置索引条目的排序标准和方向。
  （https://github.com/aperezdc/ngx-fancyindex/issues/7）

## [0.3.2] - 2013-06-05
### 修复
- 解决某些客户端会永远停滞的 bug。
- 改进非内置头部/尾部的子请求处理。

## [0.3.1] - 2011-04-04
### 增加
- `NEWS.rst` 文件，用作变更日志。

[未发布]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.5.2...HEAD
[0.5.2]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.5.1...v0.5.2
[0.5.1]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.5.0...v0.5.1
[0.5.0]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.4.4...v0.5.0
[0.4.4]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.4.3...v0.4.4
[0.4.3]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.4.2...v0.4.3
[0.4.2]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.4.1...v0.4.2
[0.4.1]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.4.0...v0.4.1
[0.4.0]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.6...v0.4.0
[0.3.6]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.5...v0.3.6
[0.3.5]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.4...v0.3.5
[0.3.4]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.3...v0.3.4
[0.3.3]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.2...v0.3.3
[0.3.2]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3.1...v0.3.2
[0.3.1]: https://github.com/aperezdc/ngx-fancyindex/compare/v0.3...v0.3.1