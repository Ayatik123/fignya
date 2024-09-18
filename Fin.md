# Quickstart

Хотите начать? На этой странице представлено хорошее введение в то, как начать работу с запросами.

Во-первых, убедитесь, что:

+ Запросы установлены
+ Запросы актуальны

Начнем с простых примеров _**Сеня**_

# Сделать запрос

Сделать запрос с помощью Requests очень просто.

Начните с импорта модуля Requests: import requests

```
>>>import requests
```

Теперь попробуем получить веб-страницу. Для этого примера давайте возьмем общедоступную временную шкалу GitHub:

```
r = requests.get('https://api.github.com/events') 
```

Теперь у нас есть объект Response с именем r. Мы можем получить всю необходимую информацию из этого объекта.

Простой API запросов означает, что все формы HTTP-запросов одинаково очевидны. Например, вот как вы делаете запрос HTTP POST: r = requests.post('https://httpbin.org/post', data={'key': 'value'}) Приятно, правда? А как насчет других типов HTTP-запросов: PUT, DELETE, HEAD и OPTIONS? Все так же просто:

r = requests.put('https://httpbin.org/put', data={'key': 'value'}) r = requests.delete('https://httpbin.org/delete') r = requests.head('https://httpbin.org/get') r = requests.options('https://httpbin.org/get')

Это все хорошо, но это только начало того, на что способны Requests.
