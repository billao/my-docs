---
title: 档案系统OCR api
description: 本接口提供给档案系统OCR调用.
---


|内容 |说明 |
| ----------- | ----------- |
|传输方式 |	http[s] (为提高安全性，推荐https) |
|测试地址 |	http[s]: //ocrapi.gzyxnet.cn/api/external/ocr |
|请求方法 |	POST 
|字符编码 |	UTF-8 |
| 响应格式 |	统一采用JSON格式 |
|开发语言 |	任意，只要可以向接口发起HTTP请求的均可 |
|图片格式 |	jpg/jpeg/png/bmp |
|单张图片大小 |	base64编码后大小不超过7M |


## 请求参数

在调用业务接口时，都需要在 Http Request Body 中配置以下参数，请求数据为FormData。


| 参数名称 | 必选 | 类型 | 描述 |
| ----------- | ----------- |----------- | ----------- |
| apiKey | 是 | string | 准许调用接口的key,请妥善保存（待定，是否需要） |
| caseNumber | 是 | string | 案卷号 |
| caseTitle | 是 | string | 案卷名称 |
| ocrType | 是 | int | ocr类型，1为表格类ocr，2为凭证类ocr  |
|  images | 是 | 二进制数据 | 批量图像数据,要求单个图像编码后大小不超过7M |



## 返回结果

| 参数名称 |  类型 | 描述 |
| ----------- | ----------- |----------- | 
| code |  int | 0表示会话调用成功 |
| message |  string | 描述信息 |
| data |  object | 响应数据块 |



## 使用事例

```js{4}

//处理批量图片文件
const selectedFiles = ref([]);
const handleFileUpload = (event) => {
      selectedFiles.value = Array.from(event.target.files);
};

//构建FormData
const formData = new FormData();
  formData.append("apiKey","***********");
  formData.append("caseNumber", "G9.3.3");
  formData.append("caseTitle", "案卷名称一");
  formData.append("ocrType", "1");
  // 添加所有图片文件
  selectedFiles.value.forEach((file, index) => {
    formData.append("images", file); 
   });

// 提交数据
const submitData = async () => {
  try {
   const response = await fetch(`https://ocrapi.gzyxnet.cn/api/external/ocr`, {
      method: "POST",
      body: formData,
    });
    const result = await response.json();
    console.log('上传成功', result);
  } catch (error) {
    console.error('上传失败', error);
  }
};
```