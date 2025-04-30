---
title: SpringBoot整合AI
date: 2025-4-30 11:09:06
description: Spring boot 整合常见的各类大模型（国内）
tags:
  - 教程
  - 大模型
categories:
cover: https://img.znxs.vip/study/202504301108852.png
top_img: https://img.znxs.vip/study/202504301108692.jpg
---

### 整合AI的几种调用方式

- #### HTTP 接入

  采用调用http请求的方式来请求大模型，阿里官方http调用方式文档：[通义千问api](https://help.aliyun.com/zh/model-studio/use-qwen-by-calling-api#9141263b961cc)

  ```java
  import com.openai.client.OpenAIClient;
  import com.openai.client.okhttp.OpenAIOkHttpClient;
  import com.openai.models.ChatCompletion;
  import com.openai.models.ChatCompletionCreateParams;
  
  /**
   * 通过http 调用
   */
  public class AIHttpInvoke {
      public static void main(String[] args) {
          OpenAIClient client = OpenAIOkHttpClient.builder()
                  .apiKey("DASHSCOPE_API_KEY")
                  .baseUrl("https://dashscope.aliyuncs.com/compatible-mode/v1")
                  .build();
          ChatCompletionCreateParams params = ChatCompletionCreateParams.builder()
                  .addUserMessage("你是谁")
                  .model("qwen-plus")
                  .build();
          ChatCompletion chatCompletion = client.chat().completions().create(params);
          System.out.println(chatCompletion.choices().get(0).message().content().orElse("无返回内容"));
      }
  }
  ```

- #### SDK 直接调用

  采用SDK调用，导入大模型的依赖来进行调用

  ```xml
  <!--通义灵积大模型SDK 调用-->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>dashscope-sdk-java</artifactId>
      <version>2.19.2</version>
  </dependency>
  ```

  在阿里官网配置文件可以查看调用文档：

  ```java
  import java.util.Arrays;
  import java.lang.System;
  
  import com.alibaba.dashscope.aigc.generation.Generation;
  import com.alibaba.dashscope.aigc.generation.GenerationParam;
  import com.alibaba.dashscope.aigc.generation.GenerationResult;
  import com.alibaba.dashscope.common.Message;
  import com.alibaba.dashscope.common.Role;
  import com.alibaba.dashscope.exception.ApiException;
  import com.alibaba.dashscope.exception.InputRequiredException;
  import com.alibaba.dashscope.exception.NoApiKeyException;
  import com.alibaba.dashscope.utils.JsonUtils;
  import org.znxs.znagent_s.constant.TestApiKey;
  
  /**
   * 通义千问调用 SDK调用方式
   */
  public class AliBaiLianModel {
      public static GenerationResult callWithMessage() throws ApiException, NoApiKeyException, InputRequiredException {
          Generation gen = new Generation();
          Message systemMsg = Message.builder()
                  .role(Role.SYSTEM.getValue())
                  .content("You are a helpful assistant.")
                  .build();
          Message userMsg = Message.builder()
                  .role(Role.USER.getValue())
                  .content("who are you？中文回答")
                  .build();
          GenerationParam param = GenerationParam.builder()
                  // 若没有配置环境变量，请用百炼API Key将下行替换为：.apiKey("sk-xxx")
                  .apiKey(TestApiKey.API_KEY)
                  // 此处以qwen-plus为例，可按需更换模型名称。模型列表：https://help.aliyun.com/zh/model-studio/getting-started/models
                  .model("qwen-max-2025-01-25")
                  .messages(Arrays.asList(systemMsg, userMsg))
                  .resultFormat(GenerationParam.ResultFormat.MESSAGE)
                  .build();
          return gen.call(param);
      }
  
      public static void main(String[] args) {
          try {
              GenerationResult result = callWithMessage();
              System.out.println("你好");
              System.out.println(JsonUtils.toJson(result) + "你好");
          } catch (ApiException | NoApiKeyException | InputRequiredException e) {
              // 使用日志框架记录异常信息
              System.err.println("An error occurred while calling the generation service: " + e.getMessage());
          }
          System.exit(0);
      }
  }
  ```

- #### LangChain4j

  要接入阿里云灵积模型，可以参考官方文档：[DashScope模型集成](https://docs.langchain4j.dev/integrations/language-models/dashscope)，提供了依赖和示例代码。

  ```xml
  <!--LangChain4j 整合 dashscope -->
  <dependency>
      <groupId>dev.langchain4j</groupId>
      <artifactId>langchain4j-community-dashscope</artifactId>
      <version>1.0.0-beta2</version>
  </dependency>
  ```

  java调用

  ```java
  import dev.langchain4j.community.model.dashscope.QwenChatModel;
  import dev.langchain4j.model.chat.ChatLanguageModel;
  import org.znxs.znagent_s.constant.TestApiKey;
  
  /**
   * 采用 LangChain4j 整合dashscope调用
   */
  public class LangChainAiInvoke {
  
      public static void main(String[] args) {
          ChatLanguageModel qwenModel = QwenChatModel.builder()
                  .apiKey(TestApiKey.API_KEY)
                  .modelName("qwen-max-2025-01-25")
                  .build();
          String answer = qwenModel.chat("我是左拿先生，一个程序员");
          System.out.println(answer);
      }
  }
  ```

- #### SpringAI

  这里比较推荐的是spring ai调用，因为整合度较高，使用比较方便 

  这里采用的是spring ai alibaba 的apring ai 参考文档：[Spring AI Alibaba](https://java2ai.com/docs/1.0.0-M6.1/get-started/?spm=4347728f.5a464c8a.0.0.1da93db43fcqRi)

  ```xml
  <!--spring ai alibaba-->
  <dependency>
      <groupId>com.alibaba.cloud.ai</groupId>
      <artifactId>spring-ai-alibaba-starter</artifactId>
      <version>1.0.0-M6.1</version>
  </dependency>
  <!--spring ai alibaba 接入本地ollama-->
  <dependency>
      <groupId>org.springframework.ai</groupId>
      <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
      <version>1.0.0-M6</version>
  </dependency>
  ```

  通过依赖注入的方式 进行调用

  ```java
  @Resource
  private ChatModel dashscopeChatModel;
  @Resource
  private ChatModel ollamaChatModel;
  
  @GetMapping("/dashscope")
  public String dashscopeTest() {
      AssistantMessage output = dashscopeChatModel.call(new Prompt("你好，我是左拿,你是谁？"))
              .getResult()
              .getOutput();
      return output.getText();
  }
  @GetMapping("/ollama")
  public String ollamaTest() {
      AssistantMessage output = ollamaChatModel.call(new Prompt("你好，我是左拿,你是谁？"))
              .getResult()
              .getOutput();
      return output.getText();
  }
  ```

  配置文件内配置模型密钥

  ```yaml
  spring:  
    ai:
      # 阿里灵犀调用
      dashscope:
        api-key: sk-***
        chat:
          options:
            model: qwen-plus-2025-04-28
      # 本地ollama调用
  #    ollama:
  #      base-url: http://localhost:11434
  #      chat:
  #        model: deepseek-r1:14b
  ```

  本来是有springai配置文件AIConfig 的 但是阿里好像没有兼容本地的ollama的，所以就没有使用这个配置文件了

  【**注意，下方配置文件有问题**】

  ```java
  import com.alibaba.cloud.ai.dashscope.api.DashScopeApi;
  import com.alibaba.cloud.ai.dashscope.chat.DashScopeChatModel;
  import org.springframework.ai.chat.model.ChatModel;
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  
  @Deprecated /// 已过时
  //@Configuration
  public class AiConfig {
  
      @Bean(name = "dashscopeModel")
      public ChatModel dashscopeModel(@Value("${spring.ai.dashscope.api-key}") String apiKey,
                                      @Value("${spring.ai.dashscope.chat.options.model}") String model) {
          // 根据提供的参数创建并返回 Dashscope 的 ChatModel 实例
          return new DashScopeChatModel(new DashScopeApi(apiKey, model));
      }
  
      @Bean(name = "ollamaModel")
      public ChatModel ollamaModel(@Value("${spring.ai.ollama.base-url}") String baseUrl,
                                   @Value("${spring.ai.ollama.chat.model}") String model) {
          // 根据提供的参数创建并返回 Ollama 的 ChatModel 实例
          // todo 这里没有兼容ollama
          return null;
      }
  }
  ```



【注意】这里好像有个坑，就是使用spring ai 依赖使用dashscope模型和ollama模型的时候，会导致依赖注入两个bean而导致冲突

```
Description:

Parameter 1 of method chatClientBuilder in org.springframework.ai.autoconfigure.chat.client.ChatClientAutoConfiguration required a single bean, but 2 were found:
	- dashscopeChatModel: defined by method 'dashscopeChatModel' in class path resource [com/alibaba/cloud/ai/autoconfigure/dashscope/DashScopeAutoConfiguration$DashScopeChatConfiguration.class]
	- ollamaChatModel: defined by method 'ollamaChatModel' in class path resource [org/springframework/ai/autoconfigure/ollama/OllamaAutoConfiguration.class]

This may be due to missing parameter name information
```

> 追加，发现了原因，是因为需要通过构造器注入的时候，不知道注入哪个chatModal，导致ChatClientAutoConfiguration，注入bean的时候，发现了两个，就有了 **ChatClientAutoConfiguration required a single bean, but 2 were found**: 这个错误，这里使用ChatClient的时候，还是只选择一种就行了 即去掉另一个的依赖，下面这两个依赖二选一  （阿里支持自己模型直接调用，真是开小灶啊，别家的模型就需要另外在添加依赖）
>
> ```xml
>         <!--spring ai alibaba-->
>         <dependency>
>             <groupId>com.alibaba.cloud.ai</groupId>
>             <artifactId>spring-ai-alibaba-starter</artifactId>
>             <version>1.0.0-M6.1</version>
>         </dependency>
>         <!--spring ai 接入本地ollama-->
> <!--        <dependency>-->
> <!--            <groupId>org.springframework.ai</groupId>-->
> <!--            <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>-->
> <!--            <version>1.0.0-M6</version>-->
> <!--        </dependency>-->
> ```



### 通过SpringAI 调用模型进行对话



