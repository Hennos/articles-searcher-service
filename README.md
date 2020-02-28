# articles-searcher-service

Для установки, выполните в консоли следующие команды:

- **git clone https://github.com/Hennos/articles-searcher-service.git**
- **cd articles-searcher-service**
- **npm install**
- **npm start**

Сервер по умолчанию будет доступен адресу http://localhost:3000.

Сервис поддерживает следующие запросы:

- **GET http://{доменное_имя}:{номер_порта}/{api}/article/{doi}** - поиск метаданных статьи по её DOI
- **GET http://{доменное_имя}:{номер_порта}/{api}/articles?{параметры_запроса}** - поиск метаданных статей через поисковый запроc. Параметры могут быть скомбинированы через &.

В настоящее время сервис поддерживает следующие API:

| API                                                         | Именование             |
| ------------------------------------------------------------| ---------------------- |
| [CrossrefAPI](https://api.crossref.org)                     | crossref               |
| [ScopusAPI](https://api.elsevier.com/content/search/scopus) | scopus                 |

Через **default.json** в папке **/config** можно настроить работу приложения. Для этого доступны следующие опции:

- **port** - номер порта, который будет прослушиваться сервером
- **crossref** - параметры сервиса для доступа к CrossrefAPI
- **scopus** - параметры сервиса для доступа к ScopusAPI

Параметры сервиса для работы с CrossrefAPI:

- **rows** - размер поисковой выдачи
- **agentName** - имя приложения, использующего CrossrefAPI (необходимо указать)
- **agentVersion** - версия приложения, использующего CrossrefAPI (необходимо указать)
- **mailTo** - почта для связи (необходимо указать)

Параметры сервиса для работы с ScopusAPI:

- **rows** - размер поисковой выдачи
- **apiKey** - ключ для авторизации в ScopusAPI (необходимо указать)

В зависимости от API доступны следующие параметры запроса:

Для CrossrefAPI:

| Наименование параметра  | Описание                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `query`                 | Query `container-title` aka. publication name                                                                       |
| `author`                | Query author given and family names                                                                                 |
| `bibliographic`         | Query bibliographic information, useful for citation look up. Includes titles, authors, ISSNs and publication years |

Для ScopusAPI:

| Наименование параметра  | Описание                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `title`                 | Contains the English or non-English article or chapter title                                                        |
| `authors`               | Contains the names of the authors of the document                                                                   |
| `keywords`              | Permits keyword searches for documents. This field accepts phrases and can be used with Boolean operators           |
| `affiliation`           | Contains the institutional or corporate address of the article's authors                                            |
