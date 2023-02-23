---
title: Failed to scan
tag: maven
categories: 2023
---
 Failed to scan [file:/F:/maven/repository/org/springframework/spring-aspects/5.1.9.RELEASE/spring-aspects-5.1.9.RELEASE.jar] from classloader hierarchy
java.util.zip.ZipException: error in opening zip file
<!-- more -->


```
mvn install:install-file -DgroupId=com.qiyuesuo.sdk -DartifactId=sdk-java -Dversion=3.4.1 -Dpackaging=jar -Dfile=D:\jar\sdk-java-3.4.1.jar
```

```
F:\maven\repository\org\springframework\spring-aspects\5.1.9.RELEASE
```

org.springframework.spring-aspects

```
mvn install:install-file -DgroupId=org.springframework.spring-aspects
-DartifactId=spring-aspects  -Dversion=5.3.1 -Dpackaging=jar -Dfile=F:\maven\spring-aspects-5.3.1.jar
```

mvn install:install-file -DgroupId=org.springframework.spring-aspects
-DartifactId=spring-aspects  -Dversion=5.3.1 -Dpackaging=jar -Dfile=F:\maven\spring-aspects-5.3.1.jar