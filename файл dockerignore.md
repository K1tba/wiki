# файл dockerignore

При создании [[Image - образы Docker|образов]] в них не требуется записывать такие вещи как папка `node_modules`, файл с инструкцией по созданию образа `Dockerfile`, папку `.git`  т.д. Для этого надо создать файл `.dockerignore` и по аналогии с [[GIT]] и его `.gitignore`  заполнить. Пример:

```.dockerignore
node_modules
.git
.idea
Dockerfile
```