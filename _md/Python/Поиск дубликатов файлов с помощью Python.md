2024-11-18
#python 
[Finding Duplicate Files with Python - GeeksforGeeks](https://translated.turbopages.org/proxy_u/en-ru.ru.9123c1ee-673b33a4-a0257249-74722d776562/https/www.geeksforgeeks.org/finding-duplicate-files-with-python/)

В этой статье мы закодируем скрипт на Python для поиска дубликатов файлов в файловой системе или внутри определенной папки.

## **Метод 1: использование Filecmp**

Модуль python filecmp предлагает функции для сравнения каталогов и файлов. Функция cmp сравнивает файлы и возвращает True, если они кажутся идентичными, в противном случае False.

**Syntax:** filecmp.cmp(f1, f2, shallow)

**Параметры:**

- **f1:** имя одного файла
- **f2:** имя другого файла для сравнения
- **поверхностно:** С помощью этого мы устанавливаем, хотим ли мы сравнивать содержимое или нет.

**Примечание:** Значение по умолчанию равно True, что гарантирует, что сравниваются только сигнатуры файлов, а не содержимое.

**Тип возвращаемого значения:** Логическое значение (True, если файлы одинаковые, в противном случае False)

**Пример:**

Мы предполагаем здесь для примера, что “text_1.txt“, "text_3.txt”, “text_4.txt” - это файлы с одинаковым содержимым, а “text_2.txt“, "text_5.txt" - это файлы с одинаковым содержимым.

- Python3

|   |
|---|
|`# Importing Libraries`<br><br>`import` `os`<br><br>`from` `pathlib` `import` `Path`<br><br>`from` `filecmp` `import` `cmp`<br><br>`# list of all documents`<br><br>`DATA_DIR` `=` `Path(``'/path/to/directory'``)`<br><br>`files` `=` `sorted``(os.listdir(DATA_DIR))`<br><br>`# List having the classes of documents`<br><br>`# with the same content`<br><br>`duplicateFiles` `=` `[]`<br><br>`# comparison of the documents`<br><br>`for` `file_x` `in` `files:`<br><br>    `if_dupl` `=` `False`<br><br>    `for` `class_` `in` `duplicateFiles:`<br><br>        `# Comparing files having same content using cmp()`<br><br>        `# class_[0] represents a class having same content`<br><br>        `if_dupl` `=` `cmp``(`<br><br>            `DATA_DIR` `/` `file_x,`<br><br>            `DATA_DIR` `/` `class_``[``0``],`<br><br>            `shallow``=``False`<br><br>        `)`<br><br>        `if` `if_dupl:`<br><br>            `class_``.append(file_x)`<br><br>            `break`<br><br>    `if` `not` `if_dupl:`<br><br>        `duplicateFiles.append([file_x])`<br><br>`# Print results`<br><br>`print``(duplicateFiles)`|

**Вывод:**

![](https://media.geeksforgeeks.org/wp-content/uploads/20211014054105/Screenshot111.png)

## **Метод 2: использование хэширования и словаря**

Для начала этот скрипт получит отдельную папку или список папок, затем, пройдя по папке, он найдет дубликаты файлов. Далее этот скрипт вычислит хэш для каждого файла, присутствующего в папке, независимо от их названия, и сохранит их в виде словаря, где хэш является ключом, а путь к файлу - значением.

- Мы должны импортировать библиотеки os, sys, hashlib.
- Затем скрипт выполняет итерацию по файлам и вызывает функцию FindDuplicate() для поиска дубликатов.

**Syntax:** FindDuplicate(Path)
**Parameter:** 
**Path:** Path to folder having files
**Return Type:** Dictionary

- Функция FindDuplicate() принимает путь к файлу и вызывает функцию Hash_File()
- Затем функция Hash_File() используется для возврата шестнадцатеричного значения этого файла. Для получения дополнительной информации о шестнадцатеричном значении [читайте здесь](https://translated.turbopages.org/proxy_u/en-ru.ru.9123c1ee-673b33a4-a0257249-74722d776562/https/www.geeksforgeeks.org/md5-hash-python/).

**Syntax:** Hash_File(path)
**Parameters:** 
**path:** Path of file
**Return Type:** HEXdigest of file

- Затем этот MD5-хэш добавляется в словарь в качестве ключа с указанием пути к файлу в качестве его значения. После этого функция FindDuplicate() возвращает словарь, в котором ключи имеют несколько значений.т.е. дубликаты файлов.
- Теперь вызывается функция Join_Dictionary(), которая объединяет словарь, возвращенный FindDuplicate(), и пустой словарь.

**Syntax:** Join_Dictionary(dict1,dict2)
**Parameters:** 
**dict1, dict2:** Two different dictionaries
**Return Type:** Dictionary

- После этого мы распечатываем список файлов с одинаковым содержимым, используя результаты.

**Пример:**

Мы предполагаем здесь для примера, что “text_1.txt“, "text_3.txt”, “text_4.txt” - это файлы с одинаковым содержимым, а “text_2.txt“, "text_5.txt" - это файлы с одинаковым содержимым.

- Python3

|   |
|---|
|`# Importing Libraries`<br><br>`import` `os`<br><br>`import` `sys`<br><br>`from` `pathlib` `import` `Path`<br><br>`import` `hashlib`<br><br>`def` `FindDuplicate(SupFolder):`<br><br>    `# Duplic is in format {hash:[names]}`<br><br>    `Duplic` `=` `{}`<br><br>    `for` `file_name` `in` `files:`<br><br>        `# Path to the file`<br><br>        `path` `=` `os.path.join(folders, file_name)`<br><br>        `# Calculate hash`<br><br>        `file_hash` `=` `Hash_File(path)`<br><br>        `# Add or append the file path to Duplic`<br><br>        `if` `file_hash` `in` `Duplic:`<br><br>            `Duplic[file_hash].append(file_name)`<br><br>        `else``:`<br><br>            `Duplic[file_hash]` `=` `[file_name]`<br><br>    `return` `Duplic`<br><br>`# Joins dictionaries`<br><br>`def` `Join_Dictionary(dict_1, dict_2):`<br><br>    `for` `key` `in` `dict_2.keys():`<br><br>        `# Checks for existing key`<br><br>        `if` `key` `in` `dict_1:`<br><br>            `# If present Append`<br><br>            `dict_1[key]` `=` `dict_1[key]` `+` `dict_2[key]`<br><br>        `else``:`<br><br>            `# Otherwise Stores`<br><br>            `dict_1[key]` `=` `dict_2[key]`<br><br>`# Calculates MD5 hash of file`<br><br>`# Returns HEX digest of file`<br><br>`def` `Hash_File(path):`<br><br>    `# Opening file in afile`<br><br>    `afile` `=` `open``(path,` `'rb'``)`<br><br>    `hasher` `=` `hashlib.md5()`<br><br>    `blocksize``=``65536`<br><br>    `buf` `=` `afile.read(blocksize)`<br><br>    `while` `len``(buf) >` `0``:`<br><br>        `hasher.update(buf)`<br><br>        `buf` `=` `afile.read(blocksize)`<br><br>    `afile.close()`<br><br>    `return` `hasher.hexdigest()`<br><br>`Duplic` `=` `{}`<br><br>`folders` `=` `Path(``'path/to/directory'``)`<br><br>`files` `=` `sorted``(os.listdir(folders))`<br><br>`for` `i` `in` `files:`<br><br>    `# Iterate over the files`<br><br>    `# Find the duplicated files`<br><br>    `# Append them to the Duplic`<br><br>    `Join_Dictionary(Duplic, FindDuplicate(i))`<br><br>`# Results store a list of Duplic values`<br><br>`results` `=` `list``(``filter``(``lambda` `x:` `len``(x) >` `1``, Duplic.values()))`<br><br>`if` `len``(results) >` `0``:`<br><br>    `for` `result` `in` `results:`<br><br>        `for` `sub_result` `in` `result:`<br><br>            `print``(``'\t\t%s'` `%` `sub_result)`<br><br>`else``:`<br><br>    `print``(``'No duplicates found.'``)`|

**Вывод:**

![](https://media.geeksforgeeks.org/wp-content/uploads/20211014054105/Screenshot111.png)