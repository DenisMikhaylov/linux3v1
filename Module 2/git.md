Упарвление GIT

Установка GIT
```
# apt install git
```

Запуск создани ябазы даных для папки /etc

```
#cd /etc
#git init

```

Настройка конфигурации 
```
git config user.name 'student'
git config user.email 'student@test.tu'
```

Добавление в репозитарий всей папки /etc
```
git add *
```
Коммит репозитария
```
git commit -m 'Add all file etc'
```
Лог git

```
# git log
```

Изменяем файл в папке и проверяем отслеживание изменений
```
git diff
```
Создание альтернативного репозитария

```
git branch second
```
Просмотреть список репозитариев

```
git branch
```
Переключение между репозитариями
```
git checkout second
```
