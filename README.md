# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/styles/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```
# Архитектура проекта
## Особенности архитектуры:
- Элементы отображения создают события при вызове стандартных событий (например, клик).
- Брокер событий подписывается на кастомные события и вызывает необходимые функции.
- Классы в пределах проекта не связаны друг с другом и взаимодействуют только через передаваемые аргументы.
# Базовый код
## Компоненты отображения
- Отображают визуальную составляющую конкретных элементов страницы.
- Устанавливают события.
- Размещают данные в дочерних элементах.
## Компоненты обработки данных (папка components/data):
- Обрабатывают данные, используемые для отображения.
- Кэшируют данные заказа, корзины, карточки, пришедшие с сервера.
- Позволяют в удобном формате вывести и обработать данные.
## Функции обработки событий (папка components/handler):
- Выполняют действия в ответ на события.
- Часто запускают отрисовку конкретных модальных окон.
## Базовые функции (папка base):
- Расширяют функционал стандартных функций JavaScript.
- Используются во всех компонентах.
## API для выполнения запросов к серверу (файл cardAPI):
- Позволяет выполнять запросы к серверу для получения данных о карточках.
--------------------------------------------------------------------------
# Классы для работы с DOM:
* Form — базовый класс для форм, реализующий интерфейс IForm.
* Order — класс, отображающий форму заказа (способ оплаты, адрес).
* Contacts — класс, отображающий форму контактов (e-mail, телефон).
* Success — класс, отображающий элемент успешного оформления заказа.
# Интерфейсы:
* IForm — описывает общие методы и свойства для классов форм.
# Классы для работы с данными:
* OrderData — класс, работающий с данными заказа.
* BasketData — класс, работающий с данными корзины.
* CardData — класс, кэширующий данные карточек товаров.
#Интерфейсы для классов данных:
* IOrder — описывает структуру данных заказа.
* IBasketData — описывает методы и свойства класса, работающего с корзиной.
* ICardData — описывает методы и свойства класса, работающего с данными карточек товаров.
# Функции для отображения модальных окон:
* showPreview — показывает превью товара.
* openBasket — показывает корзину.
* makeOrder — показывает форму заказа (способ оплаты, адрес).
* makeContacts — показывает форму контактов (e-mail, телефон).
* showSuccess — показывает элемент успешного оформления заказа.
# Файл index.ts:
Создает все необходимые элементы для работы сайта (страница, галерея, корзина, модальное окно и классы для работы с данными).
Подписывает глобальный брокер событий на все кастомные события.
Получает данные о карточках товаров с сервера.
# Общая структура проекта:
Проект организован следующим образом:
- Классы для работы с DOM находятся в файле form.ts.
- Классы для работы с данными находятся в файлах order-data.ts, basket-data.ts и card-data.ts.
- Функции для отображения модальных окон находятся в файле modal.ts.
- Файл index.ts является точкой входа в приложение и отвечает за инициализацию всех необходимых компонентов.

# 1. Класс Api

Базовый класс для работы с API:

get(uri: string);
post(uri: string, data?: object, method?: 'POST' | 'PUT' | 'DELETE');
handleResponse(response: Response);

* Методы get(uri: string) и post(uri: string, data: object, method: ApiPostMethods = 'POST') осуществляют базовые get и post запросы к серверу
* Метод handleResponse(response: Response): Promise<object> - осуществляет обработку пришедшего ответа с сервера (парсинг JSON, обработка ошибки)

type ApiListResponse<Type> = {
    total: number,
    items: Type[]
};

type ApiOrderResponse<Type> = {
    total: number,
    id: string
}

* ApiListResponse и ApiOrderResponse - типы ответов сервера по запросам product и order

# 2. Класс CardAPI

Класс для работы с данными о карточках и заказах, реализующий интерфейс ICardAPI:

interface ICardAPI {
  getCards(): Promise<ICard[]>;
  sendOrder(order: IOrder): Promise<IOrderResult>;
}

* getCards - получает с сервера информацию о карточках товаров
* sendOrder - отправляет готовый заказ на сервер

# 3. Класс EventEmitter

Класс-событийный брокер:

interface IEvents {
  on<T extends object>(event: string, callback: (data: T) => void): void;
  emit<T extends object>(event: string, data?: T): void;
  trigger<T extends object>(event: string, context?: Partial<T>): (data: T) => void;
}

* on - подписывает элемент на конкретное событие в EventEmitter
* onAll(callback: (event: EmitterEvent) => void) - подписывает элемент на все события в EventEmitter
* off(eventName: EventName, callback: Subscriber) - отписывает элемент с события в EventEmitter
* offAll() - сбрасывает все события в EventEmitter
* emit - запускает событие в EventEmitter
* trigger - создает коллбек триггер, генерирующий событие при вызове

# 4. Класс Component

Базовый класс для компонентов с общими методами работы с DOM:

* setText(value: string): void; - устанавливает текст компоненте или альтернативный текст в изображении
* setLink(value: string): void; - устанавливает ссылку в изображении или в ссылочном компоненте
* getInputValue(): string; - получает значение из поля ввода
* addClass(className: string): void; - добавляет класс компоненту
* removeClass(className: string): void; - удаляет класс с компонента
* hasClass(className: string): boolean; - проверяет наличие класса у компонента
* toggleClass(className: string, state?: boolean): void; - переключает класс в компоненте
* toggle(modifier: string, state?: boolean): void; - переключает класс по модификатору в компоненте
* disable(): void; - делает кнопку неактивной
* enable(): void; - делает кнопку активной
* toggleDisabled(state?: boolean): void; - переключает состояние активации кнопки
* isValid(): boolean; - проверяет валидацию поля ввода
* getValidationMessage(): string | undefined; - получает сообщение об ошибке валидации поля ввода
* getAttribute(value: string): string; - получает по имени атрибут компонента
* remove(): void; - удаляет элемента из DOM
* clear(): void; - удаляет контент компонента
* setContent(item?: ContentValue): void; - устанавливает контент компоненту
* append(...items: ContentValue[]): void; - вставляет дочерние элементы в конец родительского
* prepend(...items: ContentValue[]): void; - вставляет дочерние элементы в начало родительского
* replace(...items: ContentValue[]): void; - заменяет контент в компоненте
* on<T extends object>(eventName: string, handler: (data: T) => void): void; - перезаписанное базовое свойство с возможностью вызова по цепочке
* bindEvent(sourceEvent: string, targetEvent?: string, data?: object): void; - устанавливает кастомное событие на компонент
* bindEmitter(emitter: EventEmitter): void; - присваивание EventEmitter компоненту
* bem(element?: string, modifier?: string): { name: string; class: string };  - перезаписывает базовое свойство с указанием имени родительского компонента в БЭМ нотации
* element<T extends Component<any>>(name: string, mode?: elementMode): T; - поиск компонента среди дочерних
* protected assign(data?: object): void; - присваивание свойств доступных для записи компоненту
* render<DataType extends object>(data?: DataType): NodeType; - вспомогательный метод возвращающий компонент и присваивающий свойства
* protected static factory<T extends Component<any>>(this: new (el: unknown, name?: string, emitter?: EventEmitter) => T, el: unknown, data?: any, name?: string, emitter?: EventEmitter): T - фабричный метод создания компонента
* static clone<T>(templateId: string, data?: any, name?: string, emitter?: EventEmitter): T - клонирование компонента из шаблона
* static mount<T>(selectorElement: string, data?: any, name?: string, emitter?: EventEmitter): T - создание компонента из элемента существующего на странице (не в шаблоне)
* init() - базовая конфигурация компонента
* readonly name: string - имя компонента
* protected node: NodeType; - HTML-элемент компонента
* protected elements: Record<string, Component<NodeType>>; - кэш дочерних компонентов
* protected events: EventEmitter; - брокер событий компонента
* readonly fieldNames: string[]; - поля доступные для присваивания у компонента

# Типы
type ApiListResponse<Type> = {
  total: number;
  items: Type[];
};

type ApiOrderResponse<Type> = {
  total: number;
  id: string;
};

enum elementMode {
  parent = 'parent',
  children = 'children',
}
