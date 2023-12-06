# log4j

Ведение журналов в [[Java]].

[Документация](https://logging.apache.org/log4j/2.x/manual/api.html)
#### Включение в проект

Для maven добавить в файл `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.21.1</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.21.1</version>
    </dependency>
</dependencies>
```


#### Конфигурация

Добавить файл `log4j2.xml` в директорию: `<project_name>/src/main/resources`. По умолчанию [[Maven плагин ресурсов]] включит файл именно из этой директории. Если надо указать иной путь, то [[mavesrc/main/resources|читай это и смотри примеры]].

Пример файла конфигурации:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Properties>
      <Property name="filename">target/test.log</Property>
    </Properties>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        <File name="File" fileName="${filename}">
            <PatternLayout>
              <pattern>%d %p %C{1.} [%t] %msg%n</pattern>
            </PatternLayout>
        </File>
    </Appenders>

    <Loggers>
        <!-- Root Logger -->
        <Root level="all" additivity="true">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>
```

#### Использование

```java
package com.signal.test;

import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

public class Test {
    private static final Logger logger = LogManager.getLogger(Test.class);
    
    public static void main(String[] args){  
       logger.info("Hello, World!");
       logger.warn("warning!");
       logger.error("err!");
    }
}
```






#log4j