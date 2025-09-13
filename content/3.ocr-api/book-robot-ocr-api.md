---
title: 翻页机器人OCR api

description: 本接口提供给翻页机器人OCR调用.
---

|内容 |说明 |
| ----------- | ----------- |
|传输方式 |	http[s] (为提高安全性，推荐https) |
|测试地址 |	http[s]: //robotapi.yesnet.cc/api/ocr/upload |
|请求方法 |	POST 
|字符编码 |	UTF-8 |
| 响应格式 |	统一采用JSON格式 |
|开发语言 |	任意，只要可以向接口发起HTTP请求的均可 |
|图片格式 |	jpg/jpeg/png/bmp |
|图片大小 |	base64编码后大小不超过7M |


## 请求参数

在调用业务接口时，都需要在 Http Request Body 中配置以下参数，请求数据为FormData。


| 参数名称 | 必选 | 类型 | 描述 |
| ----------- | ----------- |----------- | ----------- |
| apiKey | 是 | string | 准许调用接口的key,请妥善保存。 |
| image | 是 | string | 图像数据，要求base64编码后大小不超过7M  |
| currentPage | 是 | int | 翻页机器人参数，当前页数。 |
| totalPages | 是 | int | 翻页机器人参数，总页数 |
| averageSpeed | 是 | int | 翻页机器人参数，平均翻页速度 |
| workedTime | 是 | int | 翻页机器人参数，已工作时长(分钟) |

## 返回结果

| 参数名称 |  类型 | 描述 |
| ----------- | ----------- |----------- | 
| code |  int | 0表示会话调用成功 |
| message |  string | 描述信息 |
| data |  object | 响应数据块 |

## 返回结果2

| 参数名称 |  类型 | 描述 |
| ----------- | ----------- |----------- | 
| code |  int | 0表示会话调用成功 |
| message |  string | 描述信息 |
| data |  object | 响应数据块 |
| data.groups |  array | 根据ocr返回的结果按照段落分组处理，一般展示用这个数据就行 |
| data.groups\[n].paragNo |  int | 段落号 |
| data.groups\[n].textDetections |  array | 检测到的文本信息，包括文本行内容 |
| data.groups\[n].textDetections\[n].detectedText |  string | 识别出的文本行内容 |
| data.ocrResult |  object | 腾讯ocr api 返回的原始结果，各字段[详见腾讯文档](https://cloud.tencent.com/document/product/866/34937#3.-.E8.BE.93.E5.87.BA.E5.8F.82.E6.95.B0) |




## 使用事例

```js{4}
//处理图片文件
const file = ref(null);
const handleImageUpload = (e) => {
  file.value = e.target.files[0];
};
//构建FormData
const formData = new FormData();
  formData.append("file", file.value);
  formData.append("apiKey","***********");
  formData.append("currentPage", "10");
  formData.append("totalPages", "100");
  formData.append("averageSpeed", "1");
  formData.append("workedTime", "60");
// 提交数据
const submitImage = async () => {
  try {
   const response = await fetch(`https://robotapi.yesnet.cc/api/ocr/upload`, {
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