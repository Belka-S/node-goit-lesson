
## 1 дз

По 1 дз - якщо все зрозуміло, то не читаємо, якщо не зрозуміло, то може це вам допоможе

## **Як працювати з yargs у switch**

Давайте з початку - у вашій домашній роботі передбачається реалізувати методи роботи з файлом: читання з нього всіх даних та виведення в консоль, додавання даних, видалення даних та пошук за id.
Але як ми можемо викликати ту чи іншу функцію виконання. Для цього нам треба якось скрипт передати дані.

Для цих цілей використовується **модуль yargs**він дозволяє передавати у вигляді аргументів дані, які можуть бути оброблені всередині вашого скрипта.
щоб просто запустити на виконання код з файлу в ноді треба написати node і назву того файлу який ви хочете запустити. У вашій дз це `node index.js`
Але нам треба якось передати в цей файл параметри, щоб можна було викликати на виконання конкретну функцію
 `node index.js --action="list"` 
ось тут відбувається запуск файлу index.js та передача аргументу `--action="list"`

**як він оброблятиметься у самому скрипті**

Спочатку підключаємо на початку файлу необхідний нам модуль - `const argv = require('yargs').argv;` та "достаємо"  з нього можливість роботи з аргументами. 

Далі створюємо функцію, яка приймає аргументи - `function invokeAction({ action, id, name, email, phone })`
 і викликаємо її - `invokeAction(argv);` -
якщо ми запустимо наш скрип з аргументами `node index.js --action="list" `- то в функцію потрапляє прапор `--action` зі значенням `list`
далі управління передається на
`switch (action) {`
і тут відбуватиметься пошук
    case 'list':
      // ...
      break;
відповіді зі значенням `list`

аналогічно відпрацюють решта **action**, які ви передасте, щоразу запускаючи скрипт з різними аргументами
`node index.js --action="get" --id=5`

##### Добавляємо контакт
`node index.js --action="add" --name="Mango" --email="mango@gmail.com" --phone="322-22-22"`

##### Видаляємо контакт
`node index.js --action="remove" --id=3`

**якщо ви спробуєте викликати ваш скрипт НЕ передавши аргументи - node index.j**s - ви потрапите на гілку -
       default:
          console.warn('\x1B[31m Unknown action type!');

і побачите в консолі помилку червоного кольору – з повідомленням, що це не **відомий action**

**Червоний колір забезпечує - \x1B[31m**

Ось таким методом працює у зв'язці argv та switch

Щоб у простому вигляді подивитися як це працює можете написати якось так

     // index.js
    const argv = require('yargs').argv;
    
    // TODO: рефакторить
    function invokeAction({ action, id, name, email, phone }) {
      switch (action) {
        case 'list':
          console.log('list')
          break;
    
        case 'get':
           console.log('id',id)
          break;
    
        case 'add':
         console.log( 'name email phone', name, email, phone)
          break;
    
        case 'remove':
          console.log('id',id)
          break;
    
        default:
          console.warn('\x1B[31m Unknown action type!');
      }
    }
    
    invokeAction(argv);

і запустіть цей скрип із запропонованими вам методами
`node index.js --action="list"`

### Отримуємо контакт з id
node index.js --action="get" --id=5

### Додаємо контакт
node index.js --action="add" --name="Mango" --email="mango@gmail.com" --phone="322-22-22"

### Видаляємо контакт
node index.js --action="remove" --id=3

Ну а далі вам треба прописувати методи в самому **contact.js** експортувати їх з файлу та імпортувати у файлі **index.js** та викликати їх у відповідних **case**

## Як додати скрипт користувача в мій файл package.json, який запускає файл index.js?
Ось що треба записати у файлі **package.json**

    "scripts": {
        "start": "node index.js"
    }
Цей код з файла package.json буде передавати керування від команди `npm start` на виконання команди `node index.js`. І тим самим запускатиметься на виконання файл **index.js**

**Поле «script»**
Можливо ви цього не знали, але npm містить поле під назвою `scripts` у файлі `package.json` проекту для того, щоб робити такі команди, як `npm test`, що фактично виконує вміст поля `scripts.test`, і `npm start`, що викликає інструкції з поля `scripts. start`.
**npm test і npm start** — це лише зручні посилання для `npm run test` і `npm run start`. За допомогою `npm run ...` можна виконати будь-який вміст будь-якого поля всередині `scripts`.

__________________________
### npm – головні скрипти
`npm` - головні скрипти - такі як запуск, зупинка, перезавантаження, встановлення, версія або тест не потребує виконання команди. Ці сценарії та деякі інші описані у документації npm. І виконуються вони через написання директиви `npm` та вказівки самої команди – наприклад.

**npm start**

Інші користувацькі скрипти npm, щоб викликати вам потрібно додати `run` перед ім'ям скрипта `npm run dev`
**`npm run dev`**

Абсолютно у всіх розробників знайомство з nodejs починається з того, що після кожної зміни потрібно перезавантажувати сервер. Тому виникає питання, як зробити так, щоб сервер перевантажувався автоматично.
Найпопулярніший варіант - **це nodemon.** Тобто ідея полягає в тому, що в `development` оточенні ми хочемо, щоб `nodemon` стежив за файлами, які ми змінюємо і просто перезапускав сервер, якщо ці файли відносяться до сервера.
Необхідно встановити локально в наш проект як `devDependencies nodemon`.

`npm i -D nodemon`

або

`yard add nodemon`

Тепер у package.json додамо команду для нього.

    "scripts": {
      "dev": "nodemon index.js"
    }

Тепер у консолі давайте запустимо його командою

`npm run dev`

Як ми бачимо, він запустився. Нам вийшло, що він увімкне всі файли в нашій папці і запускає команду node index.js при зміні будь-якого файлу.
Тепер, якщо ми змінимо наш index.js, то nodemon перезапустить сервер автоматично.
Разом у файлі package.json повинно бути додано два скрипти:

    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js"
      },

**В 1 ДЗ СКРИПТИ ЗАПУСКАТИ НЕМА СЕНСУ - ВИ НІЧОГО НЕ ПОБАЧИТЕ!!! ВОНИ У ВАС НЕ ВИКОНАЮТЬСЯ ПРАВИЛЬНО!!** Ви зможете з ними нормально працювати лише з 2 дз!! **ЯКЩО ЗАПУСТИТи СКРИПТИ зараз - ТО ОТРИМАЄТЕ ПОМИЛКУ - чому, тому що ви викликає index.js без аргументів і потрапляєте у гілку**

    default:
          console.warn('\x1B[31m Unknown action type!');

**І ЇЇ Ж ПОБАЧИТЕ В КОНСОЛІ!**

Що почитати- https://habr.com/ru/company/ruvds/blog/458504/ 


__________________________
### Як підключати файли?
Для підключення файлів можливо використати метод **path.join**. 

Приклад запису - `path.join([path1][, path2][, ...])` - цей метод поєднує всі аргументи та нормалізує отриманий шлях.

Для отримання точки відліку - звідки треба прокласти шлях до потрібного файлу використовується **__dirname** - повертає шлях до каталогу поточного файлу, що виконується. І вказуємо шлях до файлу, який хочемо підключити.

`const path = require('path');
const absolutePath = path.join(__dirname, './db/contacts.json');`


__________________________
### Як експортувати функції з файлу та імпортувати?

При розділенні програмного коду на кілька файлів `module.exports` використовується передача змінних і функцій для використання в тому місці, де буде викликаний цей модуль.
Якщо ви просто хочете виставити тільки одну функцію або змінну, наприклад:


    // test.js
    var name = 'william';
    
    module.exports = function(){
        console.log(name);
    }   



    // index.js
    var test = require('./test');
    test();


цей модуль викликає тільки саму функцію, а змінна `name` є локальною тому вона є недоступною для зовнішнього модуля, в якому підключається `'./test'`.


Якщо необхідно експортувати кілька функцій або змінних - можна передати в module.exports об'єкт:


    //data.js
    
    module.exports = {
        getContacts,
        getById
    }


    //app.js
    const functions = require('./data.js');
    functions.getContacts();
    functions.getById();
    
    
або можна


    const { getContacts, getById } = require('./data.js');
    getContacts();
    getById();
    

__________________________
### Яка різниця між const fs = require('fs') та fs = require('fs').promises; 

Спочатку сам модуль був написаний з використанням `callback` функцій і викликався через const `fs = request("fs")`;

Далі js розвивався і до модулю додали підтримку **промісів**

Тепер можна використовувати функції, але працювати з ними не через колбаки, а через проміси. Відповідно, треба підключити роботу з промісами 

`const fs = require('fs').promises`

Зараз вже працюють, не через проміси, а через обгортку над промісами async/await. Це скорочує код і такий код читати легше

Якщо вирішили використовувати колбеки, підключаєте бібліотеку без промісів.


__________________________
### callback / promises / async/await

#### Сallback
Небагато інформації з базового JS
Що таке callback - "У функції, які виконують будь-які асинхронні операції, передається аргумент callback - функція, яка буде викликана після завершення асинхронної дії"

коли вони використовуються? - коли є «асинхронність» (коли якийсь процес буде завершено не зараз, а потім)


    function НазваФункції (аргументи, callback) {
     ......(якийсь код)
    }

Тепер робота із самою функцією колбека

    function НазваФункції (аргументи, function(error, data) {
      if (error) {
        // обробляємо помилку
      } else {
        // успішно виконано
      }
    });


такий підхід називається «**колбек з першим аргументом-помилкою**» («error-first callback»).
**Правила такі:**
Перший аргумент функції `callback` зарезервований помилки. У цьому випадку виклик виглядає так: `callback(err)`.
Другий та наступні аргументи — для результатів виконання. У цьому випадку виклик виглядає так: `callback(null, result1, result2…)`.
Одна і та ж функція `callback` використовується і для інформування про помилку, і передачі результатів.

#####Тепер про те, як це виглядає стосовно вашої 1 дз і до першої функції

    function listContacts() {
      fs.readFile(contactsPath, (err, data) => {
        if (err)  return console.error(err.message);
          console.table(JSON.parse(data.toString()));
      });
    }
    
это если мы собираемся в основном файле просто вызвать функцию listContacts()
Если вы хотите вернуть данные и уже в основном файле и обработать и вывести их в консоль, то


    function listContacts() {  
    fs.readFile(contactsPath, (err, data) => {    
    if (err) return console.error(err.message);     
       return data;  
    });  
    return JSON.parse(list);
    }
тогда в основном файле вы можете вызвать 

    listContacts().then(data => console.table(data)

#### Потом эволюция дошла до промисов, Потом эволюция дошла до промисов,
`const fs = require('fs').promises`
Код, которому надо сделать что-то асинхронно, создаёт объект promise и возвращает его.
promise.then навешивает обработчики на успешный результат или ошибку
Если очередной then вернул промис, то далее по цепочке будет передан не сам этот промис, а его результат.
Если then возвращает промис, то до его выполнения может пройти некоторое время, оставшаяся часть цепочки будет ждать.
То есть, логика довольно проста:
В каждом then мы получаем текущий результат работы.
Можно его обработать синхронно и вернуть результат (например, применить JSON.parse). Или же, если нужна асинхронная обработка – инициировать её и вернуть промис.
При возникновении ошибки – она отправляется в ближайший обработчик onRejected.
Такой обработчик нужно поставить через второй аргумент .then(..., onRejected) или, что то же самое, через .catch(onRejected).
Чтобы поймать всевозможные ошибки, которые возникнут при загрузке и обработке данных, добавим catch в конец цепочки
`promise.then(onFulfilled, onRejected)`
пример первой функции з дз на чистых промисах


    function listContacts() { 
    readFile(contactsPath,'utf-8')
    .then(data => console.log(JSON.parse(data))
        .catch(err => console.log(err))
    }
Если собираемся вернуть данные в основной файл
 

    function listContacts() { 
    const list = readFile(contactsPath,'utf-8')
    .then(data => return JSON.parse(data))
        .catch(err => console.log(err))
    return list
    }

#### Async/await
Существует специальный синтаксис для работы с промисами, который называется «async/await». Он удивительно прост для понимания и использования.
По сути, это просто «синтаксический сахар» для получения результата промиса, более наглядный, чем promise.then.
Что бы переписать функцию на промисах с помощью async/await:
Нам нужно заменить вызовы .then на await.
И добавить ключевое слово async перед объявлением функции.


    async function listContacts() => {
      const res = await readFile(contactsPath);
      console.log(res)
    }
Ошибки можно ловить, используя try..catch, как с обычным throw
В случае ошибки выполнение try прерывается и управление прыгает в начало блока catch. Блоком try можно обернуть несколько строк:


    async function listContacts() {
      try {
        const data = await fs.readFile(contactsPath);
        const result = JSON.parse(data);
        console.table(result);
      } catch (error) {
        console.log(error);
      }
    }
Если собираемся вернуть данные в основную функцию


    async function listContacts() {
      try {
        const data = await fs.readFile(contactsPath);
        const result = JSON.parse(data);
         return result;  
      } catch (error) {
        console.log(error);
      }
    }
Что дополнительно почитать:
https://habr.com/ru/company/skillbox/blog/458950/

#### Для чего нужен файл .gitignore?
Для чего нужно использовать файл .gitignore, если можно просто выбрать файлы которые необходимо закоммитить, и сделать это?
Есть несколько задач, которые наиболее эффективно решаются с использованием файла .gitignore:
Не мучаться с выбором нужных файлов для индексации (которая git add).
В большом проекте часто бывает много файлов, которые не подлежат версионированию. А ещё вам может быть удобно хранить какие-то промежуточные результаты в папке tmp.
Настройка .gitignore позволяет не выискивать нужные файлы, а добавлять всё сразу или по крайней мере уточнять меньше.
Сделать локальный конфиг, который не будет затронут pull-ом.
Защитить чувствительную информацию от случайного раскрытия.
Случается, что вы случайно добавили и закоммитили ключи или пароли от какого-нибудь облачного хранилища, например Amazon.
Если так уж необходимо хранить чувствительную информацию в папке проекта, то нужно положить её в под-папку, игнорируемую git.
Быстро очищать проект от временных файлов.
Предположим, что вы пишете на компилируемом языке и при построении вашего проекта формируется множество промежуточных файлов (объектные, прекомпиляция, вот это всё). Перед каждой сборкой необходимо их удалять, чтобы в случае чего не прилинковать лишнее. Можно делать это вручную. Можно написать скрипт. А можно просто добавить их в .gitignore и делать так:
git clean -fX
________________________________________________
файл  .gitignore - можно сгенерировать и тогда вам самостоятельно надо разбираться с теми зависимостями, которые туда добавил генератор  .gitignore
или вы можете собрать самостоятельно создав в корне проекта файл  .gitignore: и добавить туда ту информацию по вашему проекту, которую необходимо не загружать на гит.
 комментарий — эта строка игнорируется
 не обрабатывать файлы, имя которых заканчивается на .a
*.a
 НО отслеживать файл lib.a, несмотря на то, что мы игнорируем все .a файлы с помощью предыдущего правила
!lib.a
 игнорировать только файл TODO находящийся в корневом каталоге, не относится к файлам вида subdir/TODO
/TODO
 игнорировать все файлы в каталоге build/
build/
 игнорировать doc/notes.txt, но не doc/server/arch.txt
doc/*.txt
 игнорировать все .txt файлы в каталоге doc/
doc/**/*.txt
_______________________________________________________________________
Какая задача на этапе 1 дз - убрать папку node_modules
##### Решение создать файл .gitignore и добавить него запись:
node_modules/*
_____________________________________
Как игнорировать файлы, которые уже отслеживаются?
Если вы добавили файл или папку в .gitignore, после того как они попали в репозиторий, то их необходимо удалить из репозитория командой:
`git rm --cached <file>`
Например убрать папку storage/framework/cache/. Обратите внимание: вначале отсутствует слеш.
git rm -r --cached "storage/framework/cache/"
что почитать на тему - https://devacademy.ru/article/ignorirovanie-faylov-i-katalogov-v-git