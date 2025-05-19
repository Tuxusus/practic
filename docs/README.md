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

В данной части туториала мы создадим базовый сервер, который будет работать со статическим сайтом, написанным в рамках задания по проектной практике. Эта часть будет в себе содержать этапы модификации и с подробным пояснением. Для начала диограмма классов нашего сайта с наполнением:

 
Наполнение `index.html`:
```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Главная страница</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Добро пожаловать!</h1>
        <nav>
            <a href="index.html" class="active">Главная</a>
            <a href="page2.html">Страница 2</a>
        </nav>
    </header>
    
    <main>
        <section>
            <h2>О нас</h2>
            <p>Это главная страница нашего простого сайта.</p>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2023 Простой сайт</p>
    </footer>

    <script src="js/script.js"></script>
</body>
</html>
```
Наполнение `page2.html`:
```
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Вторая страница</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Вторая страница</h1>
        <nav>
            <a href="index.html">Главная</a>
            <a href="page2.html" class="active">Страница 2</a>
        </nav>
    </header>
    
    <main>
        <section>
            <h2>Контент второй страницы</h2>
            <p>Здесь находится содержимое второй страницы.</p>
            <button id="alertBtn">Показать сообщение</button>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2023 Простой сайт</p>
    </footer>

    <script src="js/script.js"></script>
</body>
</html>
```
Наполнение `style.css`:
```
/* Общие стили */
body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
    color: #333;
    background-color: #f4f4f4;
}

header {
    background: #35424a;
    color: white;
    padding: 1rem 0;
    text-align: center;
}

nav a {
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    margin: 0 0.5rem;
}

nav a.active {
    background: #e8491d;
    border-radius: 5px;
}

main {
    padding: 2rem;
    max-width: 800px;
    margin: 0 auto;
}

footer {
    text-align: center;
    padding: 1rem;
    background: #35424a;
    color: white;
    position: fixed;
    bottom: 0;
    width: 100%;
}

button {
    background: #35424a;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    cursor: pointer;
    border-radius: 5px;
}

button:hover {
    background: #e8491d;
}
```
Наполнение `script.js`:
```
// Функция для главной страницы
if (document.getElementById('changeColorBtn')) {
    const changeColorBtn = document.getElementById('changeColorBtn');
    let colors = ['#f4f4f4', '#e6f7ff', '#ffe6e6', '#e6ffe6'];
    let currentColor = 0;
    
    changeColorBtn.addEventListener('click', function() {
        document.body.style.backgroundColor = colors[currentColor];
        currentColor = (currentColor + 1) % colors.length;
    });
}

// Функция для второй страницы
if (document.getElementById('alertBtn')) {
    const alertBtn = document.getElementById('alertBtn');
    
    alertBtn.addEventListener('click', function() {
        alert('Привет со второй страницы!');
    });
}
```
После воссоздания проекта открываем его в **Visual Studio Code** и переходим в папку `server.js`. Она сейчас пустая, давайте напишим в ней код для запуска простого сервера:
```
const http = require('http');
const PORT = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!');
});

server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
});
```
Пока что наш веб-сервер не работает с другими файлами, а просто выводит "*Hello, World!*". Чтобы это исправить, давайте добавим обработку статических файлов(**HTML**, **CSS**, **JS**) и проверку на то, существует ли файл:
```
const http = require('http');
const PORT = 3000;

const fs = require('fs');
const path = require('path');
const url = require('url');

const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'application/javascript'
};

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url);
  let filePath = path.join(__dirname, parsedUrl.pathname === '/' ? 'index.html' : parsedUrl.pathname);

  // Проверка существования файла
  fs.access(filePath, fs.constants.F_OK, (err) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/html' });
      return res.end('<h1>404 Not Found</h1>');
    }

    // Определение MIME-типа
    const ext = path.extname(filePath);
    const contentType = MIME_TYPES[ext] || 'text/plain';

    // Чтение и отправка файла
    fs.readFile(filePath, (err, content) => {
      if (err) {
        res.writeHead(500);
        return res.end('Server Error');
      }

      res.writeHead(200, { 'Content-Type': contentType });
      res.end(content);
    });
  });
});

server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
});
```
Теперь наш сервер работает со статическими файлами проекта `my-site`. Давайте добавим возможность при запуске сервера открывать интересующую нас страницу сайта. Внесём следующие изменения:
```
server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
  console.log('Доступные страницы:');
  console.log(`- Главная: http://localhost:${PORT}/`);
  console.log(`- Страница 2: http://localhost:${PORT}/page2.html`);
});
```
Когда вы запустите сервер повторно в консоли появится список с доступными страницами и ссылками на них. Теперь мы можем открывать наш сайт не только с главной страницы, но и со второй. Осталось сделать завершение работы нашего сайта более прозрачным и выводить сообщение в консоль:
```
// Обработка завершения
process.on('SIGINT', () => {
  console.log('\nСервер останавливается...');
  server.close(() => {
    console.log('Сервер успешно остановлен.');
    process.exit(0);  // явно указываем код выхода 0 (успех)
  });

  // Принудительное завершение, если сервер не остановился за 5 секунд
  setTimeout(() => {
    console.error('Принудительное завершение...');
    process.exit(1);
  }, 5000);
});
```
Наш базовый сервер готов. Итоговый код:
```
const http = require('http');
const PORT = 3000;

const fs = require('fs');
const path = require('path');
const url = require('url');

const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'application/javascript'
};

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url);
  let filePath = path.join(__dirname, parsedUrl.pathname === '/' ? 'index.html' : parsedUrl.pathname);

  fs.access(filePath, fs.constants.F_OK, (err) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/html' });
      return res.end('<h1>404 Not Found</h1>');
    }

    const ext = path.extname(filePath);
    const contentType = MIME_TYPES[ext] || 'text/plain';

    fs.readFile(filePath, (err, content) => {
      if (err) {
        res.writeHead(500);
        return res.end('Server Error');
      }

      res.writeHead(200, { 'Content-Type': contentType });
      res.end(content);
    });
  });
});

server.listen(PORT, () => {
  console.log(`Сервер запущен на http://localhost:${PORT}/`);
  console.log('Доступные страницы:');
  console.log(`- Главная: http://localhost:${PORT}/`);
  console.log(`- Страница 2: http://localhost:${PORT}/page2.html`);
});

// Обработка завершения
process.on('SIGINT', () => {
  console.log('\nСервер останавливается...');
  server.close(() => {
    console.log('Сервер успешно остановлен.');
    process.exit(0);  // явно указываем код выхода 0 (успех)
  });

  // Принудительное завершение, если сервер не остановился за 5 секунд
  setTimeout(() => {
    console.error('Принудительное завершение...');
    process.exit(1);
  }, 5000);
});
```

## Заключение 

В этом руководстве мы рассмотрели создание простого и базового HTTP-сервера на Node.js, а также научились обработке различных маршрутов и работе со статическими файлами.
