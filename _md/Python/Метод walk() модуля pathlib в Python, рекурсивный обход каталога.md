2024-09,03
#python 
[Метод walk() модуля pathlib в Python, рекурсивный обход каталога (docs-python.ru)](https://docs-python.ru/standart-library/modul-pathlib-python/metod-walk/)

## Рекурсивный обход дерева каталогов либо сверху вниз, либо снизу вверх

#### Синтаксис:

import Path

# Новое в Python 3.12.
dirpath, dirnames, filenames = Path.walk(top_down=True, on_error=None, follow_symlinks=False)

#### Параметры:

- `top_down=True` - определяет направление перебора дерева каталогов либо сверху вниз, либо снизу вверх;
- `on_error=None` - вызываемый объект, принимающий экземпляр `OSError`;
- `follow_symlinks=False` - переходить ли по символическим ссылкам.

#### Возвращаемое значение:

- [кортеж](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-tuple-kortezh/ "Кортеж tuple в Python.") из трех элементов `(dirpath, dirnames, filenames)`

#### Описание:

Метод [`Path.walk()`](https://docs-python.ru/standart-library/modul-pathlib-python/metod-walk/ "Метод walk() модуля pathlib в Python, рекурсивный обход каталога") модуля [`pathlib`](https://docs-python.ru/standart-library/modul-pathlib-python/ "Модуль pathlib в Python, операции с путями ОС.") генерирует имена файлов в дереве каталогов, перемещаясь по дереву либо сверху вниз, либо снизу вверх.

Для каждого каталога, корнем которого является `self` (включая `self`, но исключая `‘.’` и `‘..’`) метод возвращает [кортеж](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-tuple-kortezh/ "Кортеж tuple в Python.") из трех элементов `(dirpath, dirnames, filenames)`:

- `dirpath` - это путь к каталогу, который в данный момент просматривается,
- `dirnames` - это список строк для имен подкаталогов в `dirpath` (исключая '.' и '..'),
- `filenames` - это список строк для имен файлов, не относящихся к каталогу, в `dirpath`.

Чтобы получить полный путь (который начинается с `self`) к файлу или каталогу в `dirpath`, нужно выполнить команду `dirpath / name`. Сортируются списки или нет, зависит от файловой системы.

Если **необязательный аргумент `top_down`** имеет значение `True` (по умолчанию), то кортежи `(dirpath, dirnames, filenames)` для каталога генерируется раньше кортежей для любого из его подкаталогов (т.е. каталоги просматриваются сверху вниз). Если `top_down` имеет значение `False`, то кортежи `(dirpath, dirnames, filenames)` для каталога генерируется после кортежей для всех его подкаталогов (т.е. каталоги просматриваются снизу вверх). Независимо от значения `top_down`, список подкаталогов извлекается до того, как будут пройдены кортежи каталога и его подкаталогов.

Когда `top_down` имеет значение `True`, вызывающая сторона может изменять список имен каталогов `dirnames` на месте (например, используя [del](https://docs-python.ru/tutorial/instruktsija-del-python/ "Инструкция del в Python") или [срез](https://docs-python.ru/tutorial/obschie-operatsii-posledovatelnostjami-list-tuple-str-python/izvlechenie-sreza-sequence-posledovatelnosti/ "Получение среза sequence[i:j] в Python.")), а [`Path.walk()`](https://docs-python.ru/standart-library/modul-pathlib-python/metod-walk/ "Метод walk() модуля pathlib в Python, рекурсивный обход каталога") будет рекурсивно обращаться только к подкаталогам, имена которых остаются в `dirnames`. Такое поведение можно использовать для сокращения поиска, или для установления определенного порядка посещения, или даже для информирования `Path.walk()` о каталогах, которые вызывающий объект создает или переименовывает, прежде чем метод снова возобновит обход. Изменение `dirnames`, когда `top_down=False` не влияет на поведение `Path.walk()`, т. к. каталоги в `dirnames` уже созданы к моменту, когда `dirnames` передаются вызывающему объекту.

По умолчанию ошибки [`os.scandir()`](https://docs-python.ru/standart-library/modul-os-python/funktsija-scandir-modulja-os/ "Функция scandir() модуля os в Python.") игнорируются. Если указан **необязательный аргумент `on_error`**, то он должен быть вызываемым объектом, принимающим один аргумент - экземпляр [`OSError`](https://docs-python.ru/tutorial/vstroennye-iskljuchenija-interpretator-python/oshibki-operatsionnoj-sistemy-oserror/ "Исключения операционной системы: OSError в Python."). Этот вызываемый объект может обработать ошибку, чтобы продолжить обход, или повторно поднять исключение, чтобы остановить обход. Обратите внимание, что имя файла доступно как атрибут `filename` объекта `exception`.

В примере ниже считается количество байт, занятое файлами в каждом каталоге, при этом игнорируются каталоги `__pycache__`:

from pathlib import Path

concurrent = Path("cpython/Lib/concurrent")

for root, dirs, files in concurrent.walk(on_error=print):
  print(
      root,
      "содержит",
      sum((root / file).stat().st_size for file in files),
      "байт в",
      len(files),
      "файлах, не относящиеся к каталогу"
  )

  if '__pycache__' in dirs:
        dirs.remove('__pycache__')

По умолчанию `Path.walk()` не следует по символическим ссылкам, а только добавляет их в список `filenames`. Чтобы разрешать символические ссылки и размещать их в `dirnames` и `filenames` в соответствии с их целями и, следовательно, посещать каталоги, на которые указывают символические ссылки (там, где это поддерживается), необходимо установить `follow_symlinks=True`.

> **Важно!**. Нужно понимать, что установка `follow_symlinks=True` может привести к бесконечной рекурсии, если ссылка указывает на собственный родительский каталог. Метод `Path.walk()` не отслеживает каталоги, которые он уже посетил.
> 
> Примечание. Метод `Path.walk()` предполагает, что каталоги, по которым он проходит, не изменяются во время выполнения. Например, если каталог из `dirnames` был заменен символической ссылкой, а `follow_symlinks=False`, то `Path.walk()` все равно попытается спуститься в него. Чтобы предотвратить такое поведение, нужно удалить измененные каталоги из `dirnames`, если это необходимо.
> 
> Примечание. В отличие от [`os.walk()`](https://docs-python.ru/standart-library/modul-os-python/funktsija-walk-modulja-os/ "Функция walk() модуля os в Python."), Path.walk() перечисляет символические ссылки на каталоги в `filenames`, если значение `follow_symlinks=False`.

## Пример использования `Path.walk()`.

Пример представляет собой простую реализацию [`shutil.rmtree()`](https://docs-python.ru/standart-library/modul-shutil-python/funktsija-rmtree-modulja-shutil/ "Функция rmtree() модуля shutil в Python."). Обход дерева снизу вверх очень важен, поскольку [`Path.rmdir()`](https://docs-python.ru/standart-library/modul-pathlib-python/sozdanie-udalenie-fajla-kataloga-ssylki/ "Создание/удаление файла/каталога или ссылки средствами pathlib.") не позволяет удалить каталог, пока он не станет пустым.

> **ОСТОРОЖНО**: Это опасно! Например, если `delete == Path('/')`, то запуск примера может привести к удалению всех файлов.

from pathlib import Path

# удаляет все, что доступно из каталога 'delete_dir'.
delete = Path('delete_dir')
for root, dirs, files in delete.walk(top_down=False):
    for name in files:
        (root / name).unlink()
    for name in dirs:
        (root / name).rmdir()

---
