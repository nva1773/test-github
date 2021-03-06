<h2 align="center">Репозиторий для эксперентов и тренировки.</h2>

***

Вся инфорамция по работе с репозиториями взята здесь: [Git-Book](https://git-scm.com/book/ru/v1)

***
### Клонируем репозиторий с github.com

Устанавливаем на компьютер программу [Git](https://git-scm.com/downloads)

После создания пустого репозитория и добавления описания (description):

- нажимаем кнопку [Clone or download] и копируем **[url]**
- на компьютере открываем каталог, в котором будет хранится репозиторий
- нажимаем правую кнопку мыши и в выподающем списке выбираем **Git Bash Here**, тем самым запуская **git-bash.exe**
- в появившемся окне, похожем на **CMD**, только начинающемся с символа **$**, вводим: **$ git clone [url]**

в результате в данном каталоге будет создан клон репозитория.

***
### Создание репозитория в существующем каталоге

Переходим в проектный каталог, запускаем git-bash:

- в командной строке вводим: **$ git init**, в результате создаестся подкоталог .git содержащий файлы репозитория
- командой:
  - **$ git add -A**, проиндексируем (добавим) все файлы текущего каталога
  - **$ git commit -m "creat project"**, добавляем начальный коммит,
- eсли гит ругнется то необходимо сообщить Гиту свое имя и почту:
  - **$ git config --global user.name "user"**, сообщаем свое имя
  - **$ git config --global user.email "mail"**, сообщаем свою почту, и еще раз пробуем создать коммит
- далее командой:
  - **$ git remote add origin [url]**, добавляем новый удалённый Git-репозиторий под сокращеным именем **origin**
  - **$ git push origin master**, переносим все локальные изменения в удаленный репозитория origin в ветку master

***
### Исключение файла и каталогов из индексации

Для исключения из индексации необходимо создать файл **.gitignore** и поместить его в корень.

Вот пример содержания файла .gitignore:

    # символ комментария
    # Исключим из индексации (игнорируем) сам файл .gitignore
    .gitignore
    # игнорировать все файлы в каталоге tmp/
    tmp/
    # не обрабатывать файлы, имя которых заканчивается на .a
    *.a
    # НО отслеживать файл lib.a, несмотря на то, что мы игнорируем все .a файлы с помощью предыдущего правила
    !lib.a
    # игнорировать любые файлы заканчивающиеся на .o или .b - объектные и бинарные файлы сборки кода.
    *.[ob]
    # игнорировать все файлы заканчивающиеся на тильду (~), которая используется во многих текстовых редакторах
    *~
    # игнорировать все .txt файлы в каталоге doc/ 
    # Шаблон **/ доступен в Git, начиная с версии 1.8.2.
    doc/**/*.txt

***
### Настройки прокси сервера

> Если на Вашем комьютере для подключения к интернету используется прокси сервер, то выволняем следующую команду:

**$ git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080**

где: **proxyuser** и **proxypwd** - ваши учетные данные, а proxy.server.com:8080 - имя Вашего сервера и порт.

> Если же раньше, при подключении к интернету использовался прокси, а теперь нет, может выдоваться ошибка:

    Failed to connect to https.proxy port 3128: Timed out

где: http.proxy ip address proxy server, например 10.99.12.27

Но на саммом деле прокси уже давно не используется, значит надо изменить настроки Гит через локальный прокси:

    $ git config --global http.porxy localhost:3128

и после этого выполняем команду:

    $ git config --global --unset http.proxy

тепер смотри настойки Гит:

    $ git config --global --list
    user.name="name"
    user.email="mail"
    credential.helper=manager
    http.porxy=localhost:3128

пробуем клонировать еще раз:

    $ git clone [url]

***
### Как удалить ненужный коммит

> Если нужно сбрость ВСЮ ветку до ненужного коммита, то необходимо проделать следующее:

- Получаем [хэш-код] коммита, с которым хотим удалить информацию в ветке, например: f067e167a766707956982e8431979c9467f2f81e
- Заходим в папку репозитория, запускаем git-bash и выполняем команды:
  - **$ git reset --hard f067e167a766707956982e8431979c9467f2f81e**
  - **$ git push --force**

> Если нужно удалить конкретный коммит, то используем команду rebase:

- **$ git rebase -i %X%** , гдe **X** хеш коммита, идушего перед неправильным
- **$ git rebase -i @~N** , где **N** количество коммитов, которое прошло с коммита, идущего перед неправильным

Запустится текстовый редактор, в нём заменяете **pick** у неправильного коммита на **drop**. Сохраняем и закрываем. Гит вычистит его.

- **$ git push -f** , фиксируем изменения.

> Если нужно удалить последний коммит у себя и на сервере (вариант когда седлал коммит, запушил и понял, что поспешил), то необходимо проделать следующее (предупредив об этом партнеров, ведь они уже могли забрать изменения к себе):

- убеждаемся, что находимся на нужной ветке, выполнив команду:
  - **$ git checkout**
- удаляем поледний коммит у себя (сбросим историю назад на одну фиксацию, но оставим рабочее дерево со всеми изменениями) и фиксируем изменения на сервере:
  - **$ git reset HEAD^**
  - **$ git push -f**
  
- теперь делаем необходимые изменения и дополнения, индексируем их и создаем новый коммит:
  - **$ git add -A**
  - **$ git commit -m "New commit..."**
  - **$ git push -f"**
  
### Откат назад, т.е. удалить изменения в рабочей области и вернуть ее к состоянию как при последнем коммите

> Если нужно избавиться от изменений в конкретном файле:

- **$ git checkout -- path/file.abc** , где path/file.abc - пуль и имя файла

> Только те изменения, которые еще не были индексированы (командой add), через карман:

- **$ git stash save --keep-index**

> Все индексированные и неиндексированные изменения в рабочей области, сохрання их в карман:

- **$ git stash save**

> Если вы хотите вернеть прикарманеные измениния назад:

- **$ git stash apply**

> Если изменения точно не нужны, то просто выбрасываем их из кармана:

- **$ git stash drop**

> Отменить фиксацию и повторить коммит с новыми изменениями:

- **$ git commit ...**
- **$ git reset --soft HEAD ^**
- **edit...**
- **$ git commit -a -c ORIG_HEAD**

***

