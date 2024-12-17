### Nginx Fancy Index 模块

![Build Status](https://travis-ci.com/aperezdc/ngx-fancyindex.svg?branch=master)

目录

- [简介](#简介)
- [要求](#要求)
- [构建](#构建)
- [示例](#示例)
- [主题](#主题)
- [指令](#指令)

#### 简介

Fancy Index 模块使得生成文件列表成为可能，类似于内置的 `autoindex` 模块，但添加了一些样式。该模块允许一定程度上的内容自定义：

- 自定义头部。可以是本地或远程存储。
- 自定义尾部。可以是本地或远程存储。
- 添加自己的 CSS 样式规则。
- 允许按名称（默认）、修改时间或大小排序；既可以升序（默认）也可以降序。

此模块设计用于与 Nginx_ 高性能开源 Web 服务器配合使用，该服务器由 Igor Sysoev 编写。

#### 要求

##### CentOS 7

对于使用官方稳定版 Nginx 仓库的用户，提供了包含动态模块的额外包仓库，并且其中包含 fancyindex 模块。

直接安装：

```sh
yum install https://extras.getpagespeed.com/redhat/7/x86_64/RPMS/nginx-module-fancyindex-1.12.0.0.4.1-1.el7.gps.x86_64.rpm
```

或者，先添加额外仓库（以便未来更新），然后安装模块：

```sh
yum install nginx-module-fancyindex
```

然后在 `/etc/nginx/nginx.conf` 中加载模块：

```nginx
load_module "modules/ngx_http_fancyindex_module.so";
```

##### 其他平台

在大多数其他情况下，您需要 Nginx_ 的源代码。任何从 0.8 系列开始的版本都应该可以工作。

为了使用 `fancyindex_header` 和 `fancyindex_footer` 指令，您还需要将 `ngx_http_addition_module` 构建到 Nginx 中。

#### 构建

1. 解压 Nginx_ 源代码：

    ```sh
    $ gunzip -c nginx-?.?.?.tar.gz | tar -xvf -
    ```

2. 解压 fancy index 模块的源代码：

    ```sh
    $ gunzip -c nginx-fancyindex-?.?.?.tar.gz | tar -xvf -
    ```

3. 进入包含 Nginx_ 源代码的目录，运行配置脚本并指定所需的选项，确保添加一个指向 fancy index 模块源代码目录的 `--add-module` 标志：

    ```sh
    $ cd nginx-?.?.?
    $ ./configure --add-module=../nginx-fancyindex-?.?.? \
       [--with-http_addition_module] [其他所需选项]
    ```

   从版本 0.4.0 开始，该模块也可以作为动态模块构建，使用 `--add-dynamic-module=…` 并在配置文件中添加 `load_module "modules/ngx_http_fancyindex_module.so";`

4. 构建并安装软件：

    ```sh
    $ make
    ```

   然后，以 `root` 用户身份运行：

    ```sh
    # make install
    ```

5. 使用模块的配置指令_ 配置 Nginx_。

#### 示例

您可以通过在 Nginx_ 配置文件中的 `server` 部分添加以下行来测试默认的内置样式：

```nginx
location / {
    fancyindex on;              # 启用 fancy indexes
    fancyindex_exact_size off;  # 输出人类可读的文件大小
}
```

#### 主题

以下主题展示了使用该模块可以实现的自定义程度：

- [主题](https://github.com/TheInsomniac/Nginx-Fancyindex-Theme) 由 @TheInsomniac 提供。使用自定义头部和尾部。
- [主题](https://github.com/Naereen/Nginx-Fancyindex-Theme) 由 @Naereen 提供。使用自定义头部和尾部，头部包含使用 JavaScript 过滤文件名的搜索字段。
- [主题](https://github.com/fraoustin/Nginx-Fancyindex-Theme) 由 @fraoustin 提供。使用 Material Design 元素的响应式主题。
- [主题](https://github.com/alehaa/nginx-fancyindex-flat-theme) 由 @alehaa 提供。基于 Bootstrap 4 和 FontAwesome 的简单扁平主题。

#### 指令

##### fancyindex

- **语法**: `fancyindex` [on | off]
- **默认值**: `fancyindex off`
- **上下文**: http, server, location
- **描述**: 启用或禁用 fancy 目录索引。

##### fancyindex_default_sort

- **语法**: `fancyindex_default_sort` [name | size | date | name_desc | size_desc | date_desc]
- **默认值**: `fancyindex_default_sort name`
- **上下文**: http, server, location
- **描述**: 定义默认的排序标准。

##### fancyindex_directories_first

- **语法**: `fancyindex_directories_first` [on | off]
- **默认值**: `fancyindex_directories_first on`
- **上下文**: http, server, location
- **描述**: 如果启用（默认设置），将目录分组并在所有普通文件之前排序。如果禁用，目录将与文件一起排序。

##### fancyindex_css_href

- **语法**: `fancyindex_css_href uri`
- **默认值**: `fancyindex_css_href ""`
- **上下文**: http, server, location
- **描述**: 允许在生成的列表中插入链接到 CSS 样式表。提供的 `uri` 参数将原样插入到 `<link>` HTML 标签中。链接插入在内置 CSS 规则之后，因此您可以覆盖默认样式。

##### fancyindex_exact_size

- **语法**: `fancyindex_exact_size` [on | off]
- **默认值**: `fancyindex_exact_size on`
- **上下文**: http, server, location
- **描述**: 定义如何表示目录列表中的文件大小；准确表示或四舍五入到千字节、兆字节和吉字节。

##### fancyindex_name_length

- **语法**: `fancyindex_name_length length`
- **默认值**: `fancyindex_name_length 50`
- **上下文**: http, server, location
- **描述**: 定义文件名的最大长度限制（以字节为单位）。

##### fancyindex_footer

- **语法**: `fancyindex_footer path` [subrequest | local]
- **默认值**: `fancyindex_footer ""`
- **上下文**: http, server, location
- **描述**: 指定应插入到目录列表底部的文件。如果设置为空字符串，则发送模块提供的默认页脚。可选参数指示 `path` 是否应被视为使用子请求加载的 URI（默认），或是否引用本地文件。

**注意**: 使用此指令需要将 `ngx_http_addition_module` 构建到 Nginx 中。

**警告**: 当插入自定义头部/页脚时，将发出子请求，因此可以使用任何 URL 作为源。虽然外部 URL 可以工作，但仅支持内部 URL。外部 URL 完全未经测试，使用它们将导致 Nginx 在等待子请求完成时阻塞。如果您认为外部头部/页脚是必须的，请联系我 <mailto:aperez@igalia.com>。

##### fancyindex_header

- **语法**: `fancyindex_header path` [subrequest | local]
- **默认值**: `fancyindex_header ""`
- **上下文**: http, server, location
- **描述**: 指定应插入到目录列表顶部的文件。如果设置为空字符串，则发送模块提供的默认头部。可选参数指示 `path` 是否应被视为使用子请求加载的 URI（默认），或是否引用本地文件。

**注意**: 使用此指令需要将 `ngx_http_addition_module` 构建到 Nginx 中。

##### fancyindex_show_path

- **语法**: `fancyindex_show_path` [on | off]
- **默认值**: `fancyindex_show_path on`
- **上下文**: http, server, location
- **描述**: 是否输出路径和关闭的 `</h1>` 标签在头部之后。当您希望使用 PHP 脚本处理路径显示时，这非常有用。

**警告**: 仅当使用 `fancyindex_header` 提供自定义头部时，才能关闭此指令。

##### fancyindex_show_dotfiles

- **语法**: `fancyindex_show_dotfiles` [on | off]
- **默认值**: `fancyindex_show_dotfiles off`
- **上下文**: http, server, location
- **描述**: 是否列出以点开头的文件。常规做法是隐藏这些文件。

##### fancyindex_ignore

- **语法**: `fancyindex_ignore string1 [string2 [... stringN]]`
- **默认值**: 无默认值
- **上下文**: http, server, location
- **描述**: 指定生成的列表中不显示的文件名列表。如果 Nginx 是使用 PCRE 支持构建的，字符串将被解释为正则表达式。

##### fancyindex_hide_symlinks

- **语法**: `fancyindex_hide_symlinks` [on | off]
- **默认值**: `fancyindex_hide_symlinks off`
- **上下文**: http, server, location
- **描述**: 启用后，生成的列表中将不包含符号链接。

##### fancyindex_hide_parent_dir

- **语法**: `fancyindex_hide_parent_dir` [on | off]
- **默认值**: `fancyindex_hide_parent_dir off`
- **上下文**: http, server, location
- **描述**: 启用后，将不显示父目录。

##### fancyindex_localtime

- **语法**: `fancyindex_localtime` [on | off]
- **默认值**: `fancyindex_localtime off`
- **上下文**: http, server, location
- **描述**: 启用后，显示文件时间为本地时间。默认为 GMT 时间。

##### fancyindex_time_format

- **语法**: `fancyindex_time_format` string
- **默认值**: `fancyindex_time_format "%Y-%b-%d %H:%M"`
- **上下文**: http, server, location
- **描述**: 用于时间戳的格式字符串。格式说明符是 `strftime` 函数支持的子集，行为与区域设置无关（例如，星期几和月份名称始终为英文）。支持的格式有：

  - `%a`: 星期几的缩写名称。
  - `%A`: 星期几的完整名称。
  - `%b`: 月份的缩写名称。
  - `%B`: 月份的完整名称。
  - `%d`: 月份中的日期（范围 01 到 31）。
  - `%e`: 类似于 `%d`，月份中的日期，但前导零被空格替换。
  - `%F`: 等同于 `%Y-%m-%d`（ISO 8601 日期格式）。
  - `%H`: 24 小时制的小时数（范围 00 到 23）。
  - `%I`: 12 小时制的小时数（范围 01 到 12）。
  - `%k`: 24 小时制的小时数（范围 0 到 23）；单个数字前有空格。
  - `%l`: 12 小时制的小时数（范围 1 到 12）；单个数字前有空格。
  - `%m`: 月份的数字（范围 01 到 12）。
  - `%M`: 分钟数（范围 00 到 59）。
  - `%p`: 根据给定的时间值，显示 "AM" 或 "PM"。
  - `%P`: 类似于 `%p`，但小写： "am" 或 "pm"。
  - `%r`: 上午或下午的时间表示法。等同于 `%I:%M:%S %p`。
  - `%R`: 24 小时制的时间表示法（`%H:%M`）。
  - `%S`: 秒数（范围 00 到 60）。
  - `%T`: 24 小时制的时间表示法（`%H:%M:%S`）。
  - `%u`: 星期几的数字，范围 1 到 7，周一为 1。
  - `%w`: 星期几的数字，范围 0 到 6，周一为 0。
  - `%y`: 不带世纪的年份数字（范围 00 到 99）。
  - `%Y`: 带世纪的年份数字。

[nginx]: https://nginx.org