# Перевод текста
Мы можем прочитать содержимое ответа сервера. Взгляните еще раз на временную шкалу GitHub:

import requests

r = requests.get('https://api.github.com/events')
r.text
'[{"repository":{"open_issues":0,"url":"https://github.com/...

Запросы автоматически декодируют контент с сервера. Большинство кодировок Юникода легко декодируются.

Когда вы делаете запрос, Requests делает обоснованные предположения о кодировке ответа на основе заголовков HTTP. Кодировка текста, выбранная Requests, используется при доступе к r.text. Вы можете узнать, какую кодировку использует Requests, и изменить ее, используя свойство r.encoding:

r.encoding
'utf-8'
r.encoding = 'ISO-8859-1'

Если вы измените кодировку, Requests будет использовать новое значение r.encoding при каждом вызове r.text. Возможно, вам захочется сделать это в любой ситуации, когда вы можете применить специальную логику для определения того, какой будет кодировка контента. Например, HTML и XML имеют возможность указывать кодировку в своем теле. В подобных ситуациях вам следует использовать r.content, чтобы найти кодировку, а затем установить r.encoding. Это позволит вам использовать r.text с правильной кодировкой.

Запросы также будут использовать пользовательские кодировки, если они вам понадобятся. Если вы создали свою собственную кодировку и зарегистрировали ее в модуле кодеков, вы можете просто использовать имя кодека в качестве значения r.encoding, и Requests выполнит декодирование за вас.





# Аутентификация - Authentication
В этом документе обсуждается использование различных видов аутентификации с запросами.

Многие веб-сервисы требуют аутентификации, и существует множество различных типов. Ниже мы описываем различные формы аутентификации, доступные в Requests, от простых до сложных.

# Базовая аутентификация - Basic Authentication
Многие веб-сервисы, требующие аутентификации, принимают HTTP Basic Auth. Это самый простой вид, и Requests поддерживает его прямо из "коробки".

Выполнять запросы с помощью HTTP Basic Auth очень просто:

from requests.auth import HTTPBasicAuth basic = HTTPBasicAuth('user', 'pass') requests.get('https://httpbin.org/basic-auth/user/pass', auth=basic) <Response [200]>

На самом деле, HTTP Basic Auth настолько распространен, что запросы предоставляют удобное сокращение для его использования:

requests.get('https://httpbin.org/basic-auth/user/pass', auth=('user', 'pass')) <Response [200]>

Предоставление учетных данных в кортеже, подобном этому, как в HTTP Basic Auth примере выше.

# Аутентификационная сеть - netrc Anuthentication
Если метод аутентификации не указан с auth аргументом, запрос попытается получить учетные данные аутентификации для имени хоста URL из файла сети пользователя. Файл netrc переопределяет необработанные заголовки аутентификации HTTP, установленные с помощью headers.

Если учетные данные для имени хоста найдены, запрос отправляется с помощью HTTP Basic Auth.

# Перевод текста
Мы можем прочитать содержимое ответа сервера. Взгляните еще раз на временную шкалу GitHub:

import requests

r = requests.get('https://api.github.com/events')
r.text
'[{"repository":{"open_issues":0,"url":"https://github.com/...

Запросы автоматически декодируют контент с сервера. Большинство кодировок Юникода легко декодируются.

Когда вы делаете запрос, Requests делает обоснованные предположения о кодировке ответа на основе заголовков HTTP. Кодировка текста, выбранная Requests, используется при доступе к r.text. Вы можете узнать, какую кодировку использует Requests, и изменить ее, используя свойство r.encoding:

r.encoding
'utf-8'
r.encoding = 'ISO-8859-1'

Если вы измените кодировку, Requests будет использовать новое значение r.encoding при каждом вызове r.text. Возможно, вам захочется сделать это в любой ситуации, когда вы можете применить специальную логику для определения того, какой будет кодировка контента. Например, HTML и XML имеют возможность указывать кодировку в своем теле. В подобных ситуациях вам следует использовать r.content, чтобы найти кодировку, а затем установить r.encoding. Это позволит вам использовать r.text с правильной кодировкой.

Запросы также будут использовать пользовательские кодировки, если они вам понадобятся. Если вы создали свою собственную кодировку и зарегистрировали ее в модуле кодеков, вы можете просто использовать имя кодека в качестве значения r.encoding, и Requests выполнит декодирование за вас.

CUSTOM HEADERS

Если вы хотите добавить заголовки http к запросу, то просто передайте словарь в параметр headers. Например, в предыдущем примере мы не указали наш user-agent: url = 'https://api.github.com/some/endpoint' headers = {'user-agent': 'my-app/0.0.1'}

r = requests.get(url, headers=headers)

Примечание: пользовательские заголовки имеют меньший приоритет, чем более конкретные источники информации. Например:

- Заголовоки авторизации, установленные с помощью headers =, будут переопределены, если учетные данные указаны в .netrc, что, в свою очередь, будет переопределено параметром auth=. Запросы будут искать файл netrc в /netrc,/_netrc или по пути, указанному переменной среды NETRC.
- Заголовки авторизации будут удалены, если вы будете перенаправлены за пределы хоста.
- Заголовки Proxy-Authorization будут переопределены учетными данными прокси-сервера, указанными в URL-адресе.
- Заголовки Content-Length будут переопределены, когда мы сможем определить длину контента
Более того, запросы вообще не меняют свое поведение в зависимости от того, какие пользовательские заголовки указаны. Заголовки просто передаются в конечный запрос.

Примечание: все значения заголовков должны быть строкой, байт-тестированием или юникодом. Пока это разрешено, рекомендуется избегать передачи значений заголовков юникода
*Ксюша*


# **Binary-Reponse-Content** 
# Содержание бинарного ответа 
 
**Вы также можете получить доступ к телу ответа в виде байтов для нетекстовых запросов:**

r.content
b'[{"repository":{"open_issues":0,"url":"https://github.com/...

**Кодировки gzip и deflate передачи автоматически декодируются для вас.**
**Кодировка передачи br автоматически декодируется, если установлена ​​библиотека Brotli, например brotli или brotlicffi.**
**Например, чтобы создать изображение из двоичных данных, возвращаемых запросом, можно использовать следующий код:**

from PIL import Image
from io import BytesIO

i = Image.open(BytesIO(r.content))
