# Тестовое задание javaRush

### 1. Задачи

#### Требуемые технологии:
* Maven 3 (v.3.3.9) (для сборки проекта);
* Tomcat8 (для тестирования своего приложения);
* Spring (версия не ниже 4.3.0.RELEASE);
* Hibernate (версия не ниже 5.2.1.Final);
* MySQL (база данных). 
    >Для упрощения тестирования называйте все свою базу `test`, с логином и паролем `root`(нам не нужно будет для тестирования создавать кучу лишних и ненужных баз);
* Frontend: Spring MVC, или Angular, или Vaadinor, или ZK framework;
* Результат выложить на GitHub или Bitbucket.

>Версии можно смело брать самые последние. Конфликтов быть не должно.

#### CRUD(create, read, update, delete).
Необходимо реализовать стандартное CRUD приложение, которое отображаем список всех книг в базе (с пейджингом по 10 книг на странице). С возможностью их удаления, редактирования, добавления новых, и поиска по уже существующим.

У вас есть всего 1 таблица book. В ней хранится список книг(например, на книжной полке).Книги на полку можно добавлять (create), брать посмотреть(read), заменять на новый выпуск (update), убирать (delete).

**Данные, которые должны быть в таблице:**
* `id` –идентификатор книги в БД;
* `title` –название книги. Можно использовать тип VARCHAR(100);
* `description` –краткое описание о чем книга. Можно использовать тип VARCHAR(255);
* `author` –фамилия и имя автора. Можно использовать тип VARCHAR(100);
* `isbn` –ISBNкниги. Можно использовать тип VARCHAR(20);
* `printYear` –в каком году напечатана книга (INT);
* `readAlready` –читал ли кто-то эту книгу. Это булевополе.

**Бизнес-требование:** 
при редактировании может быть 2 варианта поведения:
* Книгу прочитали, и тогда изменяется только поле `readAlready`, и только, если оно было `false`.Значения поля должно стать `true`.
* Книгу заменили на новое издание. В этом случае должна быть возможность изменить `title`, `description`, `isbn`, `printYear`. А поле `readAlready` нужновыставить в `false`.Поле `author` должно быть неизменяемым с момента создания книги. 
* По какому полю искать – каждый решает для себя сам. Можно ограничиться полем title, но согласитесь, удобно просмотреть книги, которые еще не читал, или, которые вышли после 2016 года.


#### NOTES
Реализовать простенькое приложение **Notes-list**, для отображения списка заметок.

Нужно показывать список уже созданных заметок(с пейджингомпо 10 заметокна странице). Каждуюиз них можно редактировать, добавлять новые, отмечать как «Выполнено», удалять. Список можно фильтровать как «Все заметки», «Только невыполненные», «Выполненные». 

Заметки хранить в базе. Схему таблички для хранения нужно придумать самому (можно ограничиться одной таблицей).

**Бизнес-требование:** кроме фильтрации должна быть возможность сортировки заметокпо дате создания (например, поле `createdDateв` БД). Тип поля – `DATE` или `DATETIME`, или `TIMESTAMP`.

***

### 2. Что нам понадобиться

*   [IntelliJ IDEA](https://www.jetbrains.com/idea/download/);
*   [JDK 6](http://www.oracle.com/technetwork/java/javase/downloads/index.html) или более поздняя [версия](http://www.oracle.com/technetwork/java/javase/downloads/index.html);
*   [Maven](https://maven.apache.org/download.cgi);
    * Как установить и собрать простой Java-проект с помощью Maven читаем [здесь](https://spring.io/guides/gs/maven/);
*   [MySQL](https://dev.mysql.com/downloads/installer/);
*   [Git](https://git-scm.com/book/ru/v2/%D0%92%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5-%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-Git);

### 3. Установка Spring 

Для выполнения задач нам требуется Spring Framework. Мы будем использовать Spring Boot 1.5.8

*   Генерируем Spring с помощью [SPRING INITIALIZR](https://start.spring.io/)
    *   зависимости:
        *   Web - стек пакетов для web разработки с Tomcat и Spring MVC;
        *   JPA - JPA Persistence API, включая Spring-data-jpa, Spring-orm и Hibernate;
        *   JDBC - Базы данных JDBC;
        *   MySQL - MySQL jdbc драйвер.
*   Качаем и распаковываем архив;
*   Импортируем проект в IntelliJ IDEA. Как это сделать читаем [здесь](https://spring.io/guides/gs/intellij-idea/).

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/de7068c267681004c04419305dcc000c858934c9)

### 4. Создание и подключение базы данных
* Создаем пустую базу
    
    `mysql>  CREATE DATABASE IF NOT EXISTS test;`
    
* [Подключаем ее в IntelliJ IDEA](https://www.jetbrains.com/help/idea/working-with-the-database-tool-window.html#create_data_source)
* [Настраиваем подключение в Spring](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-connect-to-production-database)
    
    `application.properties`
    
    ```properties
    spring.jpa.hibernate.ddl-auto=none
    spring.datasource.url=jdbc:mysql://localhost:3306/test?autoReconnect=true&useSSL=false
    spring.datasource.username=root
    spring.datasource.password=root
    ```

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/6e13e0a955338ed46ef796ed3ec1fe0934ace46a)

### 5. Проверяем работоспособность
* Создаем первый контроллер [Controller.HomeAction](https://github.com/vladmeh/javaRushTestTask/blob/a9ec5dfab0c1c38431754aa40ba9b4562e6c35a7/src/main/java/com/vladmeh/javaRushTestTask/Controller/HomeAction.java);
* Первое тестирование
    * [ApplicationTest](https://github.com/vladmeh/javaRushTestTask/blob/07d4d78265f902c29d89e1a5b40f53230ac8cc39/src/test/java/com/vladmeh/javaRushTestTask/ApplicationTest.java) - проверяет что контекст создает нашконтроллер, а так же обрабатывает наш входящий HTTP запрос правильно (без затрат на запуск сервера) 
    * [HttpRequestTest](https://github.com/vladmeh/javaRushTestTask/blob/07d4d78265f902c29d89e1a5b40f53230ac8cc39/src/test/java/com/vladmeh/javaRushTestTask/HttpRequestTest.java) - обрабатывает наш входящий HTTP запрос
    
    
>Подробнее о тестирвоание Spring boot [здесь](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-testing.html) и [здесь](https://docs.spring.io/spring/docs/current/spring-framework-reference/testing.html#integration-testing-support-jdbc)

* Запускаем JavaRushTestTaskApplication в IDEA
    *   в браузере http://localhost:8080 будет выведено "Hello World";
* Запускаем тесты - все тесты должны проити успешно.

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/07d4d78265f902c29d89e1a5b40f53230ac8cc39)

### 6. Создаем таблицу и модель данных
* Сразу же создаем [скрипт sql](https://github.com/vladmeh/javaRushTestTask/blob/2c7eae388eb36dceecb1ea6cdde6cb87db4ce71d/src/main/resources/database.sql) где пишем создание базы, таблицы, полей.
* Создаем модель данных [Entity.Book](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Entity/Book.java)
* Создаем интерфейс репозитария [Repository.BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java) который пока наследуется от CrudRepository
* Сoздаем контроллер [Controller.BookController](https://github.com/vladmeh/javaRushTestTask/blob/fa277c2f0d5697878cb013e5a888d775bde06e92/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java) с одним методом getAllBook который будет возвращать полный список наших книг

>Подробнее о создании Spring Data JPA читаем [здесь](https://www.petrikainulainen.net/spring-data-jpa-tutorial)

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/fa277c2f0d5697878cb013e5a888d775bde06e92)

### 7. Реализация сервисного интерфейса
* Обновляем модель данных [database.sql](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/resources/database.sql)
    * в модель данных добаляем сценарий для наполнения данными
* Создаем интерфейс [Service.BookService](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
* Создаем класс реализации интерфейса BookService [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java) 
* Обновляем контроллер [Controller.BookController](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * контроллер теперь реализует свои методы через нашу сервисную службу BookService
    * добавляем методы 
        * `findBookById` -  поиск конкретной книги по id
        * `create` - создание новой книги
        * `update` - обновление существующей книги
        * `delete` - удаление существующей книги
* Тестируем
    * Модульное тестирование контроллера с помощью unit теста [BookController.test](https://github.com/vladmeh/javaRushTestTask/blob/755320b2d6eff2f8023453e9659a238f290d574e/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    * Работоспособность я тестировал с помощью сервиса [Postman Echo](https://www.getpostman.com/), с помощью которого посылаем запрос на наш сервер например добавим запись:
    ```cfml
    POST /books HTTP/1.1
    Host: localhost:8080
    Content-Type: application/json
    Cache-Control: no-cache
    
    {
    "title": "PHP объекты, шаблоны и методики программирования",
    "description": "Создавайте высокопрофессиональный код на PHP, изучив его объектно-орентированные возможности, шаблоны проектирования и важные средства разработки.",
    "autor": "Мет Зандстра",
    "isbn": "978-5-8459-1689-1",
    "printYear": 2013,
    "readAlready": true
    }
    ```
    
    удалим эту запись:
    
    ```cfml
    DELETE /books/14 HTTP/1.1
    Host: localhost:8080
    Cache-Control: no-cache
    ```

На данном этапе по адресу http://localhost:8080/books можем видеть список наших книг в формате json, а по адресу http://localhost:8080/books/1 - данные одной книги. Можем создавать, удавлять и обновлять наши записи.

[Итог](https://github.com/vladmeh/javaRushTestTask/tree/755320b2d6eff2f8023453e9659a238f290d574e)


### 8. Пейджинг и сортировка.
* Модифицируем интерфейс [BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java)
    * изменяем интерфейс так что бы он расширял интерфейс `PagingAndSortingRepository<T, ID extends Serializable>`
* Делаем изменения в интерфейс [BookService](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
    * добавляем метод `findAllByPage`, который будет принимать в качестве аргумента экземпляр интерфейса `Pageable`
* Реализуем метод `findAllByPage` в классе [BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java)

#### Разбиение на страницы.
* Модифицируем контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/edf9a5c3b1b914c46273ca73d18f01f0bb86c9b9/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * добаляем метод `getPageBooks`, который возращает постраничный список книг, с параметром `page` (номер страницы) с дефолтным значением `1`. Если параметр не указан будет выводиться первая страница.
    >метод `getAllBook` оставляем (изменим только у него `path`, что бы не было конфликтов), он нам может понадобится.
    
Сейчас по запросу в браузере `http://localhost:8080/books?page=2` будет выводиться 2-я страница списка наших книг

#### Сортировка.
* Модифицируем контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/9574047f23b003ab245996c2063547737ab0c078/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)
    * в метод `getPageBooks` добавляем два параметра `String sortBy` и `String order` отвечающих по какому полю будем сортировать и направление сортировки, соответственно. Выставляем дефолтные значения - по умолчанию (без паравметров) список будет сортироваться по `id` и по возрастанию;
    * реализуем сортировку.
    
По запросу в браузере `http://localhost:8080/books?page=2&sortBy=printYear&order=desc` будет выводиться 2-я страница списка наших книг, список отсортирован по году выпуска книги, по убыванию.

#### Тестирование
* Создаем класс [BookBuilder](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/BookBuilder.java)
* Создаем класс [PageBuilder](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/PageBuilder.java)
    >для чего это надо и что это такое читаем [здесь](http://www.natpryce.com/articles/000714.html)
    
В имеющимся тестовом классе `Controller.BookControllerTest` мы, по-сути, тестировали поведение нашего сервиса `BookService`. Поэтому переименуем тестовый класс на [Service.BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java).

* Переименовываем `Controller.BookControllerTest` -> `Service.BookServiceTest`;
    * пишем модульные тесты для методов `getPageBooks`, `findBookById`, `delete`;
* Создаем новый тестовый класс [Controller.BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java).
    * пишем методы для тестирования ответа сервера на наши запросы, в разных вариациях
    * при тестировании методов `create`, `update`, `delete` используем `Transactional` и `EntityManager` который методом `flush()` сбрасывает все изменения в базе данных.
    
>Подробнее по тестированию Spring MVC читаем [здесь](https://spring.io/guides/tutorials/bookmarks/#_testing_a_rest_service), 
> [здесь](https://www.petrikainulainen.net/programming/spring-framework/unit-testing-of-spring-mvc-controllers-rest-api/) и [здесь](https://www.petrikainulainen.net/programming/spring-framework/unit-testing-of-spring-mvc-controllers-normal-controllers/), а так-же в книге "Spring 4 для профессионалов", которая прилагается в доп.материалах к тестовому заданию.

При тестировании метода `update` контроллера `BookController` обнаружилось что он (метод) работает неправильно. По сути он делает тоже что и метод `create`.

#### Исправляем метод BookController.update() 
* В сервис [Service.BookService](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java) пишем новый метод `Book update()`;
* Реализуем его в классе [Service.BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java);
* Вносим изменения в метод [BookController.update()](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java).
* Пишем модульный тест в [BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java) для метода `update`

[Итог](https://github.com/vladmeh/javaRushTestTask/blob/222d44b4ffc64f555455192595af58fba8430ca4)

### 9. Поиск
* Расширяем [BookRepository](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Repository/BookRepository.java) 
    * добавляем методы, в кторых с помощью `@Query` директивы делаем запросы к базе данных.
    > подробнее читаем [Spring Data JPA Tutorial: Introduction to Query Methods](https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-introduction-to-query-methods/)
* Добавляем методы поиска в [BookService](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Service/BookService.java)
* Реализуем эти методы в [BookServiceImpl](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Service/BookServiceImpl.java)
* Добавляем метод `search` в контроллер [BookController](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/main/java/com/vladmeh/javaRushTestTask/Controller/BookController.java)

Теперь по запросу в браузере `http://localhost:8080/books/search` с параметрами мы можем производить поиск, сортировать, фильтровать наши данные, например по `http://localhost:8080/books/search?term=java&afterYear=2016&order=desc` будет выведен список книг, которые в заголовке, описаниии или авторе содержат слово "java", напечатанные после 2016 года и отсортированные по убыванию.

* Тестируем
    * добавляем модульный тест в [BookServiceTest](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/test/java/com/vladmeh/javaRushTestTask/Service/BookServiceTest.java)
    * добавляем интеграционные тесты в [BookControllerTest](https://github.com/vladmeh/javaRushTestTask/blob/ede3e501f82de66a152d0622a139aa6ca1ef6768/src/test/java/com/vladmeh/javaRushTestTask/Controller/BookControllerTest.java)
    
На [данном этапе](https://github.com/vladmeh/javaRushTestTask/tree/ede3e501f82de66a152d0622a139aa6ca1ef6768) мы имеем полноценное (простое) CRUD приложение которое может по нашим запросам возвращать нам данные в формате JSON.

### 10. Создание первого представления

Для реализации визуального представления нашего приложения я использовал
* [Thymeleaf](http://www.thymeleaf.org/) - шаблонизатор HTML
* Фреймворк [Bootstrap v.3](https://getbootstrap.com/docs/3.3/)

* Добавляем в pom.xml зависимости Maven для представления
    * ```xml
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.webjars/bootstrap -->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>bootstrap</artifactId>
                <version>3.3.7-1</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.webjars/webjars-locator -->
            <dependency>
                <groupId>org.webjars</groupId>
                <artifactId>webjars-locator</artifactId>
                <version>0.32-1</version>
            </dependency>
        </dependencies>
      ```
* Пишем основной шаблон default.html для нашего приложения;
* Пишем шаблон для домашней страницы index.html;
* Изменяем контроллер Controller.HomeAction;
* Тестируем
    * Проверяем визуально
        * На главной странице http://localhost:8080 должен отображаться текст `Hello, World!` в виде заголовка h1;
        * Если добавить параметр name в запросе, например `http://localhost:8080?name=Vlad`, будет отображаться текст `Hello, Vlad!`;
        
    * Запускаем тесты ApplicationTest и HttpRequestTest.
    
> Для построения шаблонов я также испоьзую [Thymeleaf Layout Dialect](https://ultraq.github.io/thymeleaf-layout-dialect/) который позволяет создавать макеты и шаблоны многократного использования.

Чтобы в дальнейшем иметь возможность получать данные в JSON формате, сохранить наши интеграционные тесты и не иметь конфликтов с url, я переименовал BookController -> BookRestController

* Переименовываем контроллер в IJ Idea с помощью `Shift + F6`, что даст нам предложение переименовать упоминание контоллера во всех классах приложения.
    * Автоматически переименуется `BookControllerTest -> BookRestControllerTest`
* В BookRestController меняем `path = "/books/api"`;
* В BookRestControllerTest меняем все пути `/books/**` на `books/api/**`;
* Запускаем BookRestControllerTest и BookServiceTest, все тесты должны проходить.

### 11. Предаствление для списка книг

* Создаем новый BookController
    * Пишем метод viewBooksList    
* Создаем шаблон для списка book/list.html
* Тестируем
    * Визуально по запросу http://localhost:8080/books мы можем видеть список первых десяти книг нашего списка в таблице.
    * По запросу http://localhost:8080/books?page=2 - вторую страницу нашего списка.
    * Пишем интеграционные тесты BookControllerTest
        * Тестируем ответ сервера на наш запрос без параметров
        * Тестируем ответ сервера на наш запрос с параметрами
          



