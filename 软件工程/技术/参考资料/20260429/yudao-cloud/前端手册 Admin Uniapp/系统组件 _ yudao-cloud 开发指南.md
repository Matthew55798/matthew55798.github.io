* 开发指南
* 前端手册 Admin Uniapp

[芋道源码](https://www.iocoder.cn "作者")

[2026-01-02](javascript:;)

目录

[1. Wot Design Uni 组件库](#_1-wot-design-uni-组件库)

[1. DictTag 字典标签](#_1-dicttag-字典标签)

[2. UserPicker 用户选择器](#_2-userpicker-用户选择器)

[3. 文件上传](#_3-文件上传)

[3.1 uploadFile API](#_3-1-uploadfile-api)

[3.2 uploadFileFromPath 工具](#_3-2-uploadfilefrompath-工具)

[3.3 useUpload Hook](#_3-3-useupload-hook)

# 系统组件

本小节，介绍项目中封装的系统组件。

## [#](#_1-wot-design-uni-组件库) 1. Wot Design Uni 组件库

项目使用 [Wot Design Uni  (opens new window)](https://wot-ui.cn/) 作为基础 UI 组件库，提供了丰富的移动端组件。

大多数情况下，你可以直接使用 Wot Design Uni 提供的组件来构建界面。

## [#](#_1-dicttag-字典标签) 1. DictTag 字典标签

字典标签组件，用于展示字典数据的标签，支持颜色高亮。

* 源码位置：[src/components/dict-tag/dict-tag.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/components/dict-tag/dict-tag.vue)
* 详细文档：[字典数据](https://cloud.iocoder.cn/admin-uniapp/dict/)

## [#](#_2-userpicker-用户选择器) 2. UserPicker 用户选择器

用户选择器组件，支持单选和多选模式，内置搜索过滤功能。

* 源码位置：[src/components/system-select/user-picker.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/components/system-select/user-picker.vue)
* 实战案例（单选）：[src/pages-system/dept/form/index.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/pages-system/dept/form/index.vue) 的 `formData.leaderUserId` 部分
* 实战案例（多选）：[src/pages-bpm/user-group/form/index.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/pages-bpm/user-group/form/index.vue) 的 `formData.userIds` 部分
* 实战案例（获取昵称）：[src/pages-system/operate-log/modules/search-form.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/pages-system/operate-log/modules/search-form.vue) 的 `getUserNickname` 部分

## [#](#_3-文件上传) 3. 文件上传

项目提供了两种文件上传方式：

### [#](#_3-1-uploadfile-api) 3.1 uploadFile API

直接调用上传 API，适用于简单场景。

* 源码位置：[src/api/infra/file/index.ts  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/api/infra/file/index.ts)
* 实战案例：[src/pages-infra/file/components/file-list.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/pages-infra/file/components/file-list.vue) 的 `handleUpload` 部分

### [#](#_3-2-uploadfilefrompath-工具) 3.2 uploadFileFromPath 工具

支持前端直连上传（S3）和后端上传两种模式，通过环境变量 `VITE_UPLOAD_TYPE` 配置。

* 源码位置：[src/utils/uploadFile.ts  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/utils/uploadFile.ts)
* 实战案例：[src/pages-core/user/profile/index.vue  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/pages-core/user/profile/index.vue) 的 `uploadFileFromPath` 部分

```
import { uploadFileFromPath } from '@/utils/uploadFile'

// 上传文件到指定目录
const url = await uploadFileFromPath(filePath, 'avatar')
```

### [#](#_3-3-useupload-hook) 3.3 useUpload Hook

[src/hooks/useUpload.ts  (opens new window)](https://github.com/yudaocode/yudao-ui-admin-uniapp/blob/master/src/hooks/useUpload.ts) 封装了文件选择、校验、上传的完整流程。

```
import useUpload from '@/hooks/useUpload'

const { loading, data, run } = useUpload({
  fileType: 'image',
  maxSize: 5 * 1024 * 1024,
  success: (url) => console.log('上传成功:', url),
})

// 触发选择和上传
run()
```

[字典数据](https://cloud.iocoder.cn/admin-uniapp/dict/) [通用方法](https://cloud.iocoder.cn/admin-uniapp/util/)

←
[字典数据](https://cloud.iocoder.cn/admin-uniapp/dict/) [通用方法](https://cloud.iocoder.cn/admin-uniapp/util/)→