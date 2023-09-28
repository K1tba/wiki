
# Проблемы при компиляции проекта [[Java]]

### Проблема переноса проекта при разработке

Проблема замечена при разработке под Windows и последующем переносе под [[Ubuntu]]. Перенесённый [[Java]] код не компилировался в IDE NetBeans.
Решение: добавить в файл `pom.xml` следующую запись:

```xml
...
<build>
    <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
    </plugins>
</build>
```

### Проблема компиляции проекта и запуска jar файла

Проблема возникла при компиляции [[Java]] в IDE NetBeans и запуском под [[Ubuntu]]. После компиляции и раздачи прав на исполнение jar файла проект падал с ошибкой.
Решение: добавить в файл `pom.xml` следующую запись:
```xml
...
<build>
	<plugins>
	    <plugin>
	        <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-jar-plugin</artifactId>
	            <version>2.3.2</version>
	            <configuration>
	                <archive>
	                    <manifest>
	                      <addClasspath>true</addClasspath>
	                      <mainClass>com.game.fool.Fool</mainClass>
	                    </manifest>
	                </archive>
	            </configuration>
		</plugin>
	<plugins>
</build>
```
Решение подсмотрено  [здесь](https://stackoverflow.com/questions/12266735/no-main-manifest-attribute-in-jar-netbeans)
