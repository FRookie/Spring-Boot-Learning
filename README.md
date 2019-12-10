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
