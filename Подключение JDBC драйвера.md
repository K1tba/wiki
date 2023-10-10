# Подключение JDBC драйвера

### Maven

Подключить JDBS драйвер для [[PostgreSQL]] можно из [Maven репозитория](https://mvnrepository.com/artifact/org.postgresql/postgresql). Для этого в файле `pom.xml` указать:

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.6.0</version>
</dependency>
```