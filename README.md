# Spring-Boot-Learning
springboot学习资料分析(中/英PDF)
# 依赖添加与总结

## 添加格式：
    父节点：
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>###</version>
        <relativePath/> <!-- lookup parent from repository -->
      </parent>
     子依赖：
     <dependencies>
        <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
      </dependencies>
    
___Spring-Boot___ 项目构建是 <kbd>pom</kbd>文件中会继承父节点<kbd>spring-boot-starter-parent</kbd>(这是项目自动构建时所继承的，当然根据公司需要可自定义父节点文件)，
此父节点文件中继承了<kbd>spring-boot-dependencies</kbd>节点（其中根据<kbd>spring-boot</kbd>构建版本的不同指定了相关<kbd>jar</kbd>依赖，<kbd>plugin</kbd>依赖以及其对应的兼容版本）。
<kbd>spring-boot-starter-parent</kbd>节点中定义了<kbd>maven</kbd>默认的配置:
### 文件解析格式以及路径：
          <resource>
            <filtering>true</filtering>
            <directory>${basedir}/src/main/resources</directory>
            <includes>
              <include>**/application*.yml</include>
              <include>**/application*.yaml</include>
              <include>**/application*.properties</include>
            </includes>
          </resource>    
### 日期格式初始化：
      <configuration>
        <verbose>true</verbose>
        <dateFormat>yyyy-MM-dd'T'HH:mm:ssZ</dateFormat>
        <generateGitPropertiesFile>true</generateGitPropertiesFile>
        <generateGitPropertiesFilename>${project.build.outputDirectory}/git.properties</generateGitPropertiesFilename>
      </configuration>
### 字符编码：
        UTF-8
...<br/> 

### Application事件和监听器
___(注：通常不需要使用<kbd>application</kbd>事件,但在某些场合除外，如在<kbd>Spring Boot</kbd>内部处理各种任务。Skr~Skr~Skr...)___

    除了常见的Spring事件(ContextRefreshedEvent)，SpringApplication也会发送其它的Application事件。
    这些事件有些是在ApplicationContext创建之前触发的，因此这些事件不能通过@Bean来注册监听器，只能通过SpringApplication.assListeners(...)或SpringApplicationBuilder.listeners(...)方法去注册。
    如果想让监听器去自动注册而不去关心应用的创建方式，可以在工程中添加META-INFO/spring.factories文件，并使用org.springframework.context.ApplicationListener作为Key指向监听器，如：

       org.springframework.context.ApplicationListener=com.example.project.MyListener
   
 应用运行时，事件会按照以下次序来执行：
 
 > + 在运行开始，但除了监听器注册和初始化之外的任何处理之前，会发送一个ApplicationStartedEvent。
 > + 在Environment将被用于已知的上下文，但在上下文被创建之前，会发送一个ApplicationEnvironmentPreparedEvent。
 > + 在refresh开始之前，但在Bean加载后会发送一个ApplicationPreparedEvent。
 > + 在refresh开始之后，相关的回调处理完，会发送一个ApplicationReadyEvent，表示应用准备好接受请求。
 > + 如果启动过程中出现异常，则会发送一个ApplicationFailedEvent。


# 注解解析
___@RestController___:这是SpringMVC中的注解，是<kbd>@Responsebody</kbd>和<kbd>@Controller</kbd>的结合。注明这是一个<kbd>Controller</kbd>类
用于<kbd>URL</kbd>请求所访问和映射的类，是一个支持<kbd>REST</kbd>的注解，且指明返回体为字符串类型。<br/>

___@ResquestMapping___:此注解指明路由信息，任何请求都会根据路由地址映射到对应的方法上。<br/>

___@EnableAutoConfiguration___:这个注解告诉<kbd>Spring Boot</kbd>根据添加的<kbd>jar</kbd>依赖猜测你想如何配置<kbd>Spring</kbd>。由于 <kbd>spring-boot-starter-web</kbd> 添加了<kbd>Tomcat</kbd>和<kbd>Spring MVC</kbd>，所以<kbd>auto-configuration</kbd>将假定你正在开发一个<kbd>web</kbd>应用，并对<kbd>Spring</kbd>进行相应地设置。<br/>

___@Entity___:用于标注在数据库表映射的实体类上,表明该实体类被<kbd>JPA</kbd>所管理.

___@Table___:应用于实体类，通过<kbd>name</kbd>属性指定对应该实体类映射表的名称.

___@Id___:应用于实体类的属性或者属性所对应的<kbd>getter</kbd>方法上，表明该属性映射为数据表的主键。

___@GeneratedValue___:与<kbd>@Id</kbd>一起使用，表明主键的生成策略，可以通过<kbd>strategy</kbd>属性指定。

    ### 生成策略：
        + AUTO : JPA自动选择的策略，是默认选项。
        + IDENTITY : 采用数据库<kbd>ID</kbd>自增长的方式生成主键值，Oracle不支持此策略。
        + SEQUENCE : 通过序列产生主键，通过<kbd>@SequenceGenerator</kbd>注解指定序列名，Mysql不支持此策略。
        + TABLE : 采用表生成方式来生成主键值，这种方式比较通用，但是效率较低。

___@Basic___:应用于属性，表示该属性映射到数据库表，<kbd>@Entity</kbd>标注的实体类的所有属性，默认即为<kbd>@Basic</kbd>.其有两个属性：

>> 1. <kbd>fetch:</kbd>属性的读取策略，有<kbd>EAGER</kbd>和<kbd>LAZY</kbd>两种取值。分别表示主动抓取策略和延迟加载策略，默认为<kbd>EAGER</kbd>。

>> 2. <kbd>optional:</kbd>表示该属性是否允许为<kbd>NULL</kbd>,默认为<kbd>TRUE</kbd>。

>> <kbd>@Basic(fetch = FetchType.LAZY)</kbd>标注某属性时，表示只有调用Hibernate对象的该属性的get方法时，才会从数据库表中查找对应该属性的字段值。

___@Column___:应用于实体类的属性，可以指定该数据库表字段的名字和其他属性。其属性包括：

> + <kbd>name</kbd>: 表示数据库表中该字段的名称，默认情形属性名称一致。

> + <kbd>nullable</kbd>: 表示该字段是否允许为<kbd>null</kbd>,默认为<kbd>true</kbd> .

> + <kbd>unique</kbd>: 表示该字段是否为唯一标识，默认为<kbd>false</kbd>。

> + <kbd>insertable</kbd>： 表示在<kbd>ORM</kbd>框架执行插入操作时，该字段是否应出现<kbd>INSERT</kbd>语句中，默认为<kbd>true</kbd>。

> + <kbd>length</kbd>：表示该字段的大小，仅对<kbd>String</kbd>类型的字段有效。

> + <kbd>updateable</kbd>：表示在<kbd>ORM</kbd>框架执行更新操作时，该字段是否应该出现在<kbd>UPDATE</kbd>语句中，默认为<kbd>true</kbd>。对于已经创建就不能更改的字段，该属性非常有用，比如<kbd>email</kbd>属性。

> + <kbd>columnDefinition</kbd>：表示该字段在数据库表中的实际类型。通常<kbd>ORM</kbd>框架可以根据属性类型自动判断数据库中字段的类型，但是有例外：

>> +    1. Date 类型无法确定数据库中字段类型究竟是DATE、TIME还是TIMESTAMP。

>> +    2. String 的默认映射类型是VARCHAR，如果希望将String类型映射到特定数据库的BLOB或TEXT字段类型，则需要进行设置。

___@Transient___: 应用在实体类属性上，表示该属性不映射到数据库表，<kbd>JPA</kbd>会自动忽略该属性。

___@Temporal___: 应用到实体类属性上，表示该属性映射到数据库是一个时间类型，具体定义为：

> + <kbd>@Temporal(TemporalType.DATE)</kbd>映射为日期date（只有日期）
> + <kbd>@Temporal(TemporalType.TIME)</kbd>映射为日期time（只有时间）
> + <kbd>@Temporal(TemporalType.TIMESTAMP)</kbd>映射为日期datetime（日期+时间）

# Logging

日志级别优先级由低到高分为：<br/>

>> <kbd>TRACE</kbd>、<kbd>DEBUG</kbd>、<kbd>INFO</kbd>、<kbd>WARN</kbd>、<kbd>ERROR</kbd>、<kbd>FETAL</kbd>、<kbd>OFF</kbd>

&nbsp;&nbsp;&nbsp;Spring Boot的默认日志配置会在写日志消息时回显到控制台，级别为<kbd>INFO</kbd>、<kbd>WARN</kbd>、<kbd>ERROR</kbd>的消息会被记录。启动应用时，通过 '--debug' 标识开启控制台的<kbd>DEBUG</kbd>级别日志记录，也可以在 'application.properties' 中配置指定。当<kbd>DEBUG</kbd>模式启用时，一系列核心<kbd>loggers</kbd>(内嵌容器、Hibernate、Spring Boot等)记录的日志会变多，但不会输出所有的信息。<br/>
相应的，可以通过指定 '--trace' 启动<kbd>trace</kbd>模式来追踪核心<kbd>loggers</kbd>(内嵌容器、Hibernate生成的Schema、Spring全部的portfolio )的所有日志信息。

## 文件输出
   
   > 默认情况，Spring Boot只会讲日志信息打印到控制台而不会输出到日志文件中。但是可以通过<kbd>logging.file</kbd>或<kbd>logging.path</kbd>属性来设置。日志文件每达到10M就会被分割。

# HttpMessageConverters
   
 > + <kbd>Spring MVC</kbd>使用<kbd>HttpMessageConverter</kbd>接口转换 HTTP 请求和响应，其默认配置可以将对象转换为<kbd>JSON</kbd>(使用<kbd>Jackson</kbd>库)或<kbd>XML</kbd>文件，字符串默认使用<kbd>UTF-8</kbd>编码。
 > + 可以使用<kbd>Spring Boot</kbd>的<kbd>HttpMessageConverters</kbd>类添加或自定义转换类。

    @Configuration
    public class MyConfiguration {
        @Bean
        public HttpMessageConverters customConverters() {
            HttpMessageConverter<?> additional = ...
            HttpMessageConverter<?> another = ...
        return new HttpMessageConverters(additional, another);
        }
    }
 
 &nbsp;&nbsp;&nbsp;&nbsp;如上程序中，上下文出现的所有HttpMessageConverter bean都会被添加到converters列表中，因此可以使用这种方式来覆盖默认的转换器。
 
# 自定义JSON序列化和反序列化

 > + 如果使用<kbd>Jackson</kbd>序列化，反序列化Json数据，可以自定义JsonSerializer和JsonDeserializer类。<br/>
 > + 自定义序列化器（serializers）通常使用Module注册到Jackson，但是Spring Boot提供了@JsonComponent注解这个替代方式，将序列化器Spring Beans。
 


