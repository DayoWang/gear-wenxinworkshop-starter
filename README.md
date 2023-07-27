<div align="right">
<a href="/README.md">中文</a> &nbsp;|&nbsp;
<a href="/README-EN.md">EN</a>
</div>

<div align="center">

![gear-wenxinworkshop-starter](https://socialify.git.ci/gemingjia/gear-wenxinworkshop-starter/image?font=Inter&forks=1&issues=1&language=1&name=1&owner=1&pattern=Floating%20Cogs&pulls=1&stargazers=1&theme=Light)

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1.0-brightgreen.svg)
![JDK](https://img.shields.io/badge/JDK-17.0.5-orange.svg)
![Maven](https://img.shields.io/badge/Maven-3.9-blue.svg)

![LICENSE](https://img.shields.io/github/license/gemingjia/gear-wenxinworkshop-starter?style=flat-square)
![COMMIT](https://img.shields.io/github/last-commit/gemingjia/gear-wenxinworkshop-starter?style=flat-square)
![LANG](https://img.shields.io/badge/language-Java-7F52FF?style=flat-square)

</div>
# Gear-WenXinWorkShop-Starter

## 获取access-token方法
![image](https://github.com/gemingjia/gear-wenxinworkshop-starter/assets/80268501/7225fb98-761a-4ead-b626-d59fc0931161)
可直接查看下面文章：

[申请文心一言&文心千帆大模型API资格、获取access_token，并使用SpringBoot接入文心一言API](https://blog.csdn.net/qq_30299877/article/details/131917097)

---

1. 前往 [文心一言资格申请](https://cloud.baidu.com/product/wenxinworkshop) 

2. [填写问卷](https://cloud.baidu.com/survey/qianfan.html)，等待审核通过（我用了一天半）

3. 审核通过后进入 [控制台](https://console.bce.baidu.com/ai/?_=#/ai/wenxinworkshop/overview/index)，点击[创建应用](https://console.bce.baidu.com/ai/?_=#/ai/wenxinworkshop/app/create)
4. 进入左侧 [应用列表](https://console.bce.baidu.com/ai/?_=#/ai/wenxinworkshop/app/list)，复制 `API Key` 与 `Secret Key`
5. 将你的 `API Key` 与 `Secret Key` 替换链接的[Key]，访问以下地址
 > https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=[API-Key]&client_secret=[Secret-Key]

## 📖 项目简介
- 百度 **“文心千帆 WENXINWORKSHOP”** 大模型的spring-boot-starter，可以帮助您快速接入百度的AI能力，只需一行代码即可调用百度文心千帆大模型。
- 完整对接文心千帆的官方API文档。
- 支持对话的流式返回。
- 完整支持 `文心一言 ErnieBot`、`ERNIE-Bot-turbo`、`BLOOMZ-7B`、`Prompt模板`模型的API（单轮对话、连续对话、流式返回）。
- 0.0.4版本中将增加Embedding-V1模型的支持。


## 🚀 快速开始
### 1、添加依赖
Repository Path: [/io/github/gemingjia/gear-wenxinworkshop-starter/0.0.2/gear-wenxinworkshop-starter-0.0.2.jar](/io/github/gemingjia/gear-wenxinworkshop-starter/0.0.2/gear-wenxinworkshop-starter-0.0.2.jar)
- Maven
```xml
<!-- 若中央仓库未更新可等待1-2小时 -->
<dependency>
  <groupId>io.github.gemingjia</groupId>
  <artifactId>gear-wenxinworkshop-starter</artifactId>
  <version>0.0.3</version>
</dependency>
```
- Gradle
```gradle
dependencies {
  implementation 'io.github.gemingjia:gear-wenxinworkshop-starter:0.0.3' 
}
```

### 2、添加access-token
- application.yml & application.yaml
  ```yaml
  gear:
    wenxin:
      access-token: xx.xxxxxxxxxx.xxxxxx.xxxxxxx.xxxxx-xxxx
  ```
- application.properties
  ```properties
  gear.wenxin.access-token=xx.xxxxxxxxxx.xxxxxx.xxxxxxx.xxxxx-xxxx
  ```

### 3、调用示例
```java

@RestController
public class ChatController {

    // 要调用的模型的客户端
    @Resource
    private ErnieBotClient ernieBotClient;

    // 单次对话
    @PostMapping("/chat")
    public BaseResponse<String> chatSingle(String msg) {
        ChatResponse response = ernieBotClient.chatSingle(msg);
        return ResultUtils.success(response.getResult());
    }

    // 连续对话
    @PostMapping("/chats")
    public BaseResponse<String> chatCont(String msg) {
        String chatUID = "test-user-1001";
        ChatResponse response = ernieBotClient.chatCont(msg, chatUID);
        return ResultUtils.success(response.getResult());
    }

    // 流式返回,单次对话
    @PostMapping(value = "/stream/chat", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<ChatResponse> chatSingleStream(String msg) {
        return ernieBotClient.chatSingleOfStream(msg);
    }

    // 流式返回,连续对话
    @PostMapping(value = "/stream/chats", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<ChatResponse> chatContStream(String msg, String msgUid) {
        return ernieBotClient.chatContOfStream(msg, msgUid);
    }

    // 模板对话
    @PostMapping("/rompt")
    public BaseResponse<PromptResponse> chatSingle() {
        Map<String, String> map = new HashMap<>();
        map.put("article", "我看见过波澜壮阔的大海，玩赏过水平如镜的西湖，却从没看见过漓江这样的水。漓江的水真静啊，静得让你感觉不到它在流动。");
        map.put("number", "20");
        PromptRequest promptRequest = new PromptRequest();
        promptRequest.setId(1234);
        promptRequest.setParamMap(map);
        PromptResponse promptResponse = promptBotClient.chatPrompt(promptRequest);

        return ResultUtils.success(promptResponse);
    }

}
```

## 📑使用文档

<div>
点击跳转 => 
<a href="/wenxin-doc.md">使用文档</a>
</div>

## 开源协议
[LICENSE](https://www.apache.org/licenses/LICENSE-2.0)
