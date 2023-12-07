# log4j

Ведение журналов в [[Java]].

[Документация](https://logging.apache.org/log4j/2.x/manual/api.html)
[Конфигурация](https://logging.apache.org/log4j/2.x/manual/configuration.html)
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

Добавить файл `log4j2.xml` в директорию: `<project_name>/src/main/resources`. По умолчанию [[Maven плагин ресурсов]] включит файл именно из этой директории. Если надо указать иной путь, то [читай это и смотри примеры](https://maven.apache.org/plugins/maven-resources-plugin/examples/resource-directory.html).

Пример файла конфигурации. В этой конфигурации логгера используются 2 аппендера, которые относятся к рутовому логгеру:

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

Может существовать несколько логгеров, причём логгер `ROOT` будет в любом случае.

Ниже пример конфигурации с двумя логгерами, причем рутовый пишет в консоль всё, а логгер `test` пишет в файл только сообщения уровня `warn` и выше:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Properties>
      <Property name="filename">log/test.log</Property>
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
    <Logger name="test" level="warn" additivity="false">
      <AppenderRef ref="File"/>
    </Logger>
    <Root level="all">
      <AppenderRef ref="Console"/>
    </Root>
  </Loggers>
</Configuration>
```

##### Пояснения конфигурации (выше)

1) `<Configuration status="WARN">` здесь `status="WARN"` относится к логированию самого `log4j2`, если ошибки возникают в самом логгере
2) блок `<Property name="filename">log/test.log</Property>` устанавливает переменную `filename`, которую потом можно использовать в конфигурации, как это сделано в строке `<File name="File" fileName="${filename}">`
3) в блоке `<Appenders>` устанавливаются 2 аппендера, каждый со своим именем, затем логгеры используют эти имена так `<AppenderRef ref="Console"/>`
4) тег `<PatternLayout>` может быть записан 2-мя способами, оба здесь представлены
5) аппендер `<File name="File" fileName="${filename}">` подхватывает имя файла `log/test.log`, при этом если директория `log` не существует - она будет создана
6) для использования логгера с именем `test` он должен быть создан `private static final Logger logger2 = LogManager.getLogger("test");` (см. пример использования ниже)
7) `additivity="false"` если установлена в `true`, то вызовы логгера `test` будут переданы в родительский логгер `root`

пример вывода в консоль с `additivity="false"`:

```
16:02:06.965 [main] INFO  ru.sgn74.signum.Signum - hello world
16:02:06.967 [main] WARN  ru.sgn74.signum.Signum - warning ahtung
16:02:06.967 [main] ERROR ru.sgn74.signum.Signum - error app
16:02:06.969 [main] WARN  test - 2 warning ahtung
16:02:06.969 [main] ERROR test - 2 error app
```

fdsf

#### Использование

```java
package com.signal.test;

import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

public class Test {
    private static final Logger logger = LogManager.getLogger(Test.class);
    private static final Logger logger2 = LogManager.getLogger("test");
    
    public static void main(String[] args){  
	    logger.info("hello world");
        logger.warn("warning ahtung");
        logger.error("error app");
        
        logger2.info("2 hello world");
        logger2.warn("2 warning ahtung");
        logger2.error("2 error app");
    }
}
```






#log4j