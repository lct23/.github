## HR Zero – проект для хакатона Лидеры Цифровой Трансформации (Краснодар) 2023

### Что успели сделать за 192 часа!

- 🎉 Бэкенд и фронт сервиса проведения онбординга сотрудников.
- 🌏 Интерфейс для добавления отделов сотрудников в базу данных
- 📕 Редактор документов.
- ✅ Редактор опросов.
- 💾 Две обучающие игры, чтобы знакомить новичка с коллективом
- 🧨 Server-side рендеринг на фронте, для лучшего SEO!

## Репозиторий

* https://github.com/lct23/krasnodar

## Стек технологий

* Бэкенд и вся бизнес-логика на Common Lisp.
* На фронтенде TailwindCSS + немного AlpineJS.
* База данных - PostgreSQL в облаке.
* Resend.com - для отправки email уведомлений.
* Развёртывание в Docker.

## Архитектура MVP

Для простоты и скоростиi MVP реализован в виде одного сервиса связанного с базой.
Мы использовали server-side рендеринг для того, чтобы всю бизнес-логику можно было
реализовать на фронтенде и не требовалось много JavaScript кода.

Так же, для простоты задачи которые должны выполняться по расписанию, запускаются тем же бэкендом.

```mermaid
graph TD;
   subgraph backend
   Passport
   click Passport "https://github.com/lct23/backend/tree/master/passport" _blank
   Events
   click Events "https://github.com/lct23/backend/tree/master/events" _blank
   Rating
   click Rating "https://github.com/lct23/backend/tree/master/rating" _blank
   Images
   click Images "https://github.com/lct23/backend/tree/master/image-store" _blank
   end
   
   subgraph frontend
   Adminka;
   click Adminka "https://github.com/lct23/backend/tree/master/admin" _blank
   Frontend;
   click Frontend "https://github.com/lct23/frontend" _blank
   end
   
   subgraph cloud
   ElasticSearch(((ElasticSearch)))
   DB[(Postgres)]
   S3[[S3]]
   end
   
   Frontend --> Passport
   Frontend --> Events
   Frontend --> Rating
   Frontend --> Images
   
   Adminka --> Passport
   Adminka --> Events
   Adminka --> Rating
   
   Events --> ElasticSearch
   Events --> DB
   Passport --> DB
   Rating --> DB
   Images --> S3
```

Общение микросервисов между собой и с фронтендом идёт по JSON-RPC, что позволяет быстро добавлять необходимый функционал, не тратя время на попытки вписать его в ограничения HTTP. Кроме того, этот протокол так же может быть использован поверх Websocket, для создания более динамичных фичей сайта.

Фронтенд использует React.JS и Next.JS для server-side рендеринга. Это должно облегчить поисковую оптимизация и обеспечит сайту приток новых пользователей из поисковых систем типа Яндекс и Google.

Админка тоже использует server-side рендеринг, но построена на базе другого фреймворка - Reblocks, потому что в нём хорошо ориентируется наш бэкендер. Этот фреймворк позволяет быстро проверять идеи и реализовывать фичи, потому что всю бизнес-логику можно описывать на бэкенде, с использованием языка программирования Common Lisp.

И микросервисы, и фронтенд собираются в Docker контейнеры и могут быть развёрнуты в любом "облаке".

