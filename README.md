# quill-image-super-solution-module

![npm](https://img.shields.io/npm/v/quill-image-super-solution-module)
![npm](https://img.shields.io/npm/dt/quill-image-super-solution-module)
![GitHub](https://img.shields.io/github/license/EthanYan6/quill-image-super-solution-module)
![GitHub Repo stars](https://img.shields.io/github/stars/EthanYan6/quill-image-super-solution-module?style=social)
![GitHub forks](https://img.shields.io/github/forks/EthanYan6/quill-image-super-solution-module?style=social)
![GitHub top language](https://img.shields.io/github/languages/top/EthanYan6/quill-image-super-solution-module)
![GitHub issues](https://img.shields.io/github/issues/EthanYan6/quill-image-super-solution-module)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/EthanYan6/quill-image-super-solution-module)



## 简介 Introduction

此模块为富文本编辑器 `vue-quill-editor` 的专用插件，为其提供了自定义上传图片到服务器、**粘贴**图片上传至服务器、**拖拽**图片上传至服务器的功能。支持与其他模块一起使用。

This module is a special plug-in for the rich text editor `vue-quill-editor`, which provides custom upload pictures to the server, **paste** pictures to the server, and **drag** pictures to the server. Features. Supports use with other modules.

> 富文本编辑器原先实现方式为：将图片编码为 `base64` 的加密字符串，然后在提交数据时，此表单字段保存一个大的 `html` 内容，因此可能受网络原因，数据库限制等等外界因素导致失败。而且当我们需要这些图片资源时，不容易进行归档，因此才需要此模块。
>
> The original implementation of the rich text editor is: encode the picture into an encrypted string of `base64`, and then when submitting the data, this form field saves a large `html` content, so it may be due to network reasons, database restrictions, etc. Factors lead to failure. And when we need these image resources, it is not easy to archive, so this module is needed.

## 亮点 Highlights Show

1. 整合了粘贴、拖拽等功能，减少安装插件个数 Integrated functions such as paste, drag and drop, reduce the number of plug-ins installed
2. 解决其他插件都存在的一个 `bug` ：插入图片时，光标位置错误 Solve a bug in other plugins: the cursor position is wrong when inserting pictures
3. 解决其他插件粘贴图片时，显示不正常的 `bug` Solve the problem of displaying abnormal `bug` when other plugins paste pictures
4. 解决其他插件上传文件类型不加限制的 `bug` Solve the `bug` that other plugins upload file types without restrictions
5. 粘贴图片与拖拽图片也实现了上传至服务器的功能 Pasting pictures and dragging pictures also realize the function of uploading to the server

## 功能 Features

-   Version 1.0.1

    1. 插入、粘贴、拖拽图片上传至服务器，自定义上传路径 Insert, paste, drag and drop pictures to upload to the server, customize the upload path
    2. 限制图片大小功能 Limit image size function
    3. 限制上传图片类型 Restrict upload image types
    4. 显示上传进度 Show upload progress
    5. 显示上传成功或失败 Show upload success or failure
    6. 支持与其他插件共同使用 Supports common use with other plugins

-   Version 1.0.2

    1. 更改说明文档，修复说明文档中示例的错误 Change the description document and fix the errors in the examples in the description

-   Version 1.0.3
  
    1. 解决编辑器中不能粘贴文本的 `bug` Solve the `bug` that cannot paste text in the editor
    
-   ~~Version 1.0.4~~
  
    1. ~~Npm 模块说明文档中的 `keyword` 设置错误，对其进行修正。 The `keyword` setting error in the Npm module description document is corrected.~~

-   Version 2.0.0
  
    1. 没更新什么，就是想发个大版本玩玩 Nothing updated, just want to send a big version for fun

-   Version 2.0.1
  
    1. 解决当复制多行文本且文本中有回车时，会出现多余空白行的 `bug` Solve the `bug` that extra blank lines will appear when copying multiple lines of text and there is a carriage return in the text

## 安装 installation

```shell
# yarn （建议使用，高速下载 Recommended, high-speed download）
yarn add quill-image-super-solution-module
# npm
npm install quill-image-super-solution-module --save-dev
```

## 引入 Introduce

```shell
import { container, ImageExtend, QuillWatch } from "quill-image-super-solution-module";
Quill.register("modules/ImageExtend", ImageExtend);
```

## 使用示例 Example

```vue
<template>
    <div class="quill-example">
        <quill-editor v-model="example" :options="editorOption"> </quill-editor>
    </div>
</template>
<script>
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
import { quillEditor, Quill } from "vue-quill-editor";
import { container, ImageExtend, QuillWatch } from "quill-image-super-solution-module";

Quill.register("modules/ImageExtend", ImageExtend);
export default {
    components: { quillEditor },
    data() {
        return {
            example: "test",
            content: "",
            // 富文本框参数设置 
            // Rich text box parameter settings
            editorOption: {
                modules: {
                    ImageExtend: {
                      	// 可选参数 是否显示上传进度和提示语
                      	// Optional parameters. Whether to display upload progress and prompt
                        loading: true,
                      	// 图片参数名
                      	// Picture parameter name
                        name: "img",
                      	// 可选参数 图片大小，单位为M，1M = 1024kb 
                      	// Optional parameters. Image size, Unit is M
                        size: 1,
                      	// 服务器地址, 如果action为空，则采用base64插入图片 
                      	// Server address, if action is empty, use base64 to insert picture
                        action: "/uploads", 
                      	// 可选 可上传的图片格式 
                      	// Optional, uploadable image format
                        accept: "image/jpg, image/png, image/gif, image/jpeg, image/bmp, image/x-icon", 
                        // response 为一个函数用来获取服务器返回的具体图片地址 
                      	// response is a function to get the specific image address returned by the server
                        // 例如服务器返回 {code: 200; data:{ url: 'baidu.com'}}
                      	// For example, the server returns {code: 200; data:{ url: 'baidu.com'}}
                        // 则 return res.data.url
                        response: (res) => {
                            return res.data.url;
                        },
                      	// 可选参数 设置请求头部 
                      	// Optional parameter. Set request header
                        headers: (xhr) => {
                            // 比如添加 csrf-token
                          	// For example, add csrf-token
                            xhr.setRequestHeader("X-CSRFToken", "TestToken");
                        }, 
                      	// 图片超过大小的回调 
                      	// Callback when the image exceeds the size
                        sizeError: () => {
                            alert("图片大小超过 1 M");
                        }, 
                      	// 可选参数 自定义开始上传触发事件
                      	// Optional parameters. Custom start upload trigger event
                        start: () => {}, 
                      	// 可选参数 自定义上传结束触发的事件，无论成功或者失败
                      	// Optional parameters. Customize the event triggered by the end of upload, regardless of success or failure
                        end: () => {},
                      	// 可选参数 上传失败触发的事件
                      	// Optional parameter. The event triggered by upload failure
                        error: () => {}, 
                      	// 可选参数  上传成功触发的事件
                      	// Optional parameter. The event triggered by a successful upload
                        success: () => {}, 
                      	// 可选参数 选择图片触发，也可用来设置头部，但比headers多了一个参数，可设置formData
                      	// Optional parameters. Select the picture to trigger. It can also be used to set the header, but there is one more parameter than headers, which can be set formData
                        change: (xhr, formData) => {
                            formData.append("example", "test");
                        }, 
                    },
                    toolbar: {
                      	// container为工具栏，此次引入了全部工具栏，也可自行配置
                      	// container is a toolbar, all toolbars are introduced this time, and they can also be configured by themselves
                        container: container, 
                        handlers: {
                            image: function() {
                                // 劫持原来的图片点击按钮事件
                              	// Hijack the original picture click button event
                                QuillWatch.emit(this.quill.id);
                            },
                        },
                    },
                },
            },
        };
    },
};
</script>
```

## 致敬 Pay tribute to

此模块参考了众多插件，集大家之所成。取之于众，用之于众，希望对大家有所帮助。

This module refers to many plug-ins and integrates what everyone has made. Take it from the public and use it for the public, I hope it will be helpful to everyone.

#### 参考插件目录 Refer to the plugin catalog

-   quill-image-extend-module
-   quill-image-paste-module
-   quill-image-drop-module
-   还有很多 `Stack Overflow` 上好心人提供的代码 There is also a lot of code provided by good people on Stack Overflow

**如果好用，请点一下右上角的星星**

**Please click the star if it works**
