# articles-searcher-service

Для установки, выполните в консоли следующие команды:

- **git clone https://github.com/Hennos/articles-searcher-service.git**
- **cd articles-searcher-service**
- **npm install**
- **npm start**

Сервер по умолчанию будет доступен адресу http://localhost:3000.

Сервис поддерживает следующие запросы:

- **GET http://{домен}:{порт}/{api}/article/{doi}** - поиск метаданных статьи по её DOI
- **GET http://{домен}:{порт}/{api}/articles?{параметры_запроса}** - поиск метаданных статей через поисковый запроc. Параметры могут быть скомбинированы через &.

В настоящее время сервис поддерживает следующие API:

| API                                                                      | Именование |
| ------------------------------------------------------------------------ | ---------- |
| [CrossrefAPI](https://api.crossref.org)                                  | crossref   |
| [ScopusAPI](https://api.elsevier.com/content/search/scopus)              | scopus     |
| [Article Match Retrieval (Web of Science)](http://support.clarivate.com) | wos        |
| [РИНЦ](https://elibrary.ru)                                              | risc       |

Через **default.json** в папке **/config** можно настроить работу приложения. Для этого доступны следующие опции:

- **port** - номер порта, который будет прослушиваться сервером
- **crossref** - параметры сервиса для доступа к CrossrefAPI
- **scopus** - параметры сервиса для доступа к ScopusAPI
- **wos** - параметры сервиса для доступа к Article Match Retrieval
- **risc** - параметры сервиса для доступа к РИНЦ

Параметры сервиса для работы с CrossrefAPI:

- **rows** - размер поисковой выдачи
- **agentName** - имя приложения, использующего CrossrefAPI (необходимо указать)
- **agentVersion** - версия приложения, использующего CrossrefAPI (необходимо указать)
- **mailTo** - почта для связи (необходимо указать)

Параметры сервиса для работы с ScopusAPI:

- **rows** - размер поисковой выдачи
- **apiKey** - ключ для авторизации в ScopusAPI (необходимо указать)

Параметры сервиса для работы с Article Match Retrieval:

- **rows** - размер поисковой выдачи
- **credentials** - данные аккаунта для авторизации:
  - **username** - ID пользователя
  - **password** - пароль от аккаунта

Параметры сервиса для работы с РИНЦ:

- **userCode** - код доступа к API РИНЦ

В зависимости от API доступны следующие параметры запроса:

Для CrossrefAPI:

| Наименование параметра | Описание                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `query`                | Query `container-title` aka. publication name                                                                       |
| `author`               | Query author given and family names                                                                                 |
| `bibliographic`        | Query bibliographic information, useful for citation look up. Includes titles, authors, ISSNs and publication years |

Для ScopusAPI:

| Наименование параметра | Описание                                                                                                  |
| ---------------------- | --------------------------------------------------------------------------------------------------------- |
| `title`                | Contains the English or non-English article or chapter title                                              |
| `authors`              | Contains the names of the authors of the document                                                         |
| `keywords`             | Permits keyword searches for documents. This field accepts phrases and can be used with Boolean operators |
| `affiliation`          | Contains the institutional or corporate address of the article's authors                                  |

Для Article Match Retrieval:

| Наименование параметра | Описание                                                     |
| ---------------------- | ------------------------------------------------------------ |
| `title`                | Title of the article                                         |
| `authors`              | List type element containing one or more author name values. |
| `year`                 | Cover date year.                                             |
| `issn`                 | ISSN or eISSN. Be sure to include the hyphen.                |

Для РИНЦ:

| Наименование параметра | Описание                  |
| ---------------------- | ------------------------- |
| `authorid`             | Идентификатор автора      |
| `titleid`              | Идентификатор публикации  |
| `orgid`                | Идентификатор организации |
| `year`                 | Год публикации            |

Для ScopusAPI доступны дополнительные методы:

- Список авторов с аффилиациями: **GET http://{домен}:{порт}/scopus/affiliations/{doi}**
- Список авторов с полными данными аффилиаций: **GET http://{домен}:{порт}/scopus/full/affiliations/{doi}**
- Поиск цитирующих статей по DOI работы: **GET http://{домен}:{порт}/scopus/citing?doi={doi}&start={смещение}**

Для CrossrefAPI доступны дополнительные методы:

- Список авторов с аффилиациями: **GET http://{домен}:{порт}/crossref/affiliations/{doi}**

Для РИНЦ доступны дополнительные методы:

- Поиск авторов **/authors?{параметры запроса}**, где параметры запроса:

| Наименование параметра | Описание                            |
| ---------------------- | ----------------------------------- |
| `spin`                 | SPIN автора                         |
| `authorid`             | Идентификатор автора                |
| `name`                 | Полное имя автора                   |
| `lastName`             | Фамилия автора                      |
| `firstName`            | Имя и отчество автора               |
| `initials`             | Инициалы автора                     |
| `country`              | Страна                              |
| `countryCode`          | Код страны                          |
| `city`                 | Город                               |
| `orgName`              | Название организации - место работы |

- Поиск авторов, принадлежащих организации **/orgAuthors/{идентификатор организации}**

- Получение данных аналитики о авторе **/author/{идентификатор автора}**

В случае возникновения ошибки при выполнении запроса к базе данных, сервис может вырнуть информационное сообщение в формате:

**{КОД_ОШИБКИ}: {Сообщение с информацией об ошибке}**

Коды ошибок:

- **DATA_API_NOT_FOUND_DATA** - на сервисе базы данных не обнаружены искомые данные
- **DATA_API_BAD_REQUEST** - неправильно сформирован запрос к сервису базы данных
- **DATA_API_REJECT_REQUEST** - сервис базы данных не смог корректно обработать поступивший запрос
- **DATA_API_UNAVAILABLE** - сервис базы данных в настоящее время недоступен
