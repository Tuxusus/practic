# **Создание веб-сервера на Node.js**

## Оглавление
- [Введение](#введение)
- [Подготовка](#подготовка)
- [Первый сервер](#первый-сервер)
- [Модификация сайта](#модификация-сайта)
- [Заключение](#заключение)


## Введение
В этом руководстве мы пошагово разберёмся, как создавать веб-сервер на **Node.js**. Создадим сначала самый простой веб-сервер, который будет работать без статических файлов, затем создадим базовый веб-сервер, который будет работать с уже написанными файлами **HTML**, **CSS**, **JS** и подробно опишем подифекацию проекта для практики.

## Подготовка
Для создания любого веб-сервера на **Node.js** нам понадобится установить следующие иснтрументы: [**Visual Studio Code**](https://code.visualstudio.com/download) и [**Node.js**](https://nodejs.org/en/download). После установки давайте создадим наш проект с названием `practic` и в этом проекте создадим файл с названием `server.js` в котором и будем с вами работать в рамках этого туториала. Для начала работы откройте файл `server.js` в **Visual Studio Code** и приготовтесь вникать.

## Первый сервер
Для начала мы с вами напишем сервервер, который не будет взаимодействовать со статическими файлами такими как HTML, CSS, JS. Наш первый сервер будет сам создавать с траничку с надписью "*Привет мир*". Переходим к самой практике. Напишите в файле следующий код:
```
const http = require('http');
http.createServer().listen(3000);
```
На данный момент код, который мы написали создаёт самый простой сервер, который не может принимать и отвечать на запросы. Если вы попробуете запустить его через терминал **Visual Studio Code**, написав `node server.js` и откроете по ссылке [`http://localhost:3000`](http://localhost:3000), то у вас откроется страница, которая какое-то время будет пытаться загрузиться, но в конечном итоге браузер выдаст ошибку. Это происходит по тому, что мы не прописали логику принятия запроса(`req`) и отпраки ответа(`res`). Давайте остановим работу нашего текущего сервара комбинацией клавиш `Contrl +` и изменим код, чтобы страница смогла открыться. Перенесите в файл следующий код:
```
const http = require('http');
http.createServer((req, res) => res.end()).listen(3000);
```
Теперь запуская сервер и переходя по ссылке у нас открывается белая страница. Отлично, по сути мы создали самый приметивный веб сервер. Теперь давайте что-нибудь добавим на нашу страницу, например слово "*Привет*". Замените код в файле на следующий:
```
const http = require('http');
http.createServer((req, res) => {
  // Устанавливаем правильные заголовки
  res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});
  res.end('Привет!');}).listen(3000);
```
Теперь перезапустив наш сервер и перейдя по ссылке, у вас должна открыться белая страница в браузере на которой будет написано "Привет". Если у вас всё получилось, то поздравляем. Вы создали свой первый просто веб-сервер, который работает не со статическими файлами, а сам выводит на странице слово "*Привет!*", когда мы переходим по ссылке. На этом создание первого сервера закончено.

## Модификация сайта

В данной части туториала мы создадим базовый сервер, который будет работать со статическим сайтом, написанным в рамках задания по проектной практике. Эта часть будет в себе содержать этапы модификации и с подробным пояснением. Структуру сайта и содержимое файлов можете посмотреть в папке `site`. Для начала давайте импортируем необходимые встроенные модули Node.js:
```
const http = require('http');
const fs = require('fs');
const path = require('path');
```
Теперь определяем порт сервера и MIME-типы для правильной обработки файлов:
```
const PORT = 3000;
const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'text/javascript',
  '.png': 'image/png',
  '.jpg': 'image/jpg',
  '.jpeg': 'image/jpeg',
  '.gif': 'image/gif',
  '.svg': 'image/svg+xml',
  '.ico': 'image/x-icon'
};
```
Создаем сервер и определяем обработчик запросов:
```
const server = http.createServer((req, res) => {
  // Декодируем URL и удаляем параметры запроса
  let filePath = decodeURI(req.url.split('?')[0]);
  
  // Если URL заканчивается на / или это корень, используем index.html
  if (filePath.endsWith('/') || filePath === '/') {
    filePath = '/index.html';
  }

  // Если в URL нет расширения, добавляем .html
  if (!path.extname(filePath)) {
    filePath += '.html';
  }

  // Формируем полный путь к файлу
  const fullPath = path.join(__dirname, filePath);
```
Читаем файл из файловой системы и отправляем ответ:
```
  fs.readFile(fullPath, (err, data) => {
    if (err) {
      // Если файл не найден, отправляем 404
      res.writeHead(404, { 'Content-Type': 'text/html; charset=utf-8' });
      return res.end('<h1>404 - Страница не найдена</h1>');
    }

    // Определяем Content-Type по расширению файла
    const extname = path.extname(filePath).toLowerCase();
    const contentType = MIME_TYPES[extname] || 'application/octet-stream';

    // Отправляем успешный ответ с содержимым файла
    res.writeHead(200, { 'Content-Type': contentType });
    res.end(data);
  });
});
```
Запускаем сервер на указанном порту:
```
server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
});
```
Добавляем обработчик для корректного завершения работы сервера:
```
process.on('SIGINT', () => {
  console.log('\nОстановка сервера...');
  server.close(() => process.exit());
});
```
Собрав все вместе, получаем следующий код:
```
const http = require('http');
const fs = require('fs');
const path = require('path');

const PORT = 3000;
const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'text/javascript',
  '.png': 'image/png',
  '.jpg': 'image/jpg',
  '.jpeg': 'image/jpeg',
  '.gif': 'image/gif',
  '.svg': 'image/svg+xml',
  '.ico': 'image/x-icon'
};

const server = http.createServer((req, res) => {
  let filePath = decodeURI(req.url.split('?')[0]);
  
  if (filePath.endsWith('/') || filePath === '/') {
    filePath = '/index.html';
  }

  if (!path.extname(filePath)) {
    filePath += '.html';
  }

  const fullPath = path.join(__dirname, filePath);

  fs.readFile(fullPath, (err, data) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/html; charset=utf-8' });
      return res.end('<h1>404 - Страница не найдена</h1>');
    }

    const extname = path.extname(filePath).toLowerCase();
    const contentType = MIME_TYPES[extname] || 'application/octet-stream';

    res.writeHead(200, { 'Content-Type': contentType });
    res.end(data);
  });
});

server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
});

process.on('SIGINT', () => {
  console.log('\nОстановка сервера...');
  server.close(() => process.exit());
});
```
## Заключение 

В этом руководстве мы рассмотрели создание простого и базового HTTP-сервера на Node.js, а также научились обработке различных маршрутов и работе со статическими файлами.
