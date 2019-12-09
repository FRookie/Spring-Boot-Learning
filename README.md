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
    
Spring-Boot项目构建是pom文件中会继承父节点spring-boot-starter-parent(这是项目自动构建时所继承的，当然根据公司需要可自定义父节点文件)，
此父节点文件中继承了spring-boot-dependencies节点（其中根据spring-boot构建版本的不同指定了相关jar依赖，plugin依赖以及其对应的兼容版本）。spring-boot-starter-parent节点中定义了
