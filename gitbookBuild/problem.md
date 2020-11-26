##搭建gitbook过程中遇到的错误

- 报错1
```
Error: ENOENT: no such file or directory, stat '/var/www/http/book/_book/gitbook/gitbook-plugin-highlight/ebook.cs
```
网上找解决方案,修改copyPluginAssets.js里的第112行, confirm: true 为 confirm: false
先用 ```locate copyPluginAssets.js```命令找到copyPluginAssets.js文件位置,然后编辑即可
```
$ locate copyPluginAssets.js
/home/ubuntu/.gitbook/versions/3.2.3/lib/output/website/copyPluginAssets.js
$ sudo vim /home/ubuntu/.gitbook/versions/3.2.3/lib/output/website/copyPluginAssets.js
```

- 报错2
```
Error: Couldn't locate plugins "splitter, expandable-chapters-small, anchors, github, github-buttons, donate, sharing-plus, anchor-navigation-ex, favicon",
Run 'gitbook install' to install plugins from registry.
```
book.json里加了插件,没安装,运行```gitbook install``` 安装一下就好了

- 报错3
```
(node:25401) [DEP0066] DeprecationWarning: OutgoingMessage.prototype._headers is deprecated
```
这个报错并没有什么影响,原因是我的node版本是12.14.0,要解决的话,换成10.15.3就好了
[切换node版本方法](https://www.jianshu.com/p/6f5e0da85dc1)
