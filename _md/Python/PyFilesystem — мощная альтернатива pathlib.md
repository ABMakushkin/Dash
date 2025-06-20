2024-11-18
#python 

[PyFilesystem — мощная альтернатива pathlib / Хабр](https://habr.com/ru/companies/skillfactory/articles/577836/)


[Автор оригинала: Will McGugan](https://docs.pyfilesystem.org/en/latest/guide.html)

![](https://habrastorage.org/r/w1560/getpro/habr/upload_files/18e/3d2/844/18e3d2844393a55a197ce5cf404db045.jpg)

Написанная с помощью PyFilesystem функция поиска дубликатов файлов будет работать без изменений с жёстким диском, zip-файом, FTP-сервером, Amazon S3 и т. д., этот API абстрагирует от физического расположения файла. В нём меньше способов выстрелить себе в ногу, чем у модулей os и io. Руководством из документации делимся к старту курса по [Fullstack-разработке на Python](https://skillfactory.ru/python-fullstack-web-developer?utm_source=habr&utm_medium=habr&utm_campaign=article&utm_content=coding_fpw_140921&utm_term=lead).

---

## И ещё раз о причинах

Пока для выбранной вами файловой системы (или любого хранилища данных, напоминающего файловую систему) существует объект FS, вы можете использовать тот же API, то есть отложить решение о месте хранения данных. Если вы решите хранить конфигурацию в _облаке_, это может обойтись изменением одной строки.

PyFilesystem также может быть полезна для юнит-тестирования. Поменяв файловую систему ОС на файловую систему в памяти, вы можете писать тесты без необходимости управляться вводом-выводом или имитацией. И вы можете быть уверены, что ваш код будет работать на Linux, MacOS и Windows.

## Открытие файловых систем

Есть два способа открыть файловую систему. Первый и наиболее естественный способ — импортировать соответствующий класс файловой системы и создать его экземпляр. Вот как можно открыть [OSFS](https://docs.pyfilesystem.org/en/latest/reference/osfs.html#fs.osfs.OSFS) (файловую систему операционной системы), которая соответствует файлам и каталогам вашего жёсткого диска:

```
>>> from fs.osfs import OSFS>>> home_fs = OSFS("~/")
```

Код создаёт объект FS, который управляет файлами и каталогами по заданному системному пути. В данном случае '~/', — ярлык вашего домашнего каталога. Вот как можно перечислить файлы/каталоги в вашем домашнем каталоге:

```
>>> home_fs.listdir('/')['world domination.doc', 'paella-recipe.txt', 'jokes.txt', 'projects']
```

Обратите внимание: параметр listdir — это один прямой слеш, означающий, что мы хотим список _корневой_ файловой системы. Так написано потому, что, с точки зрения home_fs для конструирования OSFS использовалась корневая директория.

Также обратите внимание, что это прямой слэш, даже в Windows: пути FS согласованы в формате и не зависят от платформы. Такие детали, как разделитель и кодировка, абстрагируются. Подробности смотрите [здесь](https://docs.pyfilesystem.org/en/latest/concepts.html#paths). Другие интерфейсы файловых систем могут иметь другие требования к своему конструктору. Например, вот как можно открыть файловую систему FTP:

```
>>> from ftpfs import FTPFS>>> debian_fs = FTPFS('ftp.mirror.nl')>>> debian_fs.listdir('/')['debian-archive', 'debian-backports', 'debian', 'pub', 'robots.txt']
```

Второй, более распространённый способ открытия объектов файловой системы — через _opener_, который открывает файловую систему, исходя из URL-подобного синтаксиса. Вот как ещё можно открыть домашний каталог:

```
>>> from fs import open_fs>>> home_fs = open_fs('osfs://~/')>>> home_fs.listdir('/')['world domination.doc', 'paella-recipe.txt', 'jokes.txt', 'projects']
```

Система opener особенно полезна, когда вы хотите хранить физическое расположение файлов приложения в конфигурационном файле. Если вы не укажете протокол в URL FS, то PyFilesystem будет считать, что вы хотите работать с OSFS относительно текущего рабочего каталога. Поэтому эквивалентный способ открыть домашний каталог следующий:

```
>>> from fs import open_fs>>> home_fs = open_fs('.')>>> home_fs.listdir('/')['world domination.doc', 'paella-recipe.txt', 'jokes.txt', 'projects']
```

## Печать дерева файлов

Вызов [tree()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.tree) на объекте FS распечатает ASCII-дерево вашей файловой системы. Это полезно в отладке:

```
>>> from fs import open_fs>>> my_fs = open_fs('.')>>> my_fs.tree()├── locale│   └── readme.txt├── logic│   ├── content.xml│   ├── data.xml│   ├── mountpoints.xml│   └── readme.txt├── lib.ini└── readme.txt
```

## Закрытие

Метод [close()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.close) выполнит необходимую очистку. Во множестве файловых систем (например, [OSFS](https://docs.pyfilesystem.org/en/latest/reference/osfs.html#fs.osfs.OSFS)) close делает очень мало. Другие файловые системы могут завершать обработку файлов или освобождать ресурсы только один раз, как только вызван close(). Завершив работу, можно явно вызвать этот метод:

```
>>> home_fs = open_fs('osfs://~/')>>> home_fs.writetext('reminder.txt', 'buy coffee')>>> home_fs.close()
```

Если вы используете объекты FS в качестве контекстного менеджера, close будет вызываться автоматически. Следующий пример эквивалентен предыдущему:

```
>>> with open_fs('osfs://~/') as home_fs:...    home_fs.writetext('reminder.txt', 'buy coffee')
```

Рекомендуется использовать объекты FS в качестве контекстного менеджера: это гарантирует закрытие каждой FS.

## Информация о директории

Объекты файловой системы имеют [listdir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.listdir), подобный os.listdir; он принимает путь к каталогу и возвращает список имён файлов:

```
>>> home_fs.listdir('/projects')['fs', 'moya', 'README.md']
```

Альтернативный метод составления списков каталогов — [scandir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.scandir) возвращает _iterable_ объектов [ресурсной информации](https://docs.pyfilesystem.org/en/latest/info.html#info) (объекты информации ресурсов):

```
>>> directory = list(home_fs.scandir('/projects'))>>> directory[<dir 'fs'>, <dir 'moya'>, <file 'README.md'>]
```

Эти объекты имеют ряд преимуществ перед простым именем файла. Например, вы можете определить, ссылается ли объект информации на файл или на каталог через атрибут [is_dir](https://docs.pyfilesystem.org/en/latest/reference/info_objects.html#fs.info.Info.is_dir), без дополнительного системного вызова. Информационные объекты могут содержать размер, время изменения и т. д. если вы запросите её в параметре namespaces.

> Причина, по которой scandir возвращает `iterable`, а не список, заключается в том, что может оказаться эффективнее получать информацию о каталоге по частям, когда каталог очень большой или когда информация должна быть получена по сети.

Кроме того, объекты FS имеют метод [filterdir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.filterdir), который расширяет scandir возможностью фильтрации содержимого каталога по подстановочным символам. Вот так вы найдёте все файлы Python в каталоге:

```
>>> code_fs = OSFS('~/projects/src')>>> directory = list(code_fs.filterdir('/', files=['*.py']))
```

По умолчанию объекты информации о ресурсе, возвращаемые scandir и listdir, содержат только имя файла и флаг is_dir. Вы можете запросить дополнительную информацию с помощью параметра namespaces. Вот как можно запросить дополнительные сведения (например, размер файла и время его изменения):

```
>>> directory = code_fs.filterdir('/', files=['*.py'], namespaces=['details'])
```

Код добавит свойства size и modified (и другие) к объектам информации о ресурсе, благодаря чему сработает эта конструкция:

```
>>> sum(info.size for info in directory)
```

Дополнительную информацию смотрите в [этом](https://docs.pyfilesystem.org/en/latest/info.html#info) разделе.

## Поддиректории

PyFilesystem не имеет понятия _текущего рабочего каталога_, поэтому у объекта FS нет метода chdir. К счастью, вы не прогадаете, работать с подкаталогами в PyFilesystem несложно.

Вы всегда можете указать каталог с помощью методов, принимающих путь. Например, home_fs.listdir('/projects') получит список каталогов в projects. В качестве альтернативы можно вызвать [opendir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.opendir), которая возвращает новый объект FS для подкаталога. Вот так можно перечислить содержимое папки projects вашего домашнего каталога:

```
>>> home_fs = open_fs('~/')>>> projects_fs = home_fs.opendir('/projects')>>> projects_fs.listdir('/')['fs', 'moya', 'README.md']
```

Когда вы вызываете opendir, объект FS возвращает экземпляр [SubFS](https://docs.pyfilesystem.org/en/latest/reference/subfs.html#fs.subfs.SubFS). Если вы вызовете любой из методов на объекте SubFS, это будет выглядеть так, как будто вы вызвали тот же метод на родительской файловой системе, а путь указывался относительно подкаталога.

```
>>> home_fs = open_fs('~/')>>> game_fs = home_fs.makedirs('projects/game')>>> game_fs.touch('__init__.py')>>> game_fs.writetext('README.md', "Tetris clone")>>> game_fs.listdir('/')['__init__.py', 'README.md']
```

Работая с объектами SubFS, вы можете не писать много кода манипуляции путями, который подвержен ошибкам.

## Работа с файлами

Открыть файл из объекта FS можно с помощью [open()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.open), который очень похож на io.open в стандартной библиотеке. Вот как можно открыть файл под названием “reminder.txt” в домашнем каталоге:

```
>>> with open_fs('~/') as home_fs:...     with home_fs.open('reminder.txt') as reminder_file:...        print(reminder_file.read())buy coffee
```

В случае OSFS вернётся стандартный файлоподобный объект. Другие файловые системы могут возвращать другой объект, поддерживающий те же методы: [MemoryFS](https://docs.pyfilesystem.org/en/latest/reference/memoryfs.html#fs.memoryfs.MemoryFS) вернёт io.BytesIO.

PyFilesystem также предлагает ряд сокращений общих файловых операций. Например, [readbytes()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.readbytes) вернёт содержимое файла в виде байтов, а [readtext()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.readtext) прочитает текст Unicode. Эти методы обычно предпочтительнее явного открытия файлов: объект FS может иметь оптимизированную реализацию. Другие _краткие_ методы — [download()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.download), [upload()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.upload), [writebytes()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.writebytes) и [writetext()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.writetext).

## Обход файлов

Часто вам нужно просканировать файлы в заданном каталоге и всех подкаталогах — _обойти_ файловую систему. Вот так можно вывести пути ко всем файлам Python:

```
>>> from fs import open_fs>>> home_fs = open_fs('~/')>>> for path in home_fs.walk.files(filter=['*.py']):...     print(path)
```

Атрибут walk объекта FS — это экземпляр [BoundWalker](https://docs.pyfilesystem.org/en/latest/reference/walk.html#fs.walk.BoundWalker), который должен справиться с большинством требований к обходу директорий. Подробности — в разделе [Обход](https://docs.pyfilesystem.org/en/latest/walking.html#walking).

### Глоббинг

С обходом тесно связан _глоббинг_: способом сканирования файловых систем на уровне абстракции немного выше. Пути можно фильтровать по шаблону поиска, который похож на подстановочный знак (например, *.py), но может соответствовать нескольким уровням структуры каталогов. Вот пример, где из домашнего каталога удаляются файлы .pyc:

```
>>> from fs import open_fs>>> open_fs('~/project').glob('**/*.pyc').remove()62
```

Дополнительную информацию смотрите в разделе [Глоббинг](https://docs.pyfilesystem.org/en/latest/globbing.html#globbing).

## Перемещение и копирование

Перемещать и копировать содержимое файлов вы можете методами [move()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.move) и [copy()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.copy), методы директорий — [movedir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.movedir) и [copydir()](https://docs.pyfilesystem.org/en/latest/reference/base.html#fs.base.FS.copydir) соответственно.

Эти методы оптимизированы по возможности, и в зависимости от реализации они производительнее, чем чтение и запись файлов.

Чтобы копировать и перемещать файлы _между_ файловыми системами (а не внутри одной системы), применяйте модули [move](https://docs.pyfilesystem.org/en/latest/reference/move.html#module-fs.move) и [copy](https://docs.pyfilesystem.org/en/latest/reference/copy.html#module-fs.copy). Эти методы принимают как объекты FS, так и FS URLS. Например, следующая команда сожмёт содержимое папки projects:

```
>>> from fs.copy import copy_fs>>> copy_fs('~/projects', 'zip://projects.zip')
```

Что эквивалентно такому, подробному, коду:

```
>>> from fs.copy import copy_fs>>> from fs.osfs import OSFS>>> from fs.zipfs import ZipFS>>> copy_fs(OSFS('~/projects'), ZipFS('projects.zip'))
```

Функции [copy_fs()](https://docs.pyfilesystem.org/en/latest/reference/copy.html#fs.copy.copy_fs) и [copy_dir()](https://docs.pyfilesystem.org/en/latest/reference/copy.html#fs.copy.copy_dir) также принимают параметр [Walker](https://docs.pyfilesystem.org/en/latest/reference/walk.html#fs.walk.Walker), этим можно воспользоваться для фильтрации копируемых файлов. Если вам нужно создать резервную копию только файлов Python, вы можете написать:

```
>>> from fs.copy import copy_fs>>> from fs.walk import Walker>>> copy_fs('~/projects', 'zip://projects.zip', walker=Walker(filter=['*.py']))
```

Альтернатива копированию — _зеркальное отображение_, копирующее файловую систему и поддерживающее её в актуальном состоянии, в дальнейшем копируя только изменённые файлы и каталоги. Смотрите [mirror()](https://docs.pyfilesystem.org/en/latest/reference/mirror.html#fs.mirror.mirror).

[Код](https://github.com/PyFilesystem/pyfilesystem2) на Github.

На сегодня всё. Попробовать пакет в действии вы сможете [на нашем курсе](https://skillfactory.ru/python-fullstack-web-developer?utm_source=habr&utm_medium=habr&utm_campaign=article&utm_content=coding_fpw_140921&utm_term=conc), а также вы можете перейти на страницы [других курсов](https://skillfactory.ru/catalogue?utm_source=habr&utm_medium=habr&utm_campaign=article&utm_content=sf_allcourses_140921&utm_term=conc), чтобы увидеть, как мы готовим специалистов в других направлениях: