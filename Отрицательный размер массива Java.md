# Отрицательный размер массива Java

Как это работает и почему это не падает я пока не понимаю:
```java
byte[] arr = new byte[-10];
System.out.println(arr.length); // -1
```
