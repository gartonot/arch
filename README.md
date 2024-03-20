# Архитектура приложения

> Public API - публичный интерфейс, способ изолирования и инкапсуляции файлов существует папка и все файлы/папки внутри изолированы извне и строго запрещены для использования, чтоб они могли быть использованы где-то извне они импортируются внутрь `index.*` файла и уже оттуда имеют доступ извне

## App.vue 

Корень приложения, в нём по хорошему указать layout чтоб легко манипулировать различными шаблонами, но на данном этапе они пока что не нужны, но должна быть возможность легко их интегрировать

## main.ts 

Файл с настройкой и подключением всех зависимостей, регистрации компонентов, стилей, директив и прочего, внутри минимум логики, это файл основной и  должен быть легко читаем, поэтому внутри лишь подключения внешних вещей (например директивы и всё что с ними связано подключать из папка `@/services/directives/index.ts` - и так буквально с каждым файлом, если нужно зарегистрировать компоненты глобально, для этого отдельный файл который будет подключаться)
При таком подходе основной файл будет в разы чище и понятнее при просмотре проекта, будь это новый сотрудник или уже бывалый, легкочитаемость и изолированность сгруппированых вещей


## /assets

Содержит в себе ресуры и это как правило стили и изображения (изображения так же могут лежать в папке `../public`)

### /assets/images 
>  - Изображения могут быть только `.webp`!

### /assets/vectors
> - Содержит в себе всю векторную графику в соответствующих папках \
> - `icons` - для иконок (всё что мы можем покрасить добавив currentColor) \
> - `images` - для векторых картинок, которым не требуется менять стиль, они просто векторные

### /assets/styles 
! Все файлы начинающиеся с нижнего подчёркивания считаются приватными и изолированы и доступны только внутри папки 

> - Содержат все стили для проекта которые указаны глобально
> - `./index.scss` - основной файл в котором прописаны глобальные стили и могут импортироваться прочие стили рядом стоящих папок и файлов и в дальнейшем этот файл уходит в `main.ts` (альтернативное название для файла - `global.scss`)
> - `./media.scss` - Файл с медиа запросами, написан с использованием миксинов, содержит в себе и переменные для брекпоинтов и сами миксин медиа запросов(импортируется глобально)
> - `./variables.scss` - Переменные проекта: цвета, размеры шрифтов, переменные для сетки и в целом глобальные какие-то переменные
> - `./libs` - папка с стилями из прочих библиотек, принцип подключения public api
> - `./settings` - папка с содержанием каких-то настроек, таких как обнуление стилей или их нормализация, создание утилит
> - `./ui/index.scss` - Если потребуется, то сюда описывать стилистику глобальных ui элементов - 1 ui элемент 1 файл (`button.scss`, `drawer.scss`) и в дальнейшем импортироват их в корень `./ui/index.scss`, а этот файл уже подключать в `index.scss` (`@import "./ui/index.scss"`) - для чистоты и прозрачности кода

## /directives
> - В папке содержаться все директивы, подключаемые внутрь файла `/directives/index.ts` и в дальнейшем импортированы в `main.ts`

## /ui
> - В папке содержаться максимально самостоятельные компоненты, которые не имеют бизнес логики и отвечают лишь за отображение (кнопки, поля ввода, дропдауны, селекты и др.), группировать или как-то их связывать строго запрещено, компоненты никаким образом не связанны ни с чем, если это 10 кнопок - то это 10 файлов с припиской Button

## /components 
> - В папке содержиться совокупность `ui` элементов и вёрстки, в них тоже нет особой логики или она минимальна (Пример: `<ApprovalBlock>`, `<UserAvatar>`)
> - Может использовать внутри себя только UI компоненты. Использование внутри самих себя (что-то из папки `components`) или выше лежащих (`modules` и `pages`) - строго запрещено!

## /modules
> В папке содержиться совокупность `components`, `ui` и вёрстки, так же присутствует различная бизнес логика (Пример: `<OrderList />`, `<OrderView />`)
> - Может использовать внутри себя только `components`, `ui`. Использование внутри самих себя (что-то из папки `modules`) или выше лежащих (`pages`) - строго запрещено!

## /pages 
> - В папке содержиться совокупность `modules`, `components`, `ui`, но по хорошему, чтоб страница - была максимально тонкой прослокой, без логики, максимум какая-то конфигурация на всю страницу или включить или выключить какой-то модуль 
```html
<template>
    <DesktopMap />
    <DesktopOrderList />
    <DesktopEmployeeTracker />
</template>
```
## /constants

> - В папке содержиться все константы, которые могут пригодиться для проекта, так же тут содержаться все дефолтные значения, так как они константны.
Способ подключения `public api`

## /plugins
> - В папке содержиться все плагины, все подключения и регистрации выполнять внутри внутри корня папки `/plugins/index.ts` с импортом и на экспорт отдавать функцию, которая будет инициализаровать все плагины уже внутри файла `main.ts`.
Способ подключения `public api`

## /router 
> - В папке содержиться вся настрйока роутера, какие-то внешние настройки - это отдельные файлы внутри папки `/router`, но в конечном итоге на выход идёт один файл `index.ts` - и этот файл уже подключается внутри файла `main.ts`.
Способ подключения `public api`

## /services
! Папка для обозначения всех утилит, директив, библиотек, типизации, запросов, свалка "дополнений/услуг" для всего приложения, отдельно изолированная логика

### /services/enums
> - Папка для Enum. Способ подключения `public api`


### /services/interfaces
> - Папка для Interface. Для каждой страницы должна быть своя папка с интерфейсами(`../interfaces/OrderView/`, `../interfaces/Desktop`). Все интерфейсы импоритруются внутрь главного файла своей папки для страницы, а дальше в компонентах импортируем только нужную папку для страницы (`@imports "@/services/interfaces/Desktop/index.ts"`)

### /services/libs
> - Папка для все библиотек и дополнений, структура папок должна полностью дублировать `/pages/` если нужны билоитеки для этой страницы, внутри каждой билиотеки сущетсуют изолированные енумы, хелперы, интерфейсы и типы. Способ подключения `public api`

### /services/repositories/api
> - Папка содержит в себе все запросы к бекенду

### /services/repositories/api
> - Папка содержит в себе все контракты (В будущем ушедшие в папку `/services/interfaces/DTO/index.ts`)

### /services/types/
> - Папка для Type. Содерижт в себе все типы. Все типы импоритруются внутрь главного файла своей папки для страницы, а дальше в компонентах импортируем только нужную папку для страницы (`@imports "@/services/types/Desktop/index.ts"`)


### /servies/utils/convertos 
> - Папка для конверторов. Способ подключения `public api`

### /servies/utils/use 
> - Папка для use функций. Способ подключения `public api`

### /servies/utils/validators 
> - Папка для use валидаторов. Способ подключения `public api`

## /store
> - В папке лежат все необходимые сторы, разделённые на модули (создаём папку с названием стора и суфиксом `-store-module`, внутри лежат всегда файлы `index.ts`, `state.ts`, `actions.ts`, `getters.ts`, что позволит изолировать модуль). Дальше все модули подключаются внутри файла `/store/index.ts` и доступны извне только отсюда.
Способ подключения `public api`

## /translates
> - В папке содержаться все переовды, разделённые по папка локалей (`en`, `ru`)
