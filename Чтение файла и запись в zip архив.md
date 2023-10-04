# Чтение файла и запись в zip архив

### Вариант с чтением всего файла целиком

Подходит если считываемые файлы не слишком большого размера. Для чтения файла создаётся массив байтов, размер которого определяется размером файлового потока. Затем все байты из потока пишутся в буфер и буфер передаётся потоку записи в архив.

Если файл слишком большой, то [[Java]] приложение вылетает с ошибкой. Ошибка возникает из-за того, что идёт попытка запросить памяти больше чем есть в виртуальной машине. 

> В [[Java]] есть ограничение на размер массива, даже если он не превышен, то с расходованием памяти происходит следующее: например надо создать массив типа `byte` на 100 записей. 1 байт занимает 1 байт :), следовательно массив при инициализации запросит 100 байт. Если создавать такой же массив типа `int`, то будет запрошено 400 байт. Т.к. тип `int` занимает 4 байта памяти.

```java
String archive = Paths.get("out.zip").toString();

// создание потока для записи в массив
try(ZipOutputStream zout = new ZipOutputStream(new FileOutputStream(archive))){
    
    Path file = Paths.get("myfile.png");
    String relativePath = Paths.get("sub/dir", file.toString()).toString();

	// создание потока для чтения файла
	try(FileInputStream fis = new FileInputStream(file.toString());) {

		// создание новой записи и добавление её в архив
        ZipEntry entry = new ZipEntry(relativePath);
        zout.putNextEntry(entry);
        // создаётся массив размером с файл
        byte[] buffer = new byte[fis.available()];
        // файловый поток читается в буфер целиком
        fis.read(buffer);
        // буфер пишется в поток архива
        zout.write(buffer);
        // закрыть текущую запись        
        zout.closeEntry(); 
    }
    catch(Exception ex){
        System.out.println(ex.getMessage());
    } 
} 
catch(Exception ex){
    System.out.println(ex.getMessage());
} 
```

### Вариант с чтением всего файла  частями

Подходит для любых ситуация, в том числе для архивирования больших файлов. Здесь также создаётся массив байтов, но уже фиксированной длины. Затем файловый поток считывается в цикле чанками на размер буфера и буфер записывается в поток записи архива.

```java
String archive = Paths.get("out.zip").toString();

// создание потока для записи в массив
try(ZipOutputStream zout = new ZipOutputStream(new FileOutputStream(archive))){
    
    Path file = Paths.get("myfile.png");
    String relativePath = Paths.get("sub/dir", file.toString()).toString();

	// создание потока для чтения файла
	try(FileInputStream fis = new FileInputStream(file.toString());) {
        // создание массива байтов
        int  bufferSize = 64000;
        byte buffer[]   = new byte[64000];

		// создание новой записи и добавление её в архив
        ZipEntry entry = new ZipEntry(relativePath);
        zout.putNextEntry(entry);

		// чтение файла
        while (fis.available() > 0) {
            if (fis.available() < 64000) {
                bufferSize = fis.available();
            }
            // файловый поток читается в буфер
            fis.read(buffer, 0, bufferSize );
            // буфер пишется в поток архива
            zout.write(buffer);
        }
        // закрыть текущую запись        
        zout.closeEntry(); 
    }
    catch(Exception ex){
        System.out.println(ex.getMessage());
    } 
} 
catch(Exception ex){
    System.out.println(ex.getMessage());
} 
```

#java #fileinputstream #zipoutputstream