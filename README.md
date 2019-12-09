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
    
___Spring-Boot___ 项目构建是<span>pom</span>文件中会继承父节点<span>spring-boot-starter-parent(这是项目自动构建时所继承的，当然根据公司需要可自定义父节点文件)</span>，
此父节点文件中继承了<span>spring-boot-dependencies</span>节点（其中根据<span>spring-boot</span>构建版本的不同指定了相关<span>jar</span>依赖，<span>plugin</span>依赖以及其对应的兼容版本）。
<span>spring-boot-starter-parent</span>节点中定义了<span>maven</span>默认的配置:
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
