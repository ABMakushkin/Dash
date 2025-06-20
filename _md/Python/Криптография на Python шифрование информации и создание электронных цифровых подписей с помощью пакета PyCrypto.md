2024-09-24
#python 
[Криптография на Python: шифрование информации и создание электронных цифровых подписей с помощью пакета PyCrypto / Хабр (habr.com)](https://habr.com/ru/articles/265309/)

Долго мучился с [PyCrypto](https://www.dlitz.net/software/pycrypto/api/current/), в итоге получилась эта статья и полная реализация следующего [протокола](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B8%D0%B9_%D0%BF%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB):  
  
Этап отправки:  
  
1. Алиса подписывает сообщение своей [цифровой подписью](https://ru.wikipedia.org/wiki/%D0%AD%D0%BB%D0%B5%D0%BA%D1%82%D1%80%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%B4%D0%BF%D0%B8%D1%81%D1%8C) и шифрует ее [открытым ключом](https://ru.wikipedia.org/wiki/%D0%9A%D0%BB%D1%8E%D1%87_(%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%8F)) Боба ([асимметричным алгоритмом](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0_%D1%81_%D0%BE%D1%82%D0%BA%D1%80%D1%8B%D1%82%D1%8B%D0%BC_%D0%BA%D0%BB%D1%8E%D1%87%D0%BE%D0%BC)).  
2. Алиса генерирует случайный сеансовый ключ и шифрует этим ключом сообщение (с помощью [симметричного алгоритма](https://ru.wikipedia.org/wiki/%D0%A1%D0%B8%D0%BC%D0%BC%D0%B5%D1%82%D1%80%D0%B8%D1%87%D0%BD%D1%8B%D0%B5_%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D1%8B)).  
3. Сеансовый ключ шифруется открытым ключом Боба (асимметричным алгоритмом).  
Алиса посылает Бобу зашифрованное сообщение, подпись и зашифрованный сеансовый ключ.  
  
Этап приёма:  
  
Боб получает зашифрованное сообщение Алисы, подпись и зашифрованный сеансовый ключ.  
4. Боб расшифровывает сеансовый ключ своим закрытым ключом.  
5. При помощи полученного, таким образом, сеансового ключа Боб расшифровывает зашифрованное сообщение Алисы.  
6. Боб расшифровывает и проверяет подпись Алисы.  
  
Вышеописанный протокол является [гибридной системой шифрования](https://ru.wikipedia.org/wiki/%D0%93%D0%B8%D0%B1%D1%80%D0%B8%D0%B4%D0%BD%D0%B0%D1%8F_%D0%BA%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0), которая работают следующим образом. Для симметричного алгоритма [AES](https://ru.wikipedia.org/wiki/Advanced_Encryption_Standard) (или любого другого) генерируется случайный сеансовый ключ.  
  
Такой ключ как правило имеет размер от 128 до 512 бит (в зависимости от алгоритма). Затем используется симметричный алгоритм для шифрования сообщения. В случае [блочного шифрования](https://ru.wikipedia.org/wiki/%D0%91%D0%BB%D0%BE%D1%87%D0%BD%D1%8B%D0%B9_%D1%88%D0%B8%D1%84%D1%80) необходимо использовать [режим шифрования](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B6%D0%B8%D0%BC_%D1%88%D0%B8%D1%84%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F) (например [CBC](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B6%D0%B8%D0%BC_%D1%81%D1%86%D0%B5%D0%BF%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F_%D0%B1%D0%BB%D0%BE%D0%BA%D0%BE%D0%B2_%D1%88%D0%B8%D1%84%D1%80%D0%BE%D1%82%D0%B5%D0%BA%D1%81%D1%82%D0%B0)), что позволит шифровать сообщение с длиной, превышающей длину блока. Что касается самого случайного ключа, он должен быть зашифрован с помощью открытого ключа получателя сообщения, и именно на этом этапе применяется криптосистема с открытым ключом [RSA](https://ru.wikipedia.org/wiki/RSA).  
  
Поскольку сеансовый ключ короткий, его шифрование занимает немного времени. Шифрование набора сообщений с помощью асимметричного алгоритма — это задача вычислительно более сложная, поэтому здесь предпочтительнее использовать симметричное шифрование. Затем достаточно отправить сообщение, зашифрованное симметричным алгоритмом, а также соответствующий ключ в зашифрованном виде. Получатель сначала расшифровывает ключ с помощью своего секретного ключа, а затем с помощью полученного ключа получает и всё сообщение.  
  
Начнем с генерации пары ключей для Алисы и Боба.  
  

```
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA

# key generation Alisa
privatekey = RSA.generate(2048)
f = open('c:\cipher\\alisaprivatekey.txt','wb')
f.write(bytes(privatekey.exportKey('PEM'))); f.close()
publickey = privatekey.publickey()
f = open('c:\cipher\\alisapublickey.txt','wb')
f.write(bytes(publickey.exportKey('PEM'))); f.close()
# key generation Bob
privatekey = RSA.generate(2048)
f = open('c:\cipher\\bobprivatekey.txt','wb')
f.write(bytes(privatekey.exportKey('PEM'))); f.close()
publickey = privatekey.publickey()
f = open('c:\cipher\\bobpublickey.txt','wb')
f.write(bytes(publickey.exportKey('PEM'))); f.close()
```

  
  
На данный момент система шифрования на основе RSA считается надёжной, начиная с размера ключа в 2048 бит.  
  
В системе RSA можно создавать сообщения, которые будут и зашифрованы, и содержать цифровую подпись. Для этого автор сначала должен добавить к сообщению свою цифровую подпись, а затем — зашифровать получившуюся в результате пару (состоящую из самого сообщения и подписи к нему) с помощью открытого ключа, принадлежащего получателю. Получатель расшифровывает полученное сообщение с помощью своего секретного ключа и проверяет подпись автора с помощью его открытого ключа.  
  
Использование односторонней [криптографической хеш-функции](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B8%D0%BF%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D1%84%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D1%85%D0%B5%D1%88-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F) позволяет оптимизировать вышеописанный алгоритм цифровой подписи. Производится шифрование не самого сообщения, а значения хеш-функции, взятой от сообщения. Данный метод обеспечивает следующие преимущества:  
  
1. Понижение вычислительной сложности. Как правило, документ значительно больше его хеша.  
2. Повышение криптостойкости. Криптоаналитик не может, используя открытый ключ, подобрать подпись под сообщение, а только под его хеш.  
3. Обеспечение совместимости. Большинство алгоритмов оперирует со строками бит данных, но некоторые используют другие представления. Хеш-функцию можно использовать для преобразования произвольного входного текста в подходящий формат.  
  
Хэш-функции, представляют собой функции, математические или иные, которые получают на вход строку переменной длинны (называемую прообразом) и преобразуют ее в строку фиксированной, обычно меньшей, длинны (называемую значением хэш-функции). Смысл хэш-функции состоит в получении характерного признака прообраза — значения, по которому анализируются различные прообразы при решении обратной задачи. Однонаправленная хэш-функция — это хэш-функция, которая работает только в одном направлении: легко вычислить значение хэш-функции по прообразу, но трудно создать прообраз, значение хэш-функции которого равно заданной величине. Хэш-функция является открытой, тайны ее расчета не существует. Безопасность однонаправленной хэш-функции заключается именно в ее однонаправленности.  
  
Переходим к написанию первого пункта протокола:  
  
1. Алиса подписывает сообщение своей цифровой подписью и шифрует ее открытым ключом Боба (асимметричным алгоритмом RSA).  
  

```
from Crypto.Signature import PKCS1_v1_5
from Crypto.Hash import SHA
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

# creation of signature
f = open('c:\cipher\plaintext.txt','rb')
plaintext = f.read(); f.close()
privatekey = RSA.importKey(open('c:\cipher\\alisaprivatekey.txt','rb').read())
myhash = SHA.new(plaintext)
signature = PKCS1_v1_5.new(privatekey)
signature = signature.sign(myhash)
# signature encrypt
publickey = RSA.importKey(open('c:\cipher\\bobpublickey.txt','rb').read())
cipherrsa = PKCS1_OAEP.new(publickey)
sig = cipherrsa.encrypt(signature[:128])
sig = sig + cipherrsa.encrypt(signature[128:])
f = open('c:\cipher\signature.txt','wb')
f.write(bytes(sig)); f.close()
```

  
  
В следующем листинге код для следующих двух пунктов протокола:  
  
2. Алиса генерирует случайный сеансовый ключ и шифрует этим ключом сообщение (с помощью симметричного алгоритма AES).  
3. Сеансовый ключ шифруется открытым ключом Боба (асимметричным алгоритмом RSA).  
  

```
from Crypto.Cipher import AES
from Crypto import Random
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA

# creation 256 bit session key 
sessionkey = Random.new().read(32) # 256 bit
# encryption AES of the message
f = open('c:\cipher\plaintext.txt','rb')
plaintext = f.read(); f.close()
iv = Random.new().read(16) # 128 bit
obj = AES.new(sessionkey, AES.MODE_CFB, iv)
ciphertext = iv + obj.encrypt(plaintext)
f = open('c:\cipher\plaintext.txt','wb')
f.write(bytes(ciphertext)); f.close()
# encryption RSA of the session key
publickey = RSA.importKey(open('c:\cipher\\bobpublickey.txt','rb').read())
cipherrsa = PKCS1_OAEP.new(publickey)
sessionkey = cipherrsa.encrypt(sessionkey)
f = open('c:\cipher\sessionkey.txt','wb')
f.write(bytes(sessionkey)); f.close()
```

  
  
Для режима шифрования [CFB](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B6%D0%B8%D0%BC_%D0%BE%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%BE%D0%B9_%D1%81%D0%B2%D1%8F%D0%B7%D0%B8_%D0%BF%D0%BE_%D1%88%D0%B8%D1%84%D1%80%D0%BE%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83) требуется [вектор инициализации (IV)](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%B6%D0%B8%D0%BC_%D1%88%D0%B8%D1%84%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F#Initialization_vector_.28IV.29) (переменная iv).  
  
В криптографии, вектор инициализации (IV) представляет собой некоторое число, как правило оно должно быть случайным или псевдослучайным. Случайность имеет решающее значение для достижения семантической безопасности, которая при повторном использовании схемы под тем же ключом не позволит злоумышленнику вывести отношения между сегментами зашифрованных сообщений. Вектор инициализации не шифруется и сохраняется перед зашифрованным сообщением.  
  
Далее Алиса посылает Бобу зашифрованное сообщение, подпись и зашифрованный сеансовый ключ.  
  
Боб получает зашифрованное сообщение Алисы, подпись и зашифрованный сеансовый ключ.  
  
4. Боб расшифровывает сеансовый ключ своим закрытым ключом.  
5. При помощи полученного, таким образом, сеансового ключа Боб расшифровывает зашифрованное сообщение Алисы.  
  

```
from Crypto.Cipher import AES
from Crypto import Random
from Crypto.Cipher import PKCS1_OAEP
from Crypto.PublicKey import RSA

# decryption session key
privatekey = RSA.importKey(open('c:\cipher\\bobprivatekey.txt','rb').read())
cipherrsa = PKCS1_OAEP.new(privatekey)
f = open('c:\cipher\sessionkey.txt','rb')
sessionkey = f.read(); f.close()
sessionkey = cipherrsa.decrypt(sessionkey)
# decryption message
f = open('c:\cipher\plaintext.txt','rb')
ciphertext = f.read(); f.close()
iv = ciphertext[:16]
obj = AES.new(sessionkey, AES.MODE_CFB, iv)
plaintext = obj.decrypt(ciphertext)
plaintext = plaintext[16:]
f = open('c:\cipher\plaintext.txt','wb')
f.write(bytes(plaintext)); f.close()
```

  
  
Последний этап:  
  
6. Боб расшифровывает и проверяет подпись Алисы.  
  

```
from Crypto.Signature import PKCS1_v1_5
from Crypto.Hash import SHA
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

# decryption signature
f = open('c:\cipher\signature.txt','rb')
signature = f.read(); f.close()
privatekey = RSA.importKey(open('c:\cipher\\bobprivatekey.txt','rb').read())
cipherrsa = PKCS1_OAEP.new(privatekey)
sig = cipherrsa.decrypt(signature[:256])
sig = sig + cipherrsa.decrypt(signature[256:])
# signature verification
f = open('c:\cipher\plaintext.txt','rb')
plaintext = f.read(); f.close()
publickey = RSA.importKey(open('c:\cipher\\alisapublickey.txt','rb').read())
myhash = SHA.new(plaintext)
signature = PKCS1_v1_5.new(publickey)
test = signature.verify(myhash, sig)
```

  
  
Безопасность RSA основана на сложности [задачи факторизации](https://ru.wikipedia.org/wiki/%D0%A4%D0%B0%D0%BA%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F) произведения двух больших простых чисел. [Факторизация целых чисел](https://ru.wikipedia.org/wiki/%D0%A4%D0%B0%D0%BA%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F_%D1%86%D0%B5%D0%BB%D1%8B%D1%85_%D1%87%D0%B8%D1%81%D0%B5%D0%BB) для больших чисел является задачей большой сложности. Не существует никакого известного способа, чтобы решить эту задачу быстро. Факторизация кандидат в [односторонние функции](https://ru.wikipedia.org/wiki/%D0%9E%D0%B4%D0%BD%D0%BE%D1%81%D1%82%D0%BE%D1%80%D0%BE%D0%BD%D0%BD%D1%8F%D1%8F_%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D1%8F), которые относительно легко вычисляются, но инвертируются с большим трудом. То есть, зная x просто рассчитать f(x), но по известному f(x) нелегко вычислить x. Здесь, «нелегко» означает, что для вычисления x по f(x) могут потребоваться миллионы лет, даже если над этой проблемой будут биться все компьютеры мира.  
  
Литература:  
Брюс Шнайер — Прикладная криптография