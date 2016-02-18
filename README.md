# JUMA 文档中心

JUMA 的文档中心基于 [MkDocs](http://mkdocs.org/) 制作。所有文档的源文件全部位于 `sources/` 目录下，并都为 Markdown 格式。

所有生成的文档页面全部托管于 [www.juma.io/doc/](www.juma.io/doc/)，并与本项目仓库保持实时同步。


## 帮助完善 JUMA 的文档
如果想要帮助 JUMA 完善文档，请查看：[CONTRIBUTING.md](CONTRIBUTING.md)。


## 本地运行

1\. 安装所需的 MkDocs

```
$ pip install 'mkdocs'
```

2\. 进入本项目仓库根目录，并执行 `mkdocs serve`

```
$ cd /path/to/this
$ mkdocs serve
```

3\. 使用浏览器访问：

```
http://127.0.0.1:8000/

```

如果访问不了，请尝试：
```
http://127.0.0.1:8000/basics/
```

4\. 生成网页
```
$ mkdocs build
```

## License
所有 `source/` 目录中的内容，授权方式均为 [CC:BY-NC-SA](http://creativecommons.org/licenses/by-nc-sa/4.0/)
