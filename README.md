# 一、cnblogs_automatic_blog_uploading


基于rpcxml协议，利用githook，在commit时自动发布本地markdown文章到博客园，如文章已发布，则会更新。

# 二、项目地址


项目地址：<a target='_blank' href='https://github.com/nickchen121/cnblogs_automatic_blog_uploading'>https://github.com/nickchen121/cnblogs_automatic_blog_uploading</a>

# 三、参考效果


参考效果：<a target='_blank' href='https://www.cnblogs.com/nickchen121/p/10718112.html'>https://www.cnblogs.com/nickchen121/p/10718112.html</a>

# 四、使用说明


本脚本用`python3.+`编写，请配置好运行环境。

1. 第一次使用前先把`./hooks/commit-msg`文件复制到`./.git/hooks/`中（如有则无需修改）。
2. 运行`cnblogs.py`：
    1. 程序有一个可选参数（如无特殊需求不要添加参数）。
        - `config` 设置博客信息。
        - `download` 下载文章。
    2. 第一次运行`cnblogs.py`时默认选择`config`参数，设置博客信息，会生成一个`blog_config.json`文件（文件内有博客园账号密码，小心使用）。
    3. 此后每次运行程序时，`./articles/*.md`将被上传到博客并发布；`./unpublished/*.md`将被上传到博客，但不发布（并标注分类“unpublished”）。文章均**以文件名为题**，且不发布的文章。**如果博客中已经存在同名文章，将替换其内容！**
3. 编辑`./articles/`，`./unpublished/`中markdown文件，在本地git仓库`commit`更改，自动运行`./cnblogs.py`（需要使用终端命令才能查看返回信息）。

# 五、其他脚本


## 5.1 md文档添加索引


自动给md文档添加索引，即：

```python
# 一级标题

## 二级标题
```
变为

```python
# 一、一级标题

## 1.1 二级标题
```

## 5.2 取出文件名序号


如果你的md文件为`01 第一篇md.md`/`02 第一篇md.md`/`03 第一篇md.md`，则会变成`第一篇md.md`/`第一篇md.md`/`第一篇md.md`

## 5.3 批量修改文档内容


选择特定文件目录，批量修改文件下文件的内容，**小心使用**。

## 5.4 生成目录


根据特定的字符串，生成特定的目录结构，可以参考：<a target='_blank' href='https://www.cnblogs.com/nickchen121/p/10718112.html'>https://www.cnblogs.com/nickchen121/p/10718112.html</a>

## 5.5 读取title_postid文件


博客上传成功后，会生成一个`title_postid.json`文件，里面保存了发布成功文件的信息。

# 六、注意事项（已知Bug）


1. **本程序不保证稳定性**，为防止数据丢失，建议使用前预先备份博客。
2. clone仓库不能下载`.git`文件夹，因此需要手动复制调用`cnblogs.py`的脚本`./hooks/commit-msg`到`.git`。
3. 由于metaWeBlog本身没有提供查看文章是否已发布的接口，所有使用“unpublished”分类标注未发布文章。也就是说，**当执行`python cnblogs.py download`命令时，博客中没有发布也没有“unpublished”分类的文章也会存到`./articles/`，下次运行时将被自动发布。**
4. 由于接口不允许将已经发布的文章设置为未发布，所以若`./unpublished/`内的文章在博客内有同名文章时不会被上传。