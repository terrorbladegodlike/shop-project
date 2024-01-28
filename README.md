**Мы подошли к итоговому заданию по двум пройденным модулям.Нам удалось изучить:
• проектирование RESTful API;
• проектирование базы данных MySQL;
• процесс создания веб-сервера на Node.js и Express.js;
• разработку API для взаимодействия с базой данных;
• реализацию приложения на основе паттерна MVC;
• тестирование и документирование API при помощи Postman.
Все эти знания и умения пригодятся при доработке проекта Shop.Project.
Нужно реализовать три задачи на основе текущего проекта Shop.Project:
1. Добавить фичу «похожие товары». Эта доработка затронет:
    ◦ Shop.API;
    ◦ Shop.Admin;
    ◦ Базу данных.
2. Реализовать функционал создания товара в админке. Эта доработка затронет:
    ◦ Shop.API;
    ◦ Shop.Admin.
3. Создать клиентское приложение Shop.Client на React. Далее вы найдете требования к функционалу. Можете использовать любое оформление и любые библиотеки для интерфейса.
35.6.1 «Похожие товары»
В рамках этой задачи вам нужно реализовать функционал, подобный тому, что используется в различных интернет-магазинах.
В нашей базе данных в качестве товаров есть примеры смартфонов. Когда мы обращаемся к конкретному смартфону, мы должны получать список «похожих».
Тип похожего товара содержит поля:
• `id`.
• `title`.
• `description`.
• `price`.
База данных
Вам не требуется вносить корректировки в существующие таблицы БД. Для этой задачи создайте новую таблицу, которая будет хранить список соответствий.
Предположим, в БД есть товар «Galaxy A52» (первая строка в таблице). И предположим, что «похожие» товары для него — это «Phone X» и «iPhone SE» (выделены синим).
Для такого соответствия новая таблица должна содержать две строки:
• связь между «Galaxy A52» и «Phone X»;
• связь между «Galaxy A52» и «iPhone SE».
Для обращения к товарам используйте их `id`.
По аналогии с этим примером в новой таблице хранятся связи и между другими товарами.
Shop.API
В рамках `Products` API вам нужно реализовать три эндпоинта:
• получение списка «похожих товаров» для конкретного товара;
• добавление «похожих товаров»;
• удаление связей «похожих товаров».
Все три метода взаимодействуют только с новой таблицей БД:
• метод получения списка получает `id` товара и выдаёт список «похожих товаров», связанных с этим `id`;
• метод добавления получает список пар `id` товаров, которые нужно добавить в таблицу;
• метод удаления получает список `id` товаров, для которых нужно удалить все связи из таблицы.
Например, в новую и пока ещё пустую таблицу нужно добавить связь между:
• «Galaxy A52» и «Phone X»,
• «Galaxy A52» и «iPhone SE»,
• «Zenfone 8» и «Moto G60».
В этом случае метод добавления получает массив:
• первый элемент содержит `id` «Galaxy A52» и `id` «Phone X»;
• второй элемент содержит `id` «Galaxy A52» и `id` «iPhone SE»,
• третий элемент содержит `id` «Zenfone 8» и `id` «Moto G60».
Соответственно, в новую таблицу добавятся три строки.
Теперь методу получения списка похожих товаров мы отдаём `id` «Galaxy A52». В ответ получаем массив из двух товаров: «Phone X» и «iPhone SE».
Если мы запросим список похожих товаров у «Moto G60», то получим в ответ массив с товаром «Zenfone 8».
А теперь при помощи метода удаления нужно удалить все связи для «Galaxy A52» и «Moto G60». Для этого метод удаления принимает массив из двух элементов: `id` «Galaxy A52» и `id` «Moto G60».
После его выполнения в таблице не останется строк. Все три будут удалены.
Для всех трёх запросов нужно добавить валидацию входных данных при помощи express-validator. Проверок может быть несколько: на наличие массива в `body`, на структуру массива, на структуру данных и т.д.
Помимо этого нужно дополнить метод удаления товара. Так как у товара теперь могут быть зависимости по `Foreign Key` в таблице «похожих товаров», при удалении такого товара может возникнуть ошибка БД.
Shop.Admin
View
На странице с подробным описанием товара нужно вывести список похожих товаров. Каждый товар должен содержать:
• название товара;
• стоимость;
• чекбокс «mark to remove», чтобы отметить такой товар для удаления из «похожих».
Чекбоксы работают по аналогии с такими же чекбоксами для удаления комментариев и изображений.
Также на странице нужно вывести список остальных товаров, которые имеются в базе. Кроме:
• того товара, на странице которого мы в данный момент находимся;
• тех товаров, что уже являются «похожими» для данного товара.
В этом списке можно выбрать один или несколько товаров, чтобы отметить их для добавления в «похожие» к этому товару.
Вы можете апгрейдить текущие файлы представлений или добавлять свои в качестве компонентов.
Все изменения, связанные с добавлением и удалением похожих товаров, срабатывают по кнопке «Save changes». Как и сохранение любых других изменений, связанных с данным товаром.
Model
Вам нужно создать новые методы модели в файле products.model.ts. Также нужно будет дополнить метод модели для работы с формой обновления данных товара.
Controller
Нужно внести дополнения в один из контроллеров `Products`, который отвечает за работу с формой изменений данных товара. Другие контроллеры обновлений не требуют.
Пример работы фичи с абстрактными товарами
В базе данных есть товары А, B, C, D, E и F. Товары B и E — «похожие товары» для товара A.
Заходим в админке на страницу редактирования товара А и видим:
• блок со списком похожих товаров — товары B и E;
• блок со списком остальных товаров — товары C, D, F.
Товары B и E не участвуют в этом списке.
Товар A также не участвует, т.к. мы находимся на его странице.
Далее мы отмечаем товар B как «удалить из похожих». Товары C и F отмечаем для добавления в похожие. Нажимаем кнопку сохранения изменений.
После обновления страницы видим, как изменились оба списка:
• блок со списком похожих товаров — товары C, E, F;
• блок со списком остальных товаров — товары B, D.
35.6.2 Создание товара в панели администрирования
Для этой задачи вам потребуется вносить изменения в приложения админки и API. Суть задачи: реализовать функционал создания нового товара через админку.
Shop.API
В `Products` API у нас есть метод создания товара. В текущей реализации при успешном добавлении товара в базу, метод возвращает статус-код 201 и сообщение об успешном создании товара.
Нужно отрефакторить ответ этого метода таким образом, чтобы метод возвращал созданный товар типа IProduct.
Shop.Admin
Вы можете обновить существующие представления, контроллеры и модели для реализации функционала создания товара.
Или вы можете создать новые файлы или функции, чтобы не менять существующий функционал.
View
Вам нужно доработать шапку админки таким образом, чтобы на каждой странице отображалась ссылка “Add product”.
При этом ссылка не должна отображаться на странице логина.
При нажатии на ссылку “Add product” открывается страница “/admin/new-product”.
На этой странице можно указать:
• название товара;
• описание;
• стоимость.
Также на странице находится кнопка “Save changes”. При нажатии на неё осуществляется процесс создания товара. Далее происходит редирект на адрес /admin/:id по `id` созданного товара.
Model
Существующие методы модели products можно доработать или добавить новые. В моделях должно быть реализовано обращение к `Products` API для добавления товара в базу данных.
Controller
Вам необходимо добавить контроллер для обработки запроса на /new-product. Помимо этого, вы можете расширять существующие методы контроллера products или добавлять новые.
35.6.3 Создание Shop.Client
Вам нужно создать клиентское приложение на React.
У нас в проекте есть веб-сервер, который обрабатывает пути /api и /admin. Сервер отдаёт для этих путей соответствующие приложения.
Результат билда вашего React-приложения сервер должен отдавать по корневому пути проекта: http://localhost:3000.
Фактически сервер будет отдавать HTML-файл и встроенный в него JS-файл. В сумме это и является React-приложением.
Требования к использованию библиотек
• React;
• TypeScript;
• Axios;
• React Router для реализации роутинга.
Можете использовать другие библиотеки на своё усмотрение.
Также вы можете использовать state-подход для хранения данных или задействовать Redux, React Context. Реализация с использованием стейт-менеджеров позволит вам получить дополнительные баллы по итогам проекта.
Для инициализации проекта вы можете использовать create-react-app или любой другой известный вам способ.
Для организации интерфейса вы также можете использовать любые библиотеки с UI-компонентами или наборы стилей.
Мы просим вас сосредоточиться на функционале приложения. Желательно, чтобы дизайн был аккуратным и минималистичным. Основной упор нужно сделать на реализации логики приложения.
Для всего описанного ниже функционала приложения у нас уже готовы API-методы и ресурсы в БД.
Структура приложения
Мы предлагаем вам реализовать SPA-приложение с роутингом.
Переход на какую-либо страницу не должен быть заблокирован ожиданием ответов на сетевые запросы. Используйте лоадеры для обозначения ожидания загрузки данных.
Роутинг приложения:
• Главная страница (“/”);
• Список товаров (“/products-list”);
• Страница с описанием товара (“/:id”).
Главная страница приложения содержит:
• Заголовок “Shop.Client”;
• Текст: «В базе данных находится n товаров общей стоимостью m», где «n» — общее количество товаров в базе, а «m» – их суммарная стоимость.
• Кнопка «Перейти к списку товаров». При нажатии осуществляется переход внутри SPA по адресу /products-list.
• * *Кнопка* «*Перейти в систему администрирования*».
При нажатии осуществляется переход на /admin в новой вкладке браузера.
Страница с перечнем товаров содержит:
1. Заголовок «Список товаров (n)», где «n» — общее количество товаров;
2. Список товаров, где каждый товар представлен в виде:
    ◦ Заголовка;
    ◦ Изображения-обложки либо изображения-заглушки;
    ◦ Стоимости;
    ◦ Количества комментариев к товару.
3. ** Фильтр для поиска товаров по названию и стоимости* «*от*»*-*«*до*»*.*
При нажатии на заголовок товара или его изображение осуществляется переход на страницу с подробным описанием товара.
Страница с подробным описанием товара содержит:
1. Название товара в виде заголовка;
2. Изображение-обложку (либо заглушка);
3. Список остальных изображений (если есть);
4. Описание;
5. Стоимость;
6. Список похожих товаров, где каждый товар представлен в виде:
    ◦ названия товара;
    ◦ стоимости.
При клике по названию похожего товара должен осуществляться переход на страницу с его подробным описанием.
7. Список комментариев, где каждый комментарий представлен в виде:
    ◦ заголовка;
    ◦ E-mail;
    ◦ текста комментария.
8. ** Форма добавления комментария с полями и кнопкой* «*Сохранить*»*:*
    ◦ заголовок;
    ◦ E-mail;
    ◦ текст комментария.
При нажатии на «Сохранить» комментарий сохраняется в базу данных через API. И затем выводится на этой странице среди прочих комментариев.* выделенный функционал не обязателен. Но при его реализации вы получите дополнительные баллы.**

**Критерии оценки выполнения задания
Мы будем использовать комплексный подход для оценки задания.
Задание «Похожие товары»
Функциональные требования
• Требования выполнены, функционал приложения полностью соответствует ТЗ — 5 баллов;
• Требования в целом выполнены, но есть замечания — 3 балла;
• Требования выполнены частично или есть критические ошибки в работе — 1 балл;
• Требования не выполнены — 0 баллов.
База данных
• Добавлена одна таблица, которая полностью соответствует ТЗ – 5 баллов;
• Добавлено более одной таблицы либо внесены изменения в структуру уже имеющихся таблиц. Но этого хватило для полного соответствия ТЗ – 3 балла;
• Требования ТЗ осуществлены частично за счёт создания новых таблиц или внесения изменений в структуру уже имеющихся – 1 балл;
• Работа с БД не проводилась – 0 баллов.Дополнительно вы можете получить 10 баллов за создание таблицы при помощи SQL-запроса. Для этого добавьте текстовый файл с таким запросом при сдаче задания проверяющему.
Shop.API
• Все три метода API реализованы и полностью соответствуют требованиям ТЗ — 5 баллов;
• Все три метода API реализованы, но есть недочёты по коду, ошибки при работе с валидатором данных или при работе с БД — 3 балла;
• Реализованы 1 или 2 метода, есть ошибки при работе с валидатором данных или при работе с БД — 1 балл;
• Работы с `Products` API не осуществлялись либо внесённые в код изменения неработоспособны — 0 баллов.
Shop.Admin
• Все требования ТЗ, касающиеся изменений в представлениях, моделях и контроллерах приложения выполнены — 5 баллов;
• Требования ТЗ в целом выполнены, но есть недочёты по коду. Или не реализован функционал целиком — 3 балла;
• Требования ТЗ выполнены меньше, чем наполовину, есть серьёзные недочёты — 1 балл;
• Требования ТЗ не выполнены — 0 баллов.Максимальное количество баллов за задание — 30.
Задание «Создание товара в панели администрирования»
Функциональные требования
• Требования выполнены, функционал создания товара через админку полностью реализован — 5 баллов;
• Требования в целом выполнены, но есть замечания — 3 балла;
• Есть критичные ошибки в работе функционала создания товара — 1 балл;
• Требования не выполнены — 0 баллов.
Shop.API
• Был отрефакторен только метод создания товара в `Products` API. Рефакторинг не повлиял на текущую работу метода и при этом соответствует ТЗ — 5 баллов;
• Требования ТЗ выполняются, но был изменён текущий функционал или внесены изменения в другие методы API — 3 балла;
• Требования ТЗ выполняются частично, есть ошибки в коде или серьёзные баги в текущей работе методов после внесения изменений — 1 балл;
• Требования не выполнены — 0 баллов.
Shop.Admin
• Требования ТЗ полностью выполнены.
Для страницы создания товара не был добавлен новый шаблон с формой, а были внесены минимальные изменения в текущий шаблон product.ejs.
Ссылка добавления нового товара не отображается на странице логина
— 10 баллов;
• Требования ТЗ полностью выполнены. Был добавлен новый шаблон для страницы создания товара, ссылка на добавление нового товара не отображается на странице логина — 8 баллов;
• Требования ТЗ полностью выполнены и был добавлен новый шаблон для страницы создания товара. Ссылка на добавление нового товара отображается на всех страницах админки без исключения — 5 баллов;
• Требования ТЗ в целом выполнены, но есть недочёты — 3 балла;
• Требования ТЗ выполнены лишь частично, процесс добавления товара работает с серьезными багами, есть недочёты по коду — 1 балл;
• Требования не выполнены — 0 баллов.Максимальное количество баллов за задание — 20.
Задание «Создание React-приложения Shop.Client»
Функциональные требования
• Требования ТЗ полностью соблюдены — 20 баллов.
    ◦ приложение функционирует без критичных багов;
    ◦ роутинг внутри приложения реализован и работает;
    ◦ были использованы все обязательные библиотеки;
    ◦ был использован стейт-менеджер;
    ◦ дизайн приложения не выглядит перегруженным, в нём нет лишних элементов;
    ◦ использованы лоадеры для ожидания загрузки данных.
• Требования ТЗ в целом соблюдены, нет серьезных нареканий по коду.
Реализовано больше половины пунктов из списка выше — 10 баллов;
• Требования ТЗ соблюдены частично. Есть нарекания по коду либо проблемы с билдом приложения. При этом приложение работает, частично реализованный функционал работает — 5 баллов.
• Требования ТЗ не реализованы, приложение не работает — 0 баллов.За каждое задание со звёздочкой — 5 баллов. Максимальное количество баллов за задание — 35.**