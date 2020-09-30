# quill-image-super-solution-module

## 简介

此模块为富文本编辑器 `vue-quill-editor` 的专用插件，为其提供了自定义上传图片到服务器、**粘贴**图片上传至服务器、**拖拽**图片上传至服务器的功能。支持与其他模块一起使用。

> 富文本编辑器原先实现方式为：将图片编码为 `base64` 的加密字符串，然后在提交数据时，此表单字段保存一个大的 `html` 内容，因此可能受网络原因，数据库限制等等外界因素导致失败。而且当我们需要这些图片资源时，不容易进行归档，因此才需要此模块。

## 亮点

1. 整合了粘贴、拖拽等功能，减少安装插件个数
2. 解决其他插件都存在的一个 `bug` ：插入图片时，光标位置错误
3. 解决其他插件粘贴图片时，显示不正常的 `bug`
4. 解决其他插件上传文件类型不加限制的 `bug`
5. 粘贴图片与拖拽图片也实现了上传至服务器的功能

## 功能

* Version 1.0.1
  1. 插入、粘贴、拖拽图片上传至服务器，自定义上传路径
  2. 限制图片大小功能
  3. 限制上传图片类型
  4. 显示上传进度
  5. 显示上传成功或失败
  6. 支持与其他插件共同使用
* Version 1.0.2
  1. 更改说明文档，修复说明文档中示例的错误

## 安装

```shell
# yarn （建议使用，高速下载）
yarn add quill-image-super-solution-module
# npm
npm install quill-image-super-solution-module --save-dev
```

## 引入

```shell
import { container, ImageExtend, QuillWatch } from "quill-image-super-solution-module";
Quill.register("modules/ImageExtend", ImageExtend);
```

## 使用示例

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
            editorOption: {
                modules: {
                    ImageExtend: {
                        loading: true, // 可选参数 是否显示上传进度和提示语
                        name: "img", // 图片参数名
                        size: 1, // 可选参数 图片大小，单位为M，1M = 1024kb
                        action: "/uploads", // 服务器地址, 如果action为空，则采用base64插入图片
                        accept: "image/jpg, image/png, image/gif, image/jpeg, image/bmp, image/x-icon", // 可选 可上传的图片格式
                        // response 为一个函数用来获取服务器返回的具体图片地址
                        // 例如服务器返回{code: 200; data:{ url: 'baidu.com'}}
                        // 则 return res.data.url
                        response: (res) => {
                            return res.data.url;
                        },
                        headers: (xhr) => {
                            // 比如添加 csrf-token
                            xhr.setRequestHeader("X-CSRFToken", "TestToken");
                        }, // 可选参数 设置请求头部
                        sizeError: () => {
                            alert("图片大小超过 1 M");
                        }, // 图片超过大小的回调
                        start: () => {}, // 可选参数 自定义开始上传触发事件
                        end: () => {}, // 可选参数 自定义上传结束触发的事件，无论成功或者失败
                        error: () => {}, // 可选参数 上传失败触发的事件
                        success: () => {}, // 可选参数  上传成功触发的事件
                        change: (xhr, formData) => {
                            formData.append("example", "test");
                        }, // 可选参数 选择图片触发，也可用来设置头部，但比headers多了一个参数，可设置formData
                    },
                    toolbar: {
                        container: container, // container为工具栏，此次引入了全部工具栏，也可自行配置
                        handlers: {
                            image: function() {
                                // 劫持原来的图片点击按钮事件
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

## 致敬

此模块参考了众多插件，集大家之所成。取之于众，用之于众，希望对大家有所帮助。

#### 参考插件目录

* quill-image-extend-module
* quill-image-paste-module
* quill-image-drop-module
* 还有很多 `Stack Overflow` 上好心人提供的代码