---
title: SpringBoot整合AI
date: 2025-4-30 11:09:06
description: Spring boot 整合常见的各类大模型（国内）
tags:
  - 教程
  - 大模型
categories:
cover: https://img.znxs.vip/study/202504301108692.jpg
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
>      <!--spring ai alibaba-->
>      <dependency>
>          <groupId>com.alibaba.cloud.ai</groupId>
>          <artifactId>spring-ai-alibaba-starter</artifactId>
>          <version>1.0.0-M6.1</version>
>      </dependency>
>      <!--spring ai 接入本地ollama-->
> <!--        <dependency>-->
> <!--            <groupId>org.springframework.ai</groupId>-->
> <!--            <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>-->
> <!--            <version>1.0.0-M6</version>-->
> <!--        </dependency>-->
> ```



### 通过SpringAI 调用模型进行对话

```java
@RestController
@RequestMapping("/ai")
@Slf4j
public class AIController {

    @Resource
    private ChatModel dashscopeChatModel;

//    @Resource
//    private ChatModel ollamaChatModel;

    @Resource
    private ChatService chatService;


    // 方式一 使用构造器注入
    private final ChatClient chatClient1;
    public AIController(ChatClient.Builder builder) {
        this.chatClient1 = builder
                // 设定默认的预设
                .defaultSystem("你是恋爱顾问江小白，我是{user}")
                // 自定义 顾问 实现一些额外功能
                .defaultAdvisors(
//                        new MessageChatMemoryAdvisor(chatMemory), // 对话记忆 advisor
//                        new QuestionAnswerAdvisor(vectorStore) // RAG 检索增强 advisor
                )
                .build();
    }

    // 方式二 采用建造者模式调用
    ChatClient chatClient2 = ChatClient.builder(dashscopeChatModel)
            .defaultSystem("你是恋爱顾问")
            .build();


    @GetMapping("/chat/dashscope")
    public String dashscopeChatModelTest() {
        AssistantMessage output = dashscopeChatModel.call(new Prompt("你好，我是左拿,你是谁？"))
                .getResult()
                .getOutput();
        return output.getText();
    }

//    @GetMapping("/chat/ollama")
//    public String ollamaChatModelTest() {
//        AssistantMessage output = ollamaChatModel.call(new Prompt("你好，我是左拿,你是谁？"))
//                .getResult()
//                .getOutput();
//        return output.getText();
//    }

    @GetMapping("/client/dashscope")
    public String dashscopeClientTest(@RequestParam String text) {
        // 使用建造者模式 构架实体类
//        ChatClient chatClient = ChatClient.builder(dashscopeChatModel)
//                .defaultSystem("你是恋爱顾问")
//                .build();
        ChatClient chatClient3 = chatService.getChatClient();
        // 返回字符串
        String content = chatClient3.prompt().user("你好啊，" + text).call().content();
        // 返回聊天对象
//        ChatResponse chatResponse = chatClient3.prompt().user("Tell me a joke").call().chatResponse();
        // 返回指定对象形式
        record ActorFilms(String actor, List<String> movies) {
        }
//        ActorFilms actorFilms = chatClient3.prompt().user("Generate the filmography for a random actor.").call().entity(ActorFilms.class);
        //  返回集合
//        List<ActorFilms> actorFilmsList = chatClient3.prompt().user("为熊出没制作两部电影").call().entity(new ParameterizedTypeReference<List<ActorFilms>>() {
//        });
        // 对话时动态更改系统提示词的变量
//        String content3 = chatClient.prompt()
//                .system(sp -> sp.param("user", "左拿"))
//                .user("你好，我是谁？")
//                .call()
//                .content();

        // 对话时动态设定拦截器参数，比如指定对话记忆的 id 和长度
        String response = this.chatClient1.prompt()
                .advisors(advisor -> advisor.param("chat_memory_conversation_id", "678")
                        .param("chat_memory_response_size", 100))
                .user("你好，我是谁？")
                .call()
                .content();
        return response;
    }
}
```





### RAG 整合

RAG 流程

![img](https://img.znxs.vip/study/20250615153433142_oIbFEJ2kIr2ws57u.webp)

- ### 准备数据

  首先需要准备数据，也就是知识库的数据源，这里称为原始文档，一般RAG 的 读取是从文档类文件中读取的，例如：txt、word、pdf、excel、markdown等文件。还能从网页，数据困等结构化数据中获取

  - #### 文档预处理

  对文档进行数据清晰，剔除不需要的部分，剔除影响AI 所需回答的内容例如长代码、无用的措辞、无用的案例等

- ### 文档收集和拆分 ETL（Extract Transfrom Load）

  ![img](https://img.znxs.vip/study/20250615153437645_mY1SjfXTshOxqMLt.webp)

  对文档进行块状拆分，将文档按**合适的方式**切分成片段（俗称chunks：组块）

  拆分方式有

  - 按固定大小拆分：例如512字节
  - 按语义拆分：例如根据内容、段落、章节
  - 基于递归分割策略拆分：（如递归字符 n-gram 切割）

  这样做的好处有

  - 更加方便AI进行匹配，例如根据标签匹配知识库内容
  - 内容更加精确，简介，没有多余内容

  ==>文档处理完成之后就需要对文档进行转换向量操作

- ### Embedding 模型向量转换和存储

  ![img](https://img.znxs.vip/study/20250615153441314_eYpl07AKdNneRjuL.webp)

  使用Embedding模型 对文档切片内容进行向量内容转换，将文本块内容转换成高纬度向量表示，方便捕捉文本的语义内容

  - #### 向量存储

  把转换的向量进行存储起来

- ### 文档过滤与检索

  ![img](https://img.znxs.vip/study/20250615153443605_bagqTEknnhZSss0F.webp)

  将用户的问题转换成向量表示，基于元数据，关键字，标签等规则进行过滤

  然后进行相似度匹配，通过相似度算法（余弦相似度），在向量数据库中查找最相似的几项文档切片

  再对文档进行上下问组装，将多个连续的文档块组装成连贯的上下文

- ### 查询增强和关联

  ![img](https://img.znxs.vip/study/20250615153445425_tsQzX65NIpj7bMQb.webp)

  将检索的内容与用户问题融合，合成增强提示

  再对生成内容进行封装，格式化回答内容

- ### 整合

  文档准备，首先需要准备数据，可以选择本地数据，也可以选择使用远程数据，网络上的数据

  这里采用本地的markdown文档作为数据

  准备三个markdown文件

- ### 文档处理

  对文档进行进一步处理，把文档转换成Document的格式，方便接口进行调用

  这里需要编写一个文档加载器，采用spring 官方的markdown文档处理依赖

  ```xml
  <!--spring ai md文档处理-->
  <dependency>
      <groupId>org.springframework.ai</groupId>
      <artifactId>spring-ai-markdown-document-reader</artifactId>
      <version>1.0.0-M6</version>
  </dependency>
  ```

  编写文档加载器

  ```java
  @Component
  @Slf4j
  /**
   * 文档加载器
   */
  public class LoveAppDocumentLoader {
  
      private final ResourcePatternResolver resourcePatternResolver;
  
      public LoveAppDocumentLoader(ResourcePatternResolver resourcePatternResolver) {
          this.resourcePatternResolver = resourcePatternResolver;
      }
  
      /**
       * 加载markdown
       * @return
       */
      public List<Document> loadMarkdowns() {
          ArrayList<Document> allDocuments = new ArrayList<>();
  
          try {
              // 读取本地资源文件
              Resource[] resources = resourcePatternResolver.getResources("classpath:document/*.md");
              for (Resource resource : resources) {
                  String filename = resource.getFilename();
                  // 使用spring ai 提供的md 文档读取进行构建配置
                  MarkdownDocumentReaderConfig config = MarkdownDocumentReaderConfig.builder()
                          .withHorizontalRuleCreateDocument(true)
                          .withIncludeCodeBlock(false)
                          .withIncludeBlockquote(false)
                          .withAdditionalMetadata("filename", filename)
                          .build();
                  // 进行读取
                  MarkdownDocumentReader markdownDocumentReader = new MarkdownDocumentReader(resource, config);
                  allDocuments.addAll(markdownDocumentReader.get());
              }
          } catch (IOException e) {
              log.error("文档加载失败：", e);
          }
          return allDocuments;
      }
  }
  ```

- ### 转换向量到向量存储中

  获取文档之后，编写向量存储配置类，创建自定义向量存储Bean 通过EmbeddingModel 嵌入模型转换文档数据变成向量数据到向量存储中 这里的EmbeddingModel 模型采用的是spring alibaba 整合的嵌入模型，返回一个VectorStore对象

  ```java
  /**
   * 向量存储配置（初始化基于内存的向量存储 Bean） 用于加载文档变成向量数据到内存中
   */
  @Configuration
  public class LoveAppVectorStoreConfig {
  
      @Resource
      private LoveAppDocumentLoader loveAppDocumentLoader;
  
      @Bean
      VectorStore loveAppVectorStore(EmbeddingModel dashscopeEmbeddingModel) {
          SimpleVectorStore simpleVectorStore = SimpleVectorStore.builder(dashscopeEmbeddingModel)
                  .build();
          // 加载文档
          List<Document> documents = loveAppDocumentLoader.loadMarkdowns();
          simpleVectorStore.add(documents);
          return simpleVectorStore;
      }
  }
  ```

- ### 检索和查询

  通过在client 添加QuestionAnswerAdvisor 顾问，传入自定义VectorStore， 使用Spring ai 自动帮我们实现检索和查询增强功能

  ```java
   ChatResponse chatResponse = chatClient
                  .prompt()
                  .user(message)
                  .advisors(spec -> spec.param(CHAT_MEMORY_CONVERSATION_ID_KEY, chatId)
                          .param(CHAT_MEMORY_RETRIEVE_SIZE_KEY, 10)
                  )
                  // 自定义 RAG 自定义实现 loveAppVectorStore
                  .advisors(new QuestionAnswerAdvisor(loveAppVectorStore))
                  .call()
                  .chatResponse();
  String content = chatResponse.getResult().getOutput().getText();
  ```

- 还能通过连接远程向量数据库的方式，实现RAG

以上就是RAG的基本用法



#### RAG 实践和调优

- ### **QueryRewriter**

  查询重写器，实现通过ai对提示词的重写，更加准确的进行查询，去除无用提示信息

  > 但是不推荐作为核心功能使用，因为ai重写的内容可能会丢失关键信息，从而会导致无法匹配**元信息**的问题

  ```java
  /**
   * 查询重写器
   */
  @Component
  public class QueryRewriter {
  
      private final QueryTransformer queryTransformer;
  
      public QueryRewriter(ChatModel dashscopeChatModel) {
          ChatClient.Builder builder = ChatClient.builder(dashscopeChatModel);
          // 创建查询重写转换器
          queryTransformer = RewriteQueryTransformer.builder()
                  .chatClientBuilder(builder)
                  .build();
      }
  
      /**
       * 对用户的提示词进行重写
       * @param prompt 用户提示词
       * @return
       */
      public String doQueryRewrite(String prompt) {
          Query query = new Query(prompt);
          // 执行查询重写
          Query transformedQuery = queryTransformer.transform(query);
          // 输出重写后的查询
          return transformedQuery.text();
      }
  }
  ```

- ###  **空上下文处理器** 

  通过官方的ContextualQueryAugmenter 通过自定义的空处理提示词模板，替换系统的处理空提示的时候上下文模板

  【注意】如果发现总是触发空上下文，查询是否是查询重写器导致元信息没有匹配导致找不到ARG的问题，还是需要进行DEBUG的方式来进行逐步查询

  ```java
  /**
   * 自定义 RAG 上下文查询增强 工厂
   */
  public class LoveAppContextualQueryAugmenterFactory {
  
      public static ContextualQueryAugmenter createInstance() {
          // 自定义空提示词模板
          PromptTemplate emptyContextPromptTemplate = new PromptTemplate("""
                  你应该输出下面的内容：
                  抱歉，我只能回答恋爱相关的问题，别的没办法帮到您哦，
                  有问题可以联系做左拿客服 https://znxs.vip
                  """);
  
          /*
           有三种策略
           1、若不使用这个自定义的上下文处理器，采用系统默认，系统会使用默认的空提示模板
           1、若不允许空提示 就是下面的false 则不定义提示词模板，就会使用系统默认的提示词模板
           （system）The user query is outside your knowledge base.Politely inform the user that you can't answer it.
           2、若允许空提示，则会报错
           3、若不允许空提示，并且使用自定义的提示词模板，则会应用自定义提示词模板
           */
          return ContextualQueryAugmenter.builder()
                  .allowEmptyContext(false)
                  .emptyContextPromptTemplate(emptyContextPromptTemplate)
                  .build();
      }
  }
  ```

- ### 自定义mate信息

  修改文档加载器保存Document类的元信息，例如通过保存status 为 文件后缀的单身、恋爱、已婚的分类作为元信息

  ```java
  /**
   * RAG 检索增强 自定义属性 Advisor 工厂
   * <p>
   * 可选功能，自定义文档检索器、自定义查询增强器（例如空上下文处理器）
   */
  @Slf4j
  public class LoveAppRagCustomAdvisorFactory {
  
      /**
       * 创建自定义 Advisor
       *
       * @param vectorStore 向量存储
       * @param status      状态
       * @return Advisor 顾问
       */
      public static Advisor createRagCustomAdvisor(VectorStore vectorStore, String status) {
          Filter.Expression expression = new FilterExpressionBuilder()
                  .eq("status", status)
                  .build();
  
          DocumentRetriever documentRetriever = VectorStoreDocumentRetriever.builder()
                  .vectorStore(vectorStore)
                  .filterExpression(expression) // 只有满足这个过滤表达式的才能检索通过
                  .similarityThreshold(0.5) // 相似度过滤 只有达到这个相似度才能检索通过
                  .topK(3) // 检索前几个
                  .build();
  
          // 通过官方自带的检索增强顾问来 构建实例
          return RetrievalAugmentationAdvisor.builder()
                  // 使用自定义文档检索处理
                  .documentRetriever(documentRetriever)
                  // 使用查询增强处理 空上下文处理
                  .queryAugmenter(LoveAppContextualQueryAugmenterFactory.createInstance())
                  .build();
      }
  }
  ```

  ```java
  ChatResponse chatResponse = chatClient
          .prompt()
          .user(message)
          // 使用记忆advisor
          .advisors(spec -> spec.param(CHAT_MEMORY_CONVERSATION_ID_KEY, chatId)
                  .param(CHAT_MEMORY_RETRIEVE_SIZE_KEY, 10)
          )
          // 采用 RAG 检索增强服务 (基于自定义的检索增强顾问 可以通过预设向量存储 和 元信息来配置检索过滤)
  		 .advisors(LoveAppRagCustomAdvisorFactory
                     .createRagCustomAdvisor(loveAppVectorStore, "恋爱"))
          .call()
          .chatResponse();
  ```

  **测试** 当输入恋爱才能命中到文档，输入单身因为元信息过滤所以直接无法命中

  ```java
  @Test
  void doChatWithRag() {
      String chatId = UUID.randomUUID().toString();
      String message = "我在恋爱中想玩些游戏，推荐一些游戏";
        String message = "我现在单身，有些焦虑怎么办？";
      String answer = loveApp.doChatWithRag(message, chatId);
      Assertions.assertNotNull(answer);
  }
  ```

  

- ###  自定义文档增强顾问工厂 

  通过官方的VectorStoreDocumentRetriever构造**自定义检索过滤器的文档检索实例**

  再通过官方的RetrievalAugmentationAdvisor构造出**自定义兜底策略Advisor实例**

  ```java
  /**
   * RAG 检索增强 自定义属性 Advisor 工厂
   * <p>
   * 可选功能，自定义文档检索器、自定义查询增强器（例如空上下文处理器）
   */
  @Slf4j
  public class LoveAppRagCustomAdvisorFactory {
  
      /**
       * 创建自定义 Advisor
       *
       * @param vectorStore 向量存储
       * @param status      状态
       * @return Advisor 顾问
       */
      public static Advisor createRagCustomAdvisor(VectorStore vectorStore, String status) {
          Filter.Expression expression = new FilterExpressionBuilder()
                  .eq("status", status)
                  .build();
  
          DocumentRetriever documentRetriever = VectorStoreDocumentRetriever.builder()
                  .vectorStore(vectorStore)
                  .filterExpression(expression) // 只有满足这个过滤表达式的才能检索通过
                  .similarityThreshold(0.5) // 相似度过滤 只有达到这个相似度才能检索通过
                  .topK(3) // 检索前几个
                  .build();
  
  //        // 通过官方自带的检索增强顾问来 构建实例
  //        return RetrievalAugmentationAdvisor.builder()
  //                // 使用自定义文档检索处理
  //                .documentRetriever(documentRetriever)
  //                // 使用查询增强处理 空文档处理
  //                .queryAugmenter(LoveAppContextualQueryAugmenterFactory.createInstance())
  //                .build();
  
          // 这里采用自定义兜底策略 参数为：文档过滤检索实例，未符合重新检索次数，向量存储对象
          RetryableDocumentRetriever retryableDocumentRetriever = new RetryableDocumentRetriever(documentRetriever, 2, vectorStore);
          return RetrievalAugmentationAdvisor.builder()
                  // 使用自定义文档检索处理
                  .documentRetriever(retryableDocumentRetriever)
                  // 使用增强处理 空文档处理
                	.queryAugmenter(
              		LoveAppContextualQueryAugmenterFactory.createInstance())
                  .build();
  
      }
  }
  ```

  ```java
  /**
   * 自定义 兜底策略，采用重复检索策略，若没有检索到内容，则从备用的向量数据库中进行检索内容
   * 【注意】 不过使用这个就不能使用一些其他类指定的
   *  类似于 spring ai 官方的 VectorStoreDocumentRetriever 能够配置表达式过滤以及相似度过滤
   */
  @Slf4j
  public class RetryableDocumentRetriever implements DocumentRetriever {
      private final DocumentRetriever delegate; // 文档检索
      private final int maxRetries; // 最大次数
      private final VectorStore fallbackVectorStore; // 可选：备用向量库
  
      public RetryableDocumentRetriever(DocumentRetriever delegate, int maxRetries) {
          this.delegate = delegate;
          this.maxRetries = maxRetries;
          this.fallbackVectorStore = null;
      }
  
      public RetryableDocumentRetriever(DocumentRetriever delegate, int maxRetries, VectorStore fallbackVectorStore) {
          this.delegate = delegate;
          this.maxRetries = maxRetries;
          this.fallbackVectorStore = fallbackVectorStore; // 传入备用 store
      }
  
      @NotNull
      @Override
      public List<Document> retrieve(@NotNull Query query) {
          List<Document> results = Collections.emptyList();
  
          for (int i = 0; i < maxRetries; i++) {
              // 进行召回 判断召回文档数是否为0
              results = delegate.retrieve(query);
              if (!results.isEmpty()) {
                  log.info("✅ 第 {} 次尝试成功召回 {} 个文档", i + 1, results.size());
                  break;
              } else {
                  log.warn("⚠️ 第 {} 次尝试未召回任何文档，尝试重新检索...", i + 1);
                  try {
                      Thread.sleep(500); // 简单延迟
                  } catch (InterruptedException ignored) {
                  }
              }
          }
  
          // 若召回数据为空 有兜底的向量数据库
          if (results.isEmpty() && fallbackVectorStore != null) {
              log.warn("🔄 使用备用向量库进行兜底检索");
              results = List.of(new Document("#### 未成年\n" +
                      "未成年就需要玩游戏，推荐玩明日方舟"));
          }
  
          // 若召回数据为空 并且没有兜底备用向量数据库
          if (results.isEmpty()) {
              log.error("❎ 最终仍未召回任何文档，可返回默认内容或抛出异常");
              // 可选：抛出异常、返回默认文档等
  //            return getDefaultDocuments(); // 自定义兜底文档
              throw new RuntimeException("❌ 兜底失败，请查看RAG知识库内容是否符合");
          }
          return results;
      }
  }
  
  ```

  **测试 **当无法命中的时候，会进行**指定次数的自重试**，然后使用**备用向量数据库**进行查询，再没有查询到就使用**兜底文档**或者**抛出异常**

  





### Function Calling调用

#### 进阶知识

一般情况下，无需关心工具的调用内部细节，如果需要更加精细的控制，就需要自定义ToolCallResultConverter来实现特定的转换逻辑  即**通过指定的描述让ai理解query并调用指定的方法**

##### 工具上下文

用于自定义工具的获取的参数

> 例如：用户：提示说需要退票
>
> 调用工具：根据上下文进行退票操作，返回结果给ai（甚至使用及时返回特性，直接可以返回给用户）
>
> AI：退票成功

实现：

```java
// 添加工具的时候 添加上.toolContext(Map.of("userName", "yupi"))方法  该接收的参数就是一个简单的Map对象，用于生成工具

 @Tool(description = "退票系统")
    public String sendEmail(ToolContext toolContext) {
	String user= toolContext.getContext().get("user").toString();
    // 处理退票，不需要再次询问用户信息…………
}

// 调用工具类
String content = chatClient.prompt()
                .user(message)
                .advisors(new MyLogAdvisor())
                .tools(new SendEmailTool())
                .toolContext(Map.of("user", "zs"))
                .call()
                .content();
```



#### 立即返回

通过在`@Tool()`注解上 添加 `returnDirect=ture` 属性，实现直接返回的特性，直接返回并**不会再次发起请求**AI 大模型，减少消耗，优化体验 

```java
@Tool(description = "退票系统",returnDirect=ture)
```





### MCP协议

需要注意的是，MCP 仅仅只是一个协议，并没有什么技术的实现。是客户端与服务提供方之间的协议，通过MCP 同意服务接口调用规范，方便AI 开发工具进行调用（本质上还是方法调用）



#### Spring AI 集成 MCP

- ### 服务端

  引入依赖

  ```xml
  <!--spring ai mcp server-->
  <dependency>
      <groupId>org.springframework.ai</groupId>
      <artifactId>spring-ai-mcp-server-webmvc-spring-boot-starter</artifactId>
      <version>1.0.0-M6</version>
  </dependency>
  ```

  编写工具Tool

  ```java
  @Component
  public class ImageSearchTool {
      
      @Tool(description = "search image from web")
      public String searchImage(@ToolParam(description = "Search query keyword") String query) {
          try {
              // 调用搜索功能
              return String.join("\n", searchServer(query));
          } catch (Exception e) {
              return "Error search image: " + e.getMessage();
          }
      }
  }
  ```

  编写配置文件，记得使用sse模式的时候 注释main的内容，把stdio改为false

  ```yml
  spring:
    application:
      name: zn-image-search-mcp-server
    profiles:
      active: local
    ai:
      mcp:
        server:
          name: zn-image-search-mcp-server
          version: 0.0.1
          type: SYNC
          # sse 模式 记得把stdio设置为false
          stdio: true
    # stdio
    main:
      # 关闭web应用
      web-application-type: none
      # 关闭banner
      banner-mode: off
      
  server:
    port: 8127
  ```

  在项目启动的时候，注册Bean，把Tools加载到ToolCallbackProvider 注册为Bean

  ```java
  @SpringBootApplication
  public class ZnAgentImageSearchMcpServerApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(ZnAgentImageSearchMcpServerApplication.class, args);
      }
  
      // 启动的时候注册为Bean
      @Bean
      public ToolCallbackProvider imageSearchTools(ImageSearchTool imageSearchTool) {
          // 使用 MethodToolCallbackProvider 构建 工具provider
          return MethodToolCallbackProvider.builder()
                  .toolObjects(imageSearchTool)
                  .build();
      }
  
  }
  ```

  使用sdtio方式就直接打包就行了，如果使用sse方式，需要启动项目，然后访问项目接口地址：http://localhost:8127

- ### 客户端

  导入依赖

  ```xml
  <!--spring AI MCP Client-->
  <dependency>
      <groupId>org.springframework.ai</groupId>
      <artifactId>spring-ai-mcp-client-spring-boot-starter</artifactId>
      <version>${spring-ai.version}</version>
  </dependency>
  ```

  编写配置文件，需要注意的是，使用sse方式需要注释掉mcp配置文件的文件路径配置，直接注释stdio就行

  ```yml
  spring:
    # spring ai alibaba 使用SpringAI 调用大模型
    ai:
      mcp:
        client:
          stdio:
            # 指定MCP服务器配置文件路径（推荐）
            servers-configuration: classpath:/mcp-client.json
          # 使用 stdio 的时候 需要注释sse的方式，使用sse的时候，需要注释stdio的方式
  #        sse:
  #          connections:
  #            server1:
  #              # MCP 服务地址
  #              url: http://localhost:8127
  ```

  这里导入官方提供的工具回调提供对象，通过toolCallbackProvider自动注入tool工具到FunctionCallbacks集合中，这个集合装了所有的MCP的工具，以供AI根据query选择工具

  ```java
  @Resource
  private ToolCallbackProvider toolCallbackProvider;  // 官方提供的工具回调提供对象
  
  public String toolsMcpTest(String message) {
      String content = chatClient.prompt()
          .user(message)
          // 使用工具 这里导入的是 Spring ai 提供的 自动方法提供器
          .tools(toolCallbackProvider)
          .call()
          .content();
  		return content;
  }
  ```

  测试调用，ai会根据提供的Tool集合判断是否需要使用MCP，使用哪个MCP工具，然后进行调用，然后再返回结果

  ```java
  @Test
  void toolsSelfStdioMcpTest() {
      String answer = loveApp.toolsMcpTest(
              "帮我搜索一些鸭子的图片，注意使用英文进行搜索");
      log.info("\nAI 生成结果：{}", answer);
      Assertions.assertNotNull(answer);
  }
  ```



但是MCP有很大的**安全问题**，内部代码实现不透明，Ai模型的安全性没有保障，MCP 社区审核制度没有规范等一系列问题，导致MCP的接入具有风险性

​		

### Agent 智能体

类似于人类处理任务，具有问题分析、思考、调用方法，完成任务、得出结果。





#### 自定义Agent 智能体

开发自定义AIAgent

首先需要了解Agent的基础结构，这里参考的[OpenManus](https://github.com/FoundationAgents/OpenManus) 开源智能体项目的结构

![image-20250524150432745](https://img.znxs.vip/study/20250615153456206_image-20250524150432745.png)



基础结构：

- ![img](https://img.znxs.vip/study/20250615153458637_jLGa0c6lwYRhlaHu.webp)

- BaseAgent：最基础的代理抽象类，定义了所有代理的**基本状态管理和执行循环**

- ReActAgent：实现 ReAct 模式的代理，具有**思考**（Think）和**行动**（Act）两个主要步骤

- ToolCallAgent：能够调用工具的代理，继承自 ReActAgent 并**扩展了工具调用能力**

- Manus：具体实现的**智能体实例**，集成了所有能力并添加了更多专业工具

  



开发个人Manus 智能体

- ### 定义数据模型

  ```java
  
  ```

  









## Fix修复搜索工具使用问题

大伙使用文档中的SearchAPI **效果不佳**的，可以看看下面两个SearchAPI

### [Exa.ai](https://exa.ai/)

```bash
curl --request POST \
     --url https://api.exa.ai/search \
     --header 'content-type: application/json' \
     --header "x-api-key: you-api-key" \
     --data '{
    "query": "Find blog posts about AGI:",
    "contents": {
        "text": { "maxCharacters": 1000 }
    }
}'
```

### [Google SerpApi](https://serpapi.com/)

```bash
curl -X GET 'https://serpapi.com/search.json?q=搜索内容&engine=google&api_key=YOUR_API_KEY'
```

**下面是java代码（方便大伙查看，java代码放下面了）**

#### Exa 代码

```java
/**
 * Exa Web Search工具
 * 提供基于Exa的网络搜索功能
 */
@Slf4j
public class ExaWebSearchTool {

    private final String exaApiKey;

    private static final String SEARCH_API_URL = "https://api.exa.ai/search";

    public ExaWebSearchTool(String exaApiKey) {
        this.exaApiKey = exaApiKey;
    }

    /**
     * 执行网络搜索
     *
     * @param searchQuery 搜索内容
     * @return 搜索结果摘要列表
     */
    @Tool(description = "使用EXA提供的Web Search功能进行网络搜索。如果出现搜索失败，可以尝试多次调用该工具")
    public String exaSearch(
            @ToolParam(description = "搜索内容")
            String searchQuery) {
        log.info("api:{}", exaApiKey);
        log.info("调用 EXA API 搜索关键词：{}", searchQuery);

        try {
            // 1. 构建请求参数 Map，包含 contents.text.maxCharacters
            Map<String, Object> paramMap = new HashMap<>();
            paramMap.put("query", searchQuery);

            Map<String, Object> contents = new HashMap<>();
            Map<String, Object> text = new HashMap<>();
            text.put("maxCharacters", 1000); // 控制文本长度
            contents.put("text", text);
            paramMap.put("contents", contents);

            // 2. 转换为 JSON 字符串
            String requestBodyJson = JSONUtil.toJsonStr(paramMap);

            // 3. 发送 POST 请求
            HttpResponse response = HttpRequest.post(SEARCH_API_URL)
                    .header("x-api-key", exaApiKey)
                    .header("Content-Type", "application/json")
                    .body(requestBodyJson)
                    .execute();

            // 4. 获取响应状态码和内容
            int status = response.getStatus();
            String body = response.body();

            if (status == 200 && ObjectUtil.isNotEmpty(body)) {
                JSONObject jsonResponse = JSONUtil.parseObj(body);
                JSONArray resultsArray = jsonResponse.getJSONArray("results");
                if (resultsArray != null && !resultsArray.isEmpty()) {
                    StringBuilder resultBuilder = new StringBuilder();

                    for (int i = 0; i < resultsArray.size(); i++) {
                        JSONObject result = resultsArray.getJSONObject(i);

                        String title = result.getStr("title");
                        String url = result.getStr("url");
                        String snippet = "";

                        // 获取 text.snippet（如果存在）
                        if (result.containsKey("text") && result.get("text") instanceof JSONObject) {
                            JSONObject textObj = result.getJSONObject("text");
                            snippet = textObj.getStr("snippet");
                        } else {
                            snippet = "无摘要信息";
                        }

                        // 拼接结果
                        resultBuilder.append("【结果 ").append(i + 1).append("】\n");
                        resultBuilder.append("标题: ").append(title).append("\n");
                        resultBuilder.append("链接: ").append(url).append("\n");
                        resultBuilder.append("摘要: ").append(snippet).append("\n\n");
                    }

                    return resultBuilder.toString();
                } else {
                    return "未找到相关结果";
                }
            } else {
                log.error("请求失败，状态码：{}，响应内容：{}", status, body);
                return "请求失败或无返回内容";
            }
        } catch (Exception e) {
            log.error("调用 EXA 搜索服务时发生错误", e);
            throw new RuntimeException("调用 EXA 搜索请求出现错误", e);
        }
    }
}
```

#### Google SerpApi java代码

```java
/**
 * Google SerpApi 搜索工具
 * 提供基于 SerpApi 的 Google 网络搜索功能
 */
@Slf4j
public class GoogleWebSearchTool {

    private final String serpApiKey;

    private static final String SEARCH_API_URL = "https://serpapi.com/search.json";

    public GoogleWebSearchTool(String serpApiKey) {
        this.serpApiKey = serpApiKey;
    }

    /**
     * 执行 Google 网络搜索
     *
     * @param searchQuery 搜索内容
     * @return 搜索结果摘要列表
     */
    @Tool(description = "使用 SerpApi 提供的 Google 搜索功能进行网络搜索")
    public String googleSearch(
            @ToolParam(description = "搜索内容")
            String searchQuery) {
        log.info("调用 SerpApi Google 搜索关键词：{}", searchQuery);

        try {
            // 1. 构建请求 URL（使用 GET 查询参数）
            String url = SEARCH_API_URL + "?q=" + java.net.URLEncoder.encode(searchQuery, "UTF-8")
                    + "&engine=google"
                    + "&api_key=" + serpApiKey;

            // 2. 发送 GET 请求
            HttpResponse response = HttpRequest.get(url).execute();

            // 3. 获取响应状态码和内容
            int status = response.getStatus();
            String body = response.body();

            if (status == 200 && ObjectUtil.isNotEmpty(body)) {
                JSONObject jsonResponse = JSONUtil.parseObj(body);

                // 获取 organic_results（谷歌自然搜索结果）
                JSONArray resultsArray = jsonResponse.getJSONArray("organic_results");

                if (resultsArray != null && !resultsArray.isEmpty()) {
                    StringBuilder resultBuilder = new StringBuilder();

                    List<JSONObject> results = resultsArray.toList(JSONObject.class);
                    int index = 1;

                    for (JSONObject result : results) {
                        String title = result.getStr("title");
                        String link = result.getStr("link");
                        String snippet = result.getStr("snippet"); // 可能为空

                        resultBuilder.append("【结果 ").append(index++).append("】\n");
                        resultBuilder.append("标题: ").append(title).append("\n");
                        resultBuilder.append("链接: ").append(link).append("\n");
                        resultBuilder.append("摘要: ").append(ObjectUtil.defaultIfNull(snippet, "无摘要信息")).append("\n\n");
                    }

                    return resultBuilder.toString();
                } else {
                    return "未找到相关结果";
                }
            } else {
                log.error("请求失败，状态码：{}，响应内容：{}", status, body);
                return "请求失败或无返回内容";
            }
        } catch (Exception e) {
            log.error("调用 SerpApi Google 搜索服务时发生错误", e);
            throw new RuntimeException("调用 SerpApi Google 搜索请求出现错误", e);
        }
    }
}
```



linux 安装 （需要使用到docker-compose 命令）

```yml
version: '3.5'

services:
  etcd:
    container_name: milvus-etcd
    image: quay.io/coreos/etcd:v3.5.5
    restart: always
    environment:
      - ETCD_AUTO_COMPACTION_MODE=revision
      - ETCD_AUTO_COMPACTION_RETENTION=1000
      - ETCD_QUOTA_BACKEND_BYTES=4294967296
      - ETCD_SNAPSHOT_COUNT=50000
    volumes:
      - ${DOCKER_VOLUME_DIRECTORY:-.}/volumes/etcd:/etcd
    command: etcd -advertise-client-urls=http://127.0.0.1:2379 -listen-client-urls http://0.0.0.0:2379 --data-dir /etcd
    healthcheck:
      test: ["CMD", "etcdctl", "endpoint", "health"]
      interval: 30s
      timeout: 20s
      retries: 3

  minio:
    container_name: milvus-minio
    image: minio/minio:RELEASE.2023-03-20T20-16-18Z
    restart: always
    environment:
      MINIO_ACCESS_KEY: minioadmin
      MINIO_SECRET_KEY: minioadmin
    ports:
      - "9001:9001"
      - "9000:9000"
    volumes:
      - ${DOCKER_VOLUME_DIRECTORY:-.}/volumes/minio:/minio_data
    command: minio server /minio_data --console-address ":9001"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
      interval: 30s
      timeout: 20s
      retries: 3

  standalone:
    container_name: milvus-standalone
    image: milvusdb/milvus:v2.3.5
    restart: always
    command: ["milvus", "run", "standalone"]
    security_opt:
    - seccomp:unconfined
    environment:
      ETCD_ENDPOINTS: etcd:2379
      MINIO_ADDRESS: minio:9000
    volumes:
      - ${DOCKER_VOLUME_DIRECTORY:-.}/volumes/milvus:/var/lib/milvus
      - ${DOCKER_VOLUME_DIRECTORY:-.}/milvus.yaml:/milvus/configs/milvus.yaml  
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9091/healthz"]
      interval: 30s
      start_period: 90s
      timeout: 20s
      retries: 3
    ports:
      - "19530:19530"
      - "9091:9091"
    depends_on:
      - "etcd"
      - "minio"

  attu:
    container_name: attu
    image: zilliz/attu:v2.3.6
    restart: always
    environment:
      MILVUS_URL: milvus-standalone:19530
    ports:
      - "8070:3000"  # 外部端口8000可以自定义
    depends_on:
      - "standalone"

networks:
  default:
    name: milvus
```

启动命令

```bash
# 拉取镜像
docker-compose pull
 
# 启动容器
docker-compose up -d
 
# 查看启动状态（健康状态）
docker-compose ps -a
 
# 停止容器
docker-compose down
```



## Spring ai alibaba 整合 milvus 

```xml
<!--spring ai alibaba 整合 milvus-->
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-milvus-store-spring-boot-starter</artifactId>
    <version>1.0.0-M6</version>
</dependency>
```

**添加配置**

```yml
spring:  
  ai:
    vectorstore:
      milvus:
        client:
          host: 192.168.239.128 # milvus 地址
          port: 19530   # milvus 端口号
          username: root
          password: milvus
        embedding-dimension: 1536 # 向量维度
        initialize-schema: true  # 是否初始化
        collection-name: vector_store_milvus # 这里配置名称还是失效不知道为什么
        database-name: default
```



**添加数据**

插入数据需要**准备好文档**，这里采用的Markdown文档加载器提供的数据，文档存放到项目路径`/resources/doc`目录下了

下面这个只执行一次 记得执行完把configuration注释掉

```java
//@Configuration
@Slf4j
public class MilvusVectorVectorDataInit implements ApplicationRunner {

    private final MilvusVectorStore vectorStore;

    public MilvusVectorVectorDataInit(MilvusVectorStore vectorStore) {
        this.vectorStore = vectorStore;
    }

    @Resource
    private MarkDownLoader markDownLoader;

    // 标志位，确保只执行一次
    private final AtomicBoolean hasRun = new AtomicBoolean(false);


    @Override
    public void run(ApplicationArguments args) {

        if (hasRun.get()) {
            log.warn("MilvusVectorVectorDataInit 已执行过，跳过重复执行");
            return;
        }

        List<Document> documents = markDownLoader.loadMarkDowns("classpath:doc/*.md");
        int batchSize = 25;
        for (int i = 0; i < documents.size(); i += batchSize) {
            int end = Math.min(i + batchSize, documents.size());
            List<Document> batch = documents.subList(i, end);
            vectorStore.add(batch);
            log.info("Added document batch starting at {}", i);
        }
        log.info("Vector data ");
        hasRun.set(true); // 设置为已执行
    }
}
```



**查询数据库内数据**

```java
/**
 * 向量数据查询测试
 */
@GetMapping("/select")
public List<Document> search(@RequestParam("queryMessage") String queryMessage) {
    return milvusVectorStore.similaritySearch(
            SearchRequest.builder()
                    .query(queryMessage)
                    .topK(10).build());
}
```

**联合大模型**

```java
@GetMapping(value = "/chat/rag/milvus")
public Flux<String> generationChat(
        @RequestParam("userInput") String userInput,
        HttpServletResponse response
) {
    response.setCharacterEncoding("UTF-8");
    // 发起聊天请求并处理响应
    return ChatClient.builder(dashscopeChatModel)
            .defaultSystem(SYSTEM_PROMPT)
            .build().prompt()
            .user(userInput)
            // RAG检索增强生成 使用 milvus 向量库
            .advisors(new QuestionAnswerAdvisor(milvusVectorStore, SearchRequest.builder().build())
            ).stream()
            .content();
}
```





简历写法：

基于 Spring AI 与阿里 AI 框架开发智能体系统，用户输入需求后可自动生成旅游方案、图文内容并导出为 PDF。主要技术亮点包括：

- 集成 Spring AI Alibaba，实现自定义 Advisor、文档加载器与查询增强功能。
- 适用各种方式构建自定义RAG 知识库——基于自定义向量数据库、第三方自定义RAG、远程PGVector向量数据库、自定义检索增强，根据元信息的检索过滤，
- 对话记忆持久化：自主实现了基于文件系统的 ChatMemory，结合 Kryo 高性能序列化库保存对话历史数据，解决了服务重启后对话记忆丢失的问题，增强了系统稳定性。
- ETL 数据处理：基于 Spring AI 框架实现了对恋爱知识文档的完整 ETL 数据处理流程，使用 DocumentReader、DocumentTransformer 和 DocumentWriter 实现知识文档的抽取、转换和加载，提高了知识库构建效率。
- 图片搜索 MCP：利用 Spring AI MCP Server 集成 Pexels 图片 API 实现了图片搜索 MCP 服务，让 AI 能够联网检索图片资源；同时实现了 Stdio 和 SSE 两套传输模式，适应不同的部署场景。
- 分层智能体架构：参考 OpenManus 实现了拥有自主规划能力的分层 AI 智能体架构（BaseAgent、ReActAgent、ToolCallAgent），实现了高扩展性和可维护性。
- 询问用户等待机制：通过 BlockingQueue 实现 AI 询问用户等待机制，无需重启服务即可询问用户并继续执行任务，大大提高用户体验。









