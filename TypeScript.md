1) [[#Как работает TypeScript]]
2) [[#Отличия в типах JS и TS]]
3) [[#any vs unknown]]
4) [[#Автоматическое выведение (type inference)]]
5) [[#Сужение типов и type guards]]
6) [[#Invariant]]
7) [[#Исправь типы]]
8) [[#Интерфейсы]]
9) [[#Напиши интерфейс для объекта]]
10) [[#Напиши type guard]]
11) [[#Void vs undefined]]
12) [[#strictNullChecks]]
13) [[#Вид типизации в TypeScript]]
14) [[#Type assertion]]
15) [[#Объединения (unions) и пересечения (intersections)]]
16) [[#Типы]]
17) [[#Массивы]]
18) [[#Функции]]
19) [[#Енамы]]
20) [[#Never и exhaustive check]]
21) [[#Функция возвращающая never]]
22) [[#Литеральные типы]]
23) [[#Readonly]]
24) [[#Опциональные поля]]
25) [[#As const]]
26) [[#Типизация const и let]]
27) [[#Discriminated unions]]
28) [[#Напиши discriminated union]]
29) [[#typeof и keyof]]
30) [[#Indexed Access Types]]
31) [[#Создай конфигурационный объект]]
32) [[#Индексные сигнатуры]]
33) [[#Mapped types]]
34) [[#Тип на основе объектов]]
35) [[#Дженерики]]
36) [[#Универсальный интерфейс]]
37) [[#Типизируй reduce]]
38) [[#Утилитарные типы]]
39) [[#Типизируй объект]]
40) [[# Напиши кастомный Record]]
41) [[#Условные типы (conditional types)]]
42) [[#Напиши условный тип]]
43) [[#Дистрибутивные условные типы]]
44) [[#Напиши кастомный Exclude]]
45) [[#infer]]
46) [[#Напиши кастомный ReturnType]]
47) [[#Напиши Omit и Pick]]
48) [[#Напиши Partial и Readonly]]
49) [[#Сложный кастомный тип]]
50) [[#Key remapping]]
51) [[#Переверни тип]]
52) [[#Классы]]
53) [[#Абстрактные классы]]
54) [[#Наследуй абстрактный класс]]
55) [[#Сужение/расширение доступа в классах]]
56) [[#d.ts]]
57) [[#Типизируй код]]
58) [[#satisfies]]
59) [[#Проверь с помощью satisfies]]
# Как работает TypeScript
Как TypeScript выполняется в браузере?
Что происходит с типамм при компиляции?

Как TypeScript выполняется в браузере - в браузере не выполняется TS, только JS.
TS - это надстройка над JS созданная исключительно для разработчика. 
Чтобы браузер выполнил код - он должен быть вначале скомпилирован из TS кода.

При компиляции все типы удаляются, таким образом получается чистый JS код. 

# Отличия в типах JS и TS
Какие 4 новых типа вводит TypeScript?
Какой "подвид" массивов вводит TypeScript? В чём его особенность?

Типы в JS: 
1) string
2) number
3) boolean
4) undeined
5) bigint
6) symbol
7) object
8) null

4 новых типа в TS:
1) any - отключает проверку типов, может быть чем угодно.
2) unknown - типобезопасный тип. может содержать любой тип, как any, но его нельзя использовать без проверки типа (без проверки использовать нельзя)
```ts
let str: unknown = "kek"
if(typeof str === "string" ) {...}
```
3) never - функция никогда не возвращает управление (возвращает ошибку или уходит в бесконечный цикл)
```ts
function func(){throw new Error("lol")}
```
4) void - означает отсутствие возвращаемого значения.
```ts
function func(){console.log('lel')}
```

>	Кортеж (Tuple) - массив фиксированной длины, где каждый элемент имеет свой определённый тип, зависящий от его позиции.

```ts
let types [string,number,boolean] = ['kek',2,false]
```

# any vs unknown
Чем any принципиально отличается от unknown?

1) any - отключает проверку типов, может быть чем угодно.
2) unknown - типобезопасный тип. может содержать любой тип, как any, но его нельзя использовать без проверки типа (без проверки использовать нельзя)
```ts
let str: unknown = "kek"
if(typeof str === "string" ) {...}
```

# Автоматическое выведение (type inference)
В каких случаях компилятор выводит тип автоматически?

- При инициализации
```ts
let name = 'Loh' // string
const arr = ['lel',3,null]; // [string, number, null]
const obj = {a: 'puk', b: undefined};// {a: string, b: undefined}
```
- При возвращении значения функцией
	- при наличии достаточного контекста (указаны все типы параметров)
```ts
function sum(a:number, b:number) {return a+b} // number
```
	- При указании значений по умолчанию 
```ts
function sum(a=3, b=4) {return a+b} // number
```

# Сужение типов и type guards
1. Что такое сужение типов? Зачем оно нужно?
2. Что такое type guards?
3. Какие три вида базовых type guard'ов есть в TypeScript?
4. Как сделать кастомный type guard?
5. Что изменится, если добавить к кастомному type guard'у asserts в возвращаемом типе?

>Сужение типов - процесс при котором TS постепенно уточняет тип переменной постепенно переходя от более общего к более конкретному на основе проверок в коде.

Сужение типов позволяет определить конкретный тип и использовать его особенности
```ts
function func(atr: string|number){
	console.log(atr.length) // ошибка, atr может быть number
	if(typeof atr === 'string' ) console.log(atr.length) //сужение типов с помощью проверки
}
```

>Type guards - проверки которые гарантированно определяют конкретный тип переменной.

Вот базовые type guardы (typeof, instanceof, in)
```ts
if(typeof atr === 'string' ) console.log(atr.length) // имеет ли переменная тип string
if(atr instanceof Date ) console.log(atr.timestamp) // является ли объект экземпляром класса Date
if("name" in user ) console.log(user.name) // проверяет наличие поля "name" в объекте user
```

Создание и использование кастомных TG
```ts
// при создании TG используется выражение is - это предекат типа
// запись TGFunc(): value is type означает - если TGFunc вернула true то value имеет тип type. Используется для проверки типа и ветвлении кода при необходимости.
function isString (value: unknown): value is string {
	return typeof value === "string"
}

function isUser(obj: unknown): obj is User{
	return typeof obj === "object"  
	&& typeof (obj as User).name === "string"
	&& typeof (obj as User).age === "number"
}
// Использование 
const str = 'puk';
if(isString(str)) { console.log(str.length)};

// если использовать запись TGFunc(): asserts is type то проверка на тип становится утверждением с последующей проверкой. Такой TG ничего не возвращает, но может пробросить ошибку. Используется для гарантировании что на вход придёт нужные тип. 
function assertIsString (value: unknown): asserts value is string {
	if( typeof value !== "string") {
		throw new Error(`Expected string, got ${typeof value}`)
	}
}

//Использование
function func(kek: unknown){
	assertIsString(kek)
	
	console.log(kek.length)
}
```

# Invariant
1. Что такое invariant функция? Чем она отличается от type guard?
2. Как сделать ivariant-функцию?

> Инвариант(invariant) - функция, которое принимает условие и пробрасывает ошибку, если условие не выполняется.

В отличает от type guard, который позволяет коду выполнятся дальше, если полученная переменная не является ожидаемым типом, invariant- бросает ошибку, если проверка на соответствие типу указанной переменной провалилось и код дальше не выполняется.

```ts
//Создание
function invariant(condition:unknown, errorMsg:string): asserts condition {
	if(!condition) throw new Error(errorMsg)
}

//Использование
function func(usr: unknown){
	invariant(isUser(usr),"expected type User")
	
	console.log(usr.name)
}

// Пример типизированного инварианта(добавился is type)
function invariantIsString(value: unknown, message: string): asserts value is string {
    if (typeof value !== "string") {
        throw new Error(message);
    }
}
```

# Исправь типы
```ts
// Код сейчас вызывает ошибки типизации.
// Как его доделать, чтобы TypeScript не ругался, но при этом логика повторения
// для строк и умножения для чисел сохранилась?

function getDoubledId(id: string | number) {
  if (typeof id === "string") return id.repeat(2);
  return id*2;
}

const id1: string | number = "abc";
const id2: string | number = 5;

const doubled1 = getDoubledId(id1);
const doubled2 = getDoubledId(id2);

console.log(doubled1); // "abcabc"
console.log(doubled2); // 10
```

# Интерфейсы
1. Что описывают интерфейсы в TypeScript?
2. Какие два способа расширения интерфейсов существуют?

>интерфейсы используются для описания объектов в TS

```ts
interface kek {
	a:string, 
	b: number,
	func(): void;
}

let puk:kek = {a: 'a', b: 33, func(){console.log("hello")}}

//Способы расширения
interface kek{ // два интерфейся с одним именем сольются и будет один со всеми
	loh:string // свойствами, которые есть у этих интерфейсов
} 
interface pukKek extends kek{
	puk:string
}// как kek только ещё и есть свойство puk
```

# Напиши интерфейс для объекта
```ts
// Напиши интерфейс для объекта
interface User {firstName: string, lastName: string	};
const user: User = { firstName: "Nikita", lastName: "Lumpov" }

// Расширь предыдущий интерфейс для ещё одного объекта
interface UserWithAddress extends User{address: string}
const userWithAddress = {
  firstName: "Anton",
  lastName: "Medvedev",
  address: "Кронверкская ул., 7, Санкт-Петербург, Россия"
}
```

# Напиши type guard
```ts
interface User {
  name: string
}
interface UserWithAddress extends User {
  address: string
}

// Надо сделать эту функцию type guard'ом, проверяющим
// что User это UserWithAddress
function isUserWithAddress(user: User): user is UserWithAddress { 
	return 'address' in user
  && typeof (user as UserWithAddress).address === "string"
 }
```

# Void vs undefined
1. Какое значение вернёт функция в JavaScript, если ничего не return'ить? = UNDEFINED
2. Чем возвращаемый тип функции void отличается от возвращаемого типа undefined?
void- тип TS означает, что ничего не возвращается. Его не существует в JS.
В JS вместо void будет возвращено undefined. 
Различия undefined от void :
В отличие от void - undefined можно использовать.
Если указать что функция должна возврващать undefined, но при этом не указать return - будет ошибка

# strictNullChecks
1. За что отвечает параметр strictNullChecks в tsconfig?
2. Что будет, если присвоить в переменную с типом number значение null?

> `strictNullChecks` управляет тем, можно ли присваивать `null` и `undefined` переменным с другими типами.

|Значение|Поведение|
|---|---|
|**`strictNullChecks: false`** (по умолчанию в старых проектах)|`null` и `undefined` можно присвоить **любой** переменной. Они считаются допустимыми значениями для всех типов.|
|**`strictNullChecks: true`** (рекомендуется, стандарт в новых проектах)|`null` можно присвоить только переменным с типом `null` или `null` в union-типе (`type \| null`). `undefined` — только переменным с типом `undefined` или в union-типе.|

```ts
// strictNullChecks = false
const numb: number = null // Ошибки нет 

// strictNullChecks = true
const numb: number = null // Ошибка: Type 'null' is not assignable to type 'number'
```

# Вид типизации в TypeScript

1. Типы Cat и Dog имеют одинаковую структуру объекта { name: string }. Будет ли ошибка, если присвоить в переменную с типом Cat значение переменной с типом Dog? 
Ошибки не будет т.к. структуры типов идентичны 
2. Какой вид типизации в TypeScript? 
Структурная типизация, а не номинальную, это значит, что если два объекта имеют одну структуру - они совместимы, имя типа тут роли не играет.

# Type assertion

1. Что такое type assertion (утверждение типа)? Как его выполнить? Влияет ли оно на выполнение кода?
2. Чем type assertion отличается от type casting'а?

>Type assertion(утверждение типа) - способ "заставить" TS думать, что у переменной есть  конкретный тип. Применяется в тех случаях, когда тип переменной невозможно определить силами TS, но разработчик знает, какой тип там должен быть.

```ts
function assert(str){ // assertion AS
	console.log((str as string).length)
}

function assert2(str){ // assertion <>
	console.log(<string>str.length)
}
```
Assertion никак не влияет на выполнение кода, потому что используется при компиляции(и полностью удаляется), а не при запуске кода.

>Type cast (приведение типов) - механизм преобразование одного типа в другой в JS

```js
let kek = 3; // тип number
let puk = '42' // string
kek = 4 + 'null' // неявное преобразование типа null к number

console.log(Number(puk))// Явное преобразование строки к числу
console.log(kek) // 4 + 0 
```

|Характеристика|Type assertion (TypeScript)|Type casting (Java/C#/другие языки)|
|---|---|---|
|**Когда происходит**|На этапе компиляции|На этапе выполнения (runtime)|
|**Влияет на данные**|❌ Нет (просто "говорит" компилятору)|✅ Да (преобразует данные)|
|**Проверка совместимости**|❌ Нет (компилятор верит)|✅ Да (если несовместимо — ошибка)|
|**Остаётся в JS**|❌ Нет (полностью удаляется)|— (не применимо к TS)|
|**Цель**|Удовлетворить TypeScript|Реально изменить тип/структуру данных|

# Объединения (unions) и пересечения (intersections)

1. Что такое объединение в TypeScript?
2. Что можно будет присвоить в переменную если её тип написан так:
   `const a: { a: string } | { b: string }`
3. Что такое пересечение?
4. Что можно будет присвоить в переменную если её тип написан так:
   `const a: { a: string } & { b: string }`

>**Union type (объединение)** — это тип, который позволяет переменной хранить **значения одного из нескольких типов**. Записывается с помощью вертикальной черты `|`.

```ts
const a: { a: string } | { b: string } = {a: "kek"};
const c: { a: string } | { b: string } = {b: 'loh'};
```

>**Intersection type (пересечение)** — это тип, который **комбинирует свойства нескольких типов в один**. Записывается с помощью амперсанда `&`.

```ts
const a: { a: string } & { b: string } = {a: 'loh', b:'pidr'}
```

# Типы
1. Что такое type в TypeScript?
2. Какие сущности можно описывать с помощью type?
3. В чём ключевое отличие type от interface?
4. Можно ли расширять type и каким способом?
5. Что произойдёт при повторном объявлении type с тем же именем?

>Type - это кастомный тип, который можно создавать с помощью расширения стандартных типов, либо созданием структуры объекта.

```ts
type userID = string | number; // userID может быть либо строкой либо числом
type status = 'active' | 'inactive'; // status может принимать значения только эти
type func = (date:Date)=>void
```

Сущности, которые можно описывать с помощью type

| **Примитивы**                    | `type T = string;`                                       |
| -------------------------------- | -------------------------------------------------------- |
| **Union типы**                   | `type Status = 'loading' \| 'success' \| 'error';`       |
| **Tuple (кортежи)**              | `type Point = [number, number];`                         |
| **Object (форма объекта)**       | `type User = { name: string; age: number; };`            |
| **Function (сигнатура функции)** | `type Greet = (name: string) => void;`                   |
| **Intersection типы**            | `type Full = { name: string } & { age: number };`        |
| **Условные типы**                | `type IsString<T> = T extends string ? true : false;`    |
| **Mapped типы**                  | `type Readonly<T> = { readonly [P in keyof T]: T[P]; };` |
| **Template literal типы**        | `type Greeting =`Hello ${string}`;`                      |
Отличие type от interface

| Характеристика          | `type`                                                            | `interface`                      |
| ----------------------- | ----------------------------------------------------------------- | -------------------------------- |
| **Что можно описывать** | Всё (примитивы, union, tuple, функции, объекты, условные типы...) | Только объекты и функции (формы) |
| **Расширение**          | Через `&` (intersection)                                          | Через `extends`                  |
| **Declaration merging** | ❌ Нельзя объявить дважды                                          | ✅ Можно (сливаются)              |

Можно ли расширять type и каким способом?
```ts
// Базовый тип
type Animal = {
    name: string;
};
type Dog = Animal & {
    breed: string;
    bark(): void;
};
const myDog: Dog = {
    name: "Rex",
    breed: "Shepherd",
    bark() { console.log("Woof!"); }
};
```

Что произойдёт при повторном объявлении type с тем же именем?
```ts
type User = { name: string };
// ❌ Ошибка: Duplicate identifier 'User'
type User = { age: number };
```

# Массивы
- Какими двумя способами можно написать тип для массива?
```ts
const arr:number[] = [1,2,3] // с квадратными скобками
const arr:Array<number> = [1,2,3] // с дженериком
```
- Чем `(string | number)[]` отличается от `[string, number]`?
В первом случае перед нами массив неопределённой длины, каждый элемент которого будет либо числом, либо строкой. Во втором случае перед нами кортеж из двух элементов, первый из которых - строка, а второй - число.
- Напиши тип, описывающий массив кортежей формата дата + строка
```ts
type ArrTuple = [Date,string][];
type ArrTuple = Array<[Date,string]>
```
- Напиши тип массива, первые два значения которого будут числами, а все остальные строками
```ts
type arr = [number,number, ...string[]]
```

# Функции

1. Напиши интерфейс, описывающий функцию принимающую строку и возвращающую число
```ts
interface func{(a:string):number}
```
1. Напиши тип, описывающий функцию принимающую строку и возвращающую число
```ts
type func = (a:string)=>number
```
1. Как указать опциональный параметр в функции?
```ts
function fun(a:string, b?:number){
	if (b) return a.repeat(b)
	return a
}
```
1. Чем опциональный параметр отличается от параметра с типом `тип | undefined`?
Опциональный параметр можно не передавать, а параметр с типом type| undefined передать нужно обязательно.
2. Как типизировать this в функции? Можно ли типизировать this внутри стрелочной функции?
В стрелочной функции не используется this, поэтому типизировать его нельзя. This захватывается из внешне области видимости.
```ts
interface User {
    name: string;
    age: number;
}
// this имеет тип User
function greet(this: User, greeting: string): string {
    return `${greeting}, ${this.name}! You are ${this.age} years old.`;
}
const user: User = { name: "Alice", age: 30 };
// Правильное использование: вызываем через bind или call
const boundGreet = greet.bind(user);
console.log(boundGreet("Hello")); // "Hello, Alice!..."
// Или через call/apply
console.log(greet.call(user, "Hi")); // "Hi, Alice!..."
// ❌ Ошибка: this не тот тип
const wrongUser = { name: "Bob" };
// greet.call(wrongUser, "Hey"); // Ошибка: нет поля age
```


# Енамы
1. Что такое enum? Во что он компилируется?
2. Что произойдёт, если попытаться передать значение в enum вместо ключа?
   Например `enum A { B = 1 }; const val = A[1]`
3. Что такое const enum? Во что он компилируется?
4. Как сделать аналог енамов без использования самих енамов?

> Enum(перечисления) - способ создать набор именованных констант. Бывают числовые и строковые.
```ts
enum Directions = { // Числовой enum
	up, // 0
	down, // 1
	left, // 2
	right, // 4
}
enum Status = {
	active = "ACTIVE",
	blocked = "BLOCKED",
}
```

```js
// Результат компиляции
var Directions = {
    0: "Up",
    1: "Down",
    2: "Left",
    3: "Right",
    Up: 0,
    Down: 1,
    Left: 2,
    Right: 3
}
var Status = { Pending: "PENDING", Success: "SUCCESS" }
```

```ts
// попытаться передать значение в enum вместо ключа
enum A { B = 1 }; // Для числовых enum действует обратный маппинг, см результаты компиляции
const val = A[1] // val = "B"

enum STR {blending = "PENDING"}; // Для строковый enum обратного маппинга нет.
const kek = STR["PENDING"]; // Undefined
```

Что такое const enum? Во что он компилируется?
```ts
// до компиляции
const enum Directions = {up, down, left, right}
const dir = Directions.up;
// После компиляции const enum удаляется полностью, остаётся лишь значение в других переменных. Стоит сказать, что у числовых const enum - нет обратного маппинга.
const dir = 0;
```

Как сделать аналог enum'ов без самих enum'ов
```ts
// По аналогии сделать объект с константами
const Direction = {
    Up: 0,
    Down: 1,
    Left: 2,
    Right: 3
} as const;
const Status = {
    Pending: 'PENDING',
    Success: 'SUCCESS',
    Error: 'ERROR'
} as const;

// или сделать объект, все поля которого readonly
class Direction {
    static readonly Up = 0;
    static readonly Down = 1;
    static readonly Left = 2;
    static readonly Right = 3;
}
```

# Never и exhaustive check
1. Что описывает тип never?
2. Что такое exhaustive check? Как он связан с never?
3. В каких случаях может использоваться тип never?

> never - пустой тип, не имеет допустимого значения. Представляет из себя "значение" которое возвращает функция, которая никогда не возвращает управление. 

> exhaustive check(исчерпывающая проверка) - техника, которая гарантирует, что обработаны все возможные варианты union типа. Если появится новый вариант, то TS выдаст ошибку.

```ts
type Color = 'red' | 'green' | 'blue';
function process(color: Color) {
    switch (color) {
        case 'red':
            // здесь color сужается до 'red'
            break;
        case 'green':
            // здесь color сужается до 'green'
            break;
        case 'blue':
            // здесь color сужается до 'blue'
            break;
        default:
            // Здесь TypeScript знает: color не может быть 'red', 'green' или 'blue'
            // Других вариантов у типа Color нет
            // → color имеет тип never
            const exhaustive: never = color; // ✅ работает, потому что color: never
    }
}
```


# Функция возвращающая never
Напиши функцию, которая будет всегда возвращать never
```ts
function func(){
	const str = 'lol'
	throw new Error('kek')
	return str;
}
```

# Литеральные типы
Что такое литеральные типы? Приведи пример литерального типа
2. Что такое literal widening? Как его не допустить?
3. Зачем нужны шаблонные литеральные типы (template literal types)?

> Литеральные типы - это типы, которые принимают конкретные значения, а не множество, как например string, или number.

```ts
let status: "active" | "stoped" = "stoped";
```

> literal widening (расширение литерального типа) - автоматическое преобразование литерального типа в более общий "active" | "stoped" -> string

```ts
const kek = 'kek' // имеет литеральный тип 'kek' потому что константу изменить нельзя
let puk = "puk" // TS расширяет тип от "puk" до string, потому что puk можно изменить
```

Чтобы избежать literal widening нужно либо:
- Использовать const переменную
- Явно указывать литеральный тип
- использовать конструкцию as const для массивов и объектов : let config = { dir: "up" } as const;

> Шаблонные литералы - совокупность литеральных типов, тем самым создавая новые литеральные типы.

```ts
type Direction = 'up' | 'down';
type Size = 'small' | 'medium' | 'large';
// Шаблонный литеральный тип — комбинирует их во все возможные варианты
type Position = `${Direction}-${Size}`;
// Position = 'up-small' | 'up-medium' | 'up-large' | 'down-small' | 'down-medium' | 'down-large'
```


# Readonly
 Что делает модификатор readonly у полей в интерфейсах/типах?
Пример: `type A = { readonly a: string }`
	readonly - делает поле объекта доступным только для чтения на уровне TS.
```ts
const a:A = {a:'kek'};
a.a = 'loh' // ошибка, поле доступно только для чтения
```
Как readonly работает на вложенные объекты? Станет ли весь объект readonly, если к полю где он лежит применить модификатор readonly?
	readonly делает недоступным для перезаписи только само поле, если в этом поле записан другой объект, то его структура останется неизменной, его поля не станут readonly
Влияет ли модификатор readonly на возможность изменить значение в поле в рантайме?
	Нет, readonly работает только в TS , до рантайма этот модификатор не доходит.

# Опциональные поля
1. Что такое опциональные поля в объектах? Как указать, что поле опциональное?
2. Чем `field?: string` отличается от `field: string | undefined`?

> Опциональные поля - это поля, которые могут отсутствовать в объекте. При обращении к ним TS знает, что их может не быть и потребует проверку на undefined.

```ts
interface User{
	name: string;
	race?: string; // к типу добавляется тип undefined, но при этом его можно и не указывать
	age: string | undefined; // это поле должно быть указано обязательно
}

const umtaka: User = {
	name: "Osas",
	age: undefined, // Нужно указать обязательно, даже если возвраст не определён.
};
umtaka.race = 'negr'
```

# As const
Что делает as const при применении к объекту или массиву?
```ts
// as const рекурсивно делает все поля (даже "c") readonly
let kuk = {a:string, b:{с:string}} as const;

// нельзя применять к объявлениям типов, только к значениям
// interface Kek {a:string, b:{с:string}} as const // ошибка

// as const сводит значение к литеральному типу
let kek = {a: "puk"} as const // теперь kek.a имеет тип "puk" а не string

// as const превращает массив в кортеж  все элементы которого readonly
let kek = [1,"kek"] as const // это это кортж типа [1,"kek"]
```

Чем тип объекта без as const отличается от типа того же объекта с as const? Как это связано с literal widening?
	Все поля объекта с "as const" защищены от literal widening, потому что без as const поля будут расширяться до общих типов kek.a = "loh" // будет иметь тип string, а не 'loh'

Как as const влияет на строковые и числовые значения в объекте?
	Строковые и числовые значения **сужаются до литеральных типов**, а не остаются `string` или `number`.

Как меняется тип массива при использовании as const? 
	Становится кортежом.
Является ли as const глубокой неизменяемостью на уровне выполнения кода?
	Нет. as const - это конструкция TS а значит существуют только на этапах до компиляции.

# Типизация const и let

Почему const x = 5 и let x = 5 имеют разные типы? Как это связано с literal widening?
```ts
const x = 5 // имеет тип 5, потому что изменять значение в этой переменной нельзя
let y = 5 // имеет тип number, потому что значение этой переменной можно изменять из-за этого TS расширяет тип до number, этот процесс называется literal widening
```
Почему при объявлении объекта через const, типы полей объекта всё ещё обобщаются до number, string и т.д.?
	Потому что эти поля всё ещё можно изменять. const делает невозожным перезапись в эту переменную другого значения.

# Discriminated unions
1. Что такое discriminated union?

>discriminated union (размеченное объединение) - паттерн по которому несколько типов объединяются в union посредством общего поля дискриминанта

```ts
interface Circle {
    type: "circle";      // общее поле-дискриминант
    radius: number;
}
interface Square {
    type: "square";      // общее поле-дискриминант
    side: number;
}
interface Triangle {
    type: "triangle";    // общее поле-дискриминант
    base: number;
    height: number;
}
type Shape = Circle | Square | Triangle;// union - объединение
// Использование
function getArea(shape: Shape): number {
    // TypeScript сужает тип по значению поля type
    switch (shape.type) {
        case "circle":
            return Math.PI * shape.radius ** 2;   // shape — Circle
        case "square":
            return shape.side ** 2;                // shape — Square
        case "triangle":
            return (shape.base * shape.height) / 2; // shape — Triangle
    }
}
```

2. Почему для поля-дискриминанта используется строковый литерал, а не обычный string?
	**Потому что для сужения типа TypeScript должен иметь возможность сопоставить **конкретное значение** с конкретным типом.** Если поле-дискриминант имеет тип `string`, TypeScript не сможет определить, какой именно вариант union'а используется. (читай комменты ниже)
```ts
interface Circle {
    type: string;        // ❌ обычный string — не литерал
    radius: number;
}
interface Square {
    type: string;        // ❌ обычный string
    side: number;
}
type Shape = Circle | Square;
function getArea(shape: Shape) {
    // ❌ Ошибка: TypeScript не может сузить тип
    if (shape.type === "circle") {
        // shape всё ещё Circle | Square
        return Math.PI * shape.radius ** 2;  // ❌ Ошибка: radius может не быть
    }
}
//Строковый литерал (`"circle"`, `"square"`) позволяет TypeScript **гарантировать**, что каждому варианту union'а соответствует **уникальное конкретное значение** поля-дискриминанта.
```

3. Чем discriminated unions удобнее обычных union без общего поля?


| **Безопасное сужение типа**    | TypeScript гарантированно знает тип после проверки дискриминанта             |
| ------------------------------ | ---------------------------------------------------------------------------- |
| **Автодополнение в IDE**       | После `switch (shape.type)` IDE подскажет варианты                           |
| **Exhaustive check**           | Можно использовать `never` для проверки, что все варианты обработаны         |
| **Читаемость кода**            | `switch (shape.type)` нагляден и предсказуем                                 |
| **Ошибки на этапе компиляции** | Если добавить новый тип в union, TypeScript подсветит все незатронутые места |

# Напиши discriminated union
```ts
// Есть функция, в которой необходимо определить тип животного
// Упрости логику работы этой функции за счёт применения паттерна discriminated union.
type Fish = { type: "fish"; name: string; isBig: boolean }
type Sheep = { type: "sheep"; weight: string; color: boolean }
type Cow = { type: "cow"; name: string; canFly: boolean }
type Cat = { type: "cat"; name: string; weight: boolean }

type Animal = Fish | Sheep | Cow | Cat;

function identifyAnimal(animal: Animal) {
	switch (animal.type){
		case "fish":
			console.log('It`s fish!')
			break;
		case "sheep":
			console.log('It`s sheep!')
			break;
		case "cow":
			console.log('It`s cow!')
			break;
		default:
			console.log('It`s cat!')
			break;
	}
}
```

# typeof и keyof
Для чего используется оператор typeof в типах TypeScript?
```ts
const user = {
    name: "Alice",
    age: 30,
    email: "alice@example.com"
};
// typeof в типе — получаем тип этого значения
type User = typeof user;
// User = { name: string; age: number; email: string; }
```
Чем typeof в TypeScript отличается от typeof в JavaScript?

|                    | `   typeof` в JavaScript                          | `typeof` в TypeScript         |
| ------------------ | ------------------------------------------------- | ----------------------------- |
| **Где работает**   | В рантайме (в выполняемом коде)                   | На этапе компиляции (в типах) |
| **Что возвращает** | Строку: `"string"`, `"number"`, `"object"` и т.д. | Тип TypeScript                |
Что делает оператор keyof в TypeScript?

Какой тип получится у выражения keyof { a: number; b: string }?
> **`keyof`** — это оператор, который принимает **тип объекта** и возвращает **union всех его ключей** (в виде строковых литералов).

```ts
interface User {
    name: string;
    age: number;
    email: string;
}
type UserKeys = keyof User;
// UserKeys = "name" | "age" | "email"
```

# Indexed Access Types
Что такое indexed access types? Как он работает с объектами и массивами?
> indexed access types (типы с доступом по индексу) - типы у которых свойство объекта или элемент массива можно получить используя синтаксис квадратных скобок : T\[K] Где T -  тип, K - ключ.

```ts
interface User {
	name: string,
	age: number,
}

type NameType = User['name'] // string
type AgeType = User['age'] // string
```

Как получить объединение типов полей a и b у типа
   `type Data = { a: string; b: number; c: boolean; }`?
```ts
type Data = { a: string; b: number; c: boolean; };

// Способ 1: через union ключей
type AorB = Data["a" | "b"];   // string | number

// Способ 2: явное объединение
type AorB2 = Data["a"] | Data["b"];   // string | number

// Способ 3: через Pick + keyof (более сложный)
type AorB3 = Pick<Data, "a" | "b">[keyof Pick<Data, "a" | "b">];
// тоже string | number
```
Как получить тип значения массива из типа `type ArrOfNumbers = number[]`?
```ts
type ArrOfNumbers = number[];
type Element = ArrOfNumbers[number];   // number
```

# Создай конфигурационный объект
1. Создай объект COLORS с ключами RED, GREEN, BLUE и значениями "red", "green", "blue".
2. Напиши функцию getColorName, которая может принимать **только ключи** этого объекта и возвращает соответствующее значение.
3. Обязательно типизируй и принимаемое, и возвращаемое значение. Функция должна возвращать строковый литерал из возможных значений объекта COLORS, а не просто string.

```ts
const COLORS = {RED: "red", GREEN: "green", BLUE: "blue"} as const
// ВАЖНО использовать as const, тогда типы свойств будут сужены до литеральных типов равных значениям.

type ColorKey = keyof typeof COLORS;           // "RED" | "GREEN" | "BLUE"
type ColorValue = typeof COLORS[ColorKey];     // "red" | "green" | "blue"

function getColorName(color: ColorKey): ColorValue{
	return COLORS[color]
}
```


# Индексные сигнатуры
Что такое индексные сигнатуры в TypeScript?
> Индексная сигнатура - это способ описать тип объекта, когда заранее неизвестны имена ключей, но известен тип ключа и тип значения

```ts
type Kek = {[key:string]: number}//Тип объекта все поля которого будут числами.

const puk:Kek = {age: 35, height: 196}
```
Сделай тип для объекта, который может содержать в себе любые ключи, а значения всегда будут равны литералу "a"
```ts
type Puk = {[key:string]: 'a'}
```
В чём отличие между указанием string и number в индексной сигнатуре? 
```ts
type Puk = {[key:string]: 'a'} // ключи будут строками, также можно использовать числовые значения
type Puk = {[key:number]: 'a'} // ключи будут числами
```
Можно ли будет использовать число в качестве ключа, если в индексной сигнатуре будет string?
	Да, можно JS автоматически преобразует число в строку при доступе к свойству.
```ts
type Puk = {[key:string]: string}
const kek:Puk = {};
kek[1] = 'kek' // эквивалентно kek["1"]
```
Типизируй массив строк в интерфейсе с помощью интерфейса и индексной сигнатуры
```ts
interface Arr {[key:number]: string}
const arr:Arr = ['geh','loh']
```
Напиши тип для объекта, который гарантировано будет содержать строку по ключу "a", а все остальные поля будут числами
```ts
type Puk = {
	a: string,
	[key:string]: number // ошибка "a" не может быть string и number
}

type Mixed = {
    a: string;
} & {
    [key: string]: number;
};
```

# Mapped types
Что такое mapped type в TypeScript и зачем он используется?
> **Mapped type (преобразованный тип)** — это способ создать **новый тип на основе существующего**, применяя преобразование к каждому свойству исходного типа. Работает как «цикл по ключам» на уровне типов

Синтаксис: { [P in K]: T[P] }, где:
P — переменная-ключ (как параметр цикла)
K — объединение ключей (обычно keyof T)
T\[P] — тип значения исходного свойства

```ts
// Простейший mapped type: копия типа
type Copy<T> = {
    [P in keyof T]: T[P];
};

interface User {
    name: string;
    age: number;
}

type UserCopy = Copy<User>;  // { name: string; age: number; }
```

Использование:
```ts
// создать копию типа но со всеми полями readonly
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

// Убирает readonly
type Mutable<T> = {
    -readonly [P in keyof T]: T[P];
};

//Транмформация типов
// Преобразует все значения в boolean
type ToBoolean<T> = {
    [P in keyof T]: boolean;
};

// фильтрация
// Оставляет только ключи, значения которых — строки
type StringKeys<T> = {
    [P in keyof T as T[P] extends string ? P : never]: T[P];
};
```

# Тип на основе объектов
```ts
const STATUSES = {
  todo: 0,
  processing: 1,
  done: 2,
} as const

const MESSAGES = {
  0: "TODO",
  1: "Processing task",
  2: "Task ready",
} as const

// Надо сделать тип, который по значениям из объекта STATUSES будет
// возвращать текст из объекта MESSAGES. Нельзя делать промежуточные типы,
// должен быть только один - StatusNumberToMessage
type StatusNumberToMessage = {
  [p in typeof STATUSES[keyof typeof STATUSES]] : typeof MESSAGES[p]
}
// Шаг 1: typeof STATUSES[keyof typeof STATUSES]
// keyof typeof STATUSES → "todo" | "processing" | "done"
// typeof STATUSES[keyof typeof STATUSES] → 0 | 1 | 2
// Шаг 2: K проходит по значениям 0, 1, 2
// typeof MESSAGES[K] → для K=0 → "TODO", для K=1 → "Processing task", для K=2 → "Task ready"
// Результат:
// {
//   0: "TODO";
//   1: "Processing task";
//   2: "Task ready";
// }
```

# Дженерики
Что такое дженерик? Чем его применение лучше применения литеральных типов?
> Дженерик - параметризированный тип, который позволяет создавать функции, интерфейсами, классы, работающие с разными типами данных сохраняя при этом типобезопасность

```ts 
// Без дженерика — потеря типов (any)
function identity(value: any): any {
    return value;
}
// С дженериком — тип сохраняется
function identity<T>(value: T): T {
    return value;
}
const result = identity("hello");  // тип: string
const result2 = identity(42);      // тип: number
//Литеральные типы фиксируют **конкретное значение**. Дженерики — **связь между типами**.
```

Куда можно передать дженерик?

| Конструкция        | Пример                                      |
| ------------------ | ------------------------------------------- |
| **Функции**        | `function func<T>(arg: T): T {}`            |
| **Классы**         | `class Box<T> { value: T; }`                |
| **Интерфейсы**     | `interface Response<T> { data: T; }`        |
| **Type aliases**   | `type Result<T> = T \| Error;`              |
| **Методы классов** | `class Example { method<T>(arg: T): T {} }` |

В каких случаях дженерик выводится автоматически?
	Если контекст позволяет однозначно определить тип
```ts
function identity<T>(value: T): T {
    return value;
}
// ✅ Автоматический вывод
const a = identity("hello");   // T выводится как string
const b = identity(42);        // T выводится как number
const c = identity(true);      // T выводится как boolean
// ❌ Не может вывести (нужно указать явно)
function getProperty<T, K>(obj: T, key: K): T[K] {
    return obj[key];
}
// Нужно явно указать K, если TypeScript не может вывести
```
Как работает ключевое слово extends в дженериках?
```ts
//`extends` в дженериках **ограничивает** допустимые типы. Дженерик может быть только тем типом, который соответствует ограничению.

function getLength<T extends { length: number }>(value: T): number {
    return value.length;  // ✅ У value точно есть length
}
getLength("hello");     // ✅ строка имеет length
getLength([1, 2, 3]);   // ✅ массив имеет length
```
Как указать дефолтное значение дженерика? Как оно будет работать?
```ts
// Дженерик с дефолтным значением
type ApiResponse<T = unknown> = {
    data: T;
    status: number;
};
// Использование
type Response1 = ApiResponse;           // data: unknown
type Response2 = ApiResponse<string>;   // data: string
type Response3 = ApiResponse<number>;   // data: number
```

# Универсальный интерфейс
У тебя есть библиотека для работы с запросами. Ответ на запрос возвращает объект с полями status, headers, data.
1. headers это объект без фиксированных данных, там могут быть любые строчные ключи и любые строчные значения
2. status это статус ответа, любое число
3. data может быть любым объектом, надо сделать дженерик который позволит указывать какие данные там лежат
Напиши тип FetchResponse, который описывает ответ на запрос, этот тип должен соответстовать всем вышеописанным требованиям

```ts
type FetchResponse<T = unknown> = {
  headers: {[k: string]: string};
  status: number;
  data : T;
}
```

# Типизируй reduce
```ts
// Есть кастомная функция reduce, которая работает как стандартный reduce из js.
// Надо с помощью дженериков типизировать эту функцию
function reduce<T, U>(
    array: T[],
    callback: (acc: U, item: T, index: number) => U,
    initial: U
): U {
    let acc = initial;
    for (let i = 0; i < array.length; i++) {
        acc = callback(acc, array[i], i);
    }
    return acc;
}
// T - тип элементов  (number, string ...)
// U - Тип аккумулятора (промежуточного и конечного результата) (number,string ...)
// T и U могут быть разными
```

# Утилитарные типы
Что такое утилитарные типы в TypeScript и зачем они нужны?
> утилитарные типы - встроенные в TS дженерик типы, которые выполняют распространённые преобразования типов. Избавляют от необходимости писать вручную mapped types
```ts
// Вместо ручного mapped type
type MyPartial<T> = { [P in keyof T]?: T[P] | undefined};

// Используем встроенный Partial
type PartialUser = Partial<{ name: string; age: number }>;
// { name?: string; age?: number }
```
Чем Exclude<T, K> отличается от Omit<T, K>?
```ts
// Exclude — для union
type Colors = "red" | "green" | "blue";
type WithoutRed = Exclude<Colors, "red">;  // "green" | "blue"
// Omit — для объектов
interface User {
    id: number;
    name: string;
    password: string;
}
type PublicUser = Omit<User, "password">;  // { id: number; name: string; }
```
Чем Pick<T, K> отличается от Extract<T, K>?
```ts
// Pick — для объектов
interface User {
    id: number;
    name: string;
    email: string;
}
type UserPreview = Pick<User, "id" | "name">;  // { id: number; name: string; }

// Extract — для union
type Status = "loading" | "success" | "error";
type GoodStatus = Extract<Status, "success" | "loading">;  // "loading" | "success"
```
Зачем нужны типы Parameters\<T> и ReturnType\<T>?

```ts
function greet(name: string, age: number): string {
    return `${name} is ${age} years old`;
}
// Параметры как кортеж
type GreetParams = Parameters<typeof greet>;  // [string, number]
// Тип возвращаемого значения
type GreetReturn = ReturnType<typeof greet>;  // string
// Реальный пример: типизация обёрток
function withLogging<T extends (...args: any[]) => any>(fn: T): (...args: Parameters<T>) => ReturnType<T> {
    return (...args) => {
        console.log("Called with:", args);
        return fn(...args);
    };
}
```
Зачем нужны типы Readonly\<T>, Nullable\<T> и NonNullable\<T>?
```ts
// Readonly — для неизменяемых объектов
const config: Readonly<{ timeout: number; retries: number }> = {
    timeout: 5000,
    retries: 3
};
// config.timeout = 1000;  // ❌ Ошибка

// NonNullable — отфильтровать null/undefined
type MaybeString = string | null | undefined;
type DefinitelyString = NonNullable<MaybeString>;  // string

// Nullable (самодельный)
type Nullable<T> = T | null;
type MaybeNumber = Nullable<number>;  // number | null
```
Зачем нужны типы Partial\<T> и Required\<T>?
```ts
interface User {
    id: number;
    name?: string;
    age?: number;
}
// Все поля опциональны
type PartialUser = Partial<User>;  // { id?: number; name?: string; age?: number; }
// Все поля обязательны
type RequiredUser = Required<User>;  // { id: number; name: string; age: number; }
```
Что делает утилитарный тип Record<K, T>?
```ts
//**`Record<K, T>`** создаёт **тип объекта**, где:
// Ключи — из union `K`    
// Значения — типа `T`

type WeekDays = Record<"mon" | "tue" | "wed", string>;
// { mon: string; tue: string; wed: string; }
const schedule: WeekDays = {
    mon: "Work",
    tue: "Meeting",
    wed: "Rest"
};
```
Как получить из типа Promise\<number> тип number с помощью утилитарных типов?
```ts
type PromiseNumber = Promise<number>;
type Unwrapped = Awaited<PromiseNumber>;  // number
// Работает с вложенными Promise
type Nested = Promise<Promise<string>>;
type UnwrappedNested = Awaited<Nested>;  // string
```

# Типизируй объект
```ts
// Напиши типы для этого объекта
const courseData = {
  id: 7,
  created_at: new Date(),
  owner: { id: 1, name: "Nikita", random: 928 },
  students_map: {
    12: { id: 12, name: "Oleg", random: "1" },
    24: { id: 24, name: "German", random: "31" },
    31: { id: 31, name: "Vlad", random: 17 },
    74: { id: 31, name: "Vlad" },
  },
  analytics: [
	  ["12.01.2024", 392139312],
	  ["14.01.2024", 322139951],
	  ["15.01.2024", 362192112],
  ],
}

interface User {
	id:number;
	name: string,
	random?: string | number
}

interface Course {
  id: number;
  created_at: Date;
  owner: User;
  students_map: Record<number,User>;
  analytics: [string,number][]
}
```

# Напиши кастомный Record
```ts
// Напиши тип CustomRecord, который будет полностью повторять логику работы Record.
type CustomRecord<K extends string|number|symbol,V> = {[k in K]: V}
```

# Условные типы (conditional types)
Что такое condinional types?
> conditional type (условные тип) - тип, которые возвращается при обработке условия.
```ts
// схема условия conditional type :  `T extends U ? X : Y`
// Если `T` можно присвоить типу `U` (т.е. `T` является подтипом `U`), то `X`
// Иначе `Y`

type IsString<T> = T extends string ? "yes" : "no";
type A = IsString<string>;   // "yes"
type B = IsString<number>;   // "no"
type C = IsString<"hello">;  // "yes" (литерал — подтип string)
```

Как работает условие в условном типе? Зачем там extends?
	`T extends U ? X : Y`
	`extends` в условных типах проверяет **отношение подтипа** (subtype relationship): является ли `T` подтипом `U`?
	 Если Значение типа `T` можно безопасно присвоить переменной типа `U`, то говорят что T является подтипом U

Что будет результатом проверки `string | number extends string ? boolean : object`
	Условный тип проверяет, является ли **весь union** `string | number` подтипом `string`.
	- `string` — подтип `string` ✅   
	- `number` — подтип `string` ❌
	Так как `number` **не** является подтипом `string`, весь union `string | number` **не** является подтипом `string`.
	Следовательно, условие `false` → выбирается ветка `object`.
# Напиши условный тип
Напиши условный тип IsPrimitive\<T>, который возвращает "yes", если T это примитивный тип (string, number, boolean, bigint, symbol, null или undefined), и "no" во всех остальных случаях.
```ts
type Primitive = string | number | boolean | bigint | symbol | null | undefined;

type IsPrimitive<T> = T extends Primitive ? "yes" : "no"
```

# Дистрибутивные условные типы
Что такое дистрибутивный условный тип?
> **Дистрибутивный условный тип** — это условный тип вида `T extends U ? X : Y`, где `T` является **«голым» параметром типа** (не обёрнутым ни в какой другой тип). Когда в такой тип передаётся **union**, TypeScript **распределяет (distributes)** условную проверку по каждому члену union, а затем объединяет результаты обратно в union.

**Ключевое правило:** Дистрибутивность включается **только** когда левая часть `extends` является **голым параметром** (не массивом, не кортежем, не промисом, не объектом и т.д.).
```ts
// Обычный условный тип
type ToArray<T> = T extends any ? T[] : never;

// Тип StrNumArr станет (string | number)[]? Нет!
// Благодаря дистрибуции он станет string[] | number[]
type StrNumArr = ToArray<string | number>; 

// Разбор:
// (string extends any ? string[] : never) | (number extends any ? number[] : never)
// string[] | number[]
```

Что будет результатом проверки `type T<X> = X extends string ? true : false; type R = T<string | number>`?
	R = T<string | number> = (string extends string ? true : false) | (number extends string ? true : false) = true | false = boolean 
Как изменится результат, если написать `type R = T<(string | number)>`
	Результат **не изменится**, потому что круглые скобки не влияют на дистрибуцию, Они лишь группируют union, но не меняют его суть. поэтому результат не изменится (boolean)
Есть тип `type ToArray<T> = T extends any ? T[] : never`. Какой результаn выйдет при 
вызове ToArray<string | number> и почему?
	Внимание вопрос с подвохом, неправильно думать что ответом будет (string | number) []
	Когда ты пишешь `T<(string | number)>`, то просто передаешь один union-тип. Для TypeScript `string | number` и `(string | number)` — это абсолютно одно и то же. Как только этот союз попадает внутрь `type T<X> = X extends ...`, он «разлетается» по веткам условия, потому что `X` внутри определения ничем не защищен. 
	Ответом будет = string[] | number[] потому что string и number - являются подмножеством any
	Если нужно , чтобы дистрибуция не сработало (т.е. произошло сравнение union) то нужно использовать кортеж
```ts
type ToArrayNonDistributive<T> = [T] extends [any] ? T[] : never;
type Result2 = ToArrayNonDistributive<string | number>;  // (string | number)[]
```

# Напиши кастомный Exclude
Напиши тип CustomExclude, который будет полностью повторять логику работы Exclude
```ts
type CustomExclude<D,E> = D extends E ?  never : D;
```

# infer
[пояснение](https://www.youtube.com/watch?v=XZIOj_6k7EI)
Что делает ключевое слово infer в условных типах? и Как с помощью infer извлечь тип возвращаемого значения функции?
>infer - позволяет извлечь тип из другого условного типа.  `infer` объявляет новый параметр-типа, который выводится из проверяемого типа внутри условного типа. infer работает не просто с «другим условным типом», а с проверяемым типом внутри условного типа (через extends),
```ts
// Предположим, что нам нужно получить возвращаемое значение функйии, и при этом мы не можем использовать ReturnType

type MyReturnType<func  extends (...args: any[])=> any> = func extends (...args: any[])=> infer funcReturnType ? funcReturnType : never
// func  extends (...args: any[])=> any - означает что дженерик func должен быть функцией
// данная конструкция означает - если тип func ялвяется типом функцией, которая принимает любое колличество любого типа параметров, то запиши тип (infer) в funcReturnType и верни, в противном случан верни never.
function kek (a:string) {return a}
type puk = MyReturnType<typeof kek>; // string

// Теперь попробуем вернуть параметры функции.
type MyParameters<func> = func extends (...args: infer funcParameters)=> any ? funcParameters : never
type pukrameters = MyParameters<typeof kek>; // [a: string]
```

Как можно с помощью infer получить тип элемента массива из типа массива T[]?
```ts
type arrElementType<arr> = arr extends (infer arrElType)[] ? arrElType : never;

type A = arrElementType<string[]>;     // string
```

Можно ли использовать infer вне условного типа?
	**Нет, нельзя.** `infer` может использоваться **только внутри условного типа** (после `extends`).
Как можно ограничить выводимый infer'ом тип? Например, выводить тип только если он number
```ts
type ExtractNumber2<T> = T extends infer U ? (U extends number ? U : never) : never;
```

# Напиши кастомный ReturnType
Напиши тип ReturnType, который будет полностью повторять логику работы ReturnType. Не забудь добавить ограничение, чтобы в CustomReturnType можно было передать только функцию, а не любой тип.

```ts
type CustomReturnType<func  extends (...args: any[])=> any> = func extends (...args: any[])=> infer funcReturnType ? funcReturnType : never
```

# Напиши Omit и Pick
Напиши реализацию CustomOmit, которая будет полностью повторять работу Omit
```ts
type CustomOmit<I,O extends keyof I> = {
	[P in keyof I as P extends O ? never : P]: I[P] 
};
// O extends keyof I - перечисление ommited свойств должно быть подмножеством объединения свойств interface
// [P in keyof I as P extends O ? never : P]: I[P]; = 
  // ( P in keysof I) - TS пробегается по всем свойствам интерфейса I,  если одно из них совпадает с одним из ommited свойств то для него будет возвращенно never, все остальные свойства будут не тронуты.
  // и в конце [...] : I[P] - каждому свойству интерфейса I будет присовоен его тип, взятый с помощью I[P].
```
Напиши реализацию CustomPick, которая будет полностью повторять работу Pick
```ts
type CustomPick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

# Напиши Partial и Readonly
1. Напиши реализацию CustomPartial, которая будет полностью повторять работу Partial
2. Напиши реализацию CustomReadonly, которая будет полностью повторять работу Readonly
```ts
// 1
type CustomPartial<T> = { [P in keyof T]?: T[P]  | undefined }

// 2
type CustomReadonly<T> = { readonly [P in keyof T] : T[P]}
```

# Сложный кастомный тип
```ts
// Ты пишешь утилиту для работы с формами. Тебе надо сделать тип,
// Который будет преобразовывать описание полей в формате объекта,
// В тайпскриптовый тип.

// Енам с типами данных для формы
enum FieldType {
  String = "String",
  Number = "Number",
  Object = "Object",
  Array = "Array",
}

// Варианты полей формы. Кстати, тут применяеется discriminated union подход
type Field =
  | { type: FieldType.String }
  | { type: FieldType.Number }
  | { type: FieldType.Object, properties: Record<string, Field> }
  | { type: FieldType.Array, itemType: Field }

// Описание формы в формате объекта
const fields = {
  type: FieldType.Object,
  properties: {
    id: { type: FieldType.Number },
    name: { type: FieldType.String },
    roles: { type: FieldType.Array, itemType: { type: FieldType.String } },
  }
} as const

// Тебе надо написать реализацию FieldValue, при передаче
// в который будет возвращаться тип-объект с данными формы.
type MyField = FieldValue<typeof fields> // { id: number, name: string, roles: string[] }

type FieldValue<T extends Field> = T extends { type: FieldType.String }
  ? string
  : T extends { type: FieldType.Number }
  ? number
  : T extends { type: FieldType.Object; properties: Record<string, Field> }
  ? {
      [K in keyof T["properties"]]: FieldValue<T["properties"][K]>;
    }
  : T extends { type: FieldType.Array; itemType: Field }
  ? FieldValue<T["itemType"]>[]
  : never;
```

# Key remapping
Что позволяет делать key remapping в рамках mapped types?
> key remapping - возможность изменять имена ключей при создании новых типов.
```ts
// простой маппинг (ничего не меняет)
type kek<T> = {[P in keyof T]: T[P]} // просто пробежались по всем ключам.
// Теперь переименуем ключи
type puk<T> = {[P in keyof T as `new_pukKey${string & P}`]:T[P]}
```
Что произойдёт с ключом, если в key remapping вернуть вместо него never?
	Ключ будет изключён из оригинального пула ключей
```ts
type filterString<T> = {[P in keyof T as T[P] extends string? never: P]: T[P]}
// T[P] - это тип поля .
type Puk = filterString<{a:string, b:number, c:string}> // {b:number}
```
Что будет в Result после применения типа PrefixKeys?
   ```ts
   type PrefixKeys<T> = { [K in keyof T as `_${string & K}`]: T[K] }
   type Result = PrefixKeys<{ a: string, b: number }>// {_a: string, _b: number}
   ```
# Переверни тип
```ts
// Есть тип описывающий статус-коды HTTP ответа. Тебе надо сделать
// утилитарный тип, который позволит переворачивать такие типы.
// Например было 400: "Bad Request", стало "Bad request": 400
type StatusToNumber = {
  200: "Ok",
  400: "Bad request",
  404: "Not found"
}

type NumberToStatus = Reverse<StatusToNumber>

type Reverse<T> = {[P in keyof T as T[P]] : P}
```

# Классы
Как указать, что класс является реализацией какого-то интерфейса? Может ли класс реализовывать несколько интерфейсов?
```ts
interface Human{
	name: string;
	makePuk()=>void
}

interface Citizen {
	payTax()=>void;
}

class User implements Human, Citizen{
	name: string;
	password: string;
	constructor(name:string, password:string){
		this.name = name;
		this.password = password;
	}
	
	makePuk(){console.log('Puk')}
	payTax(){console.log('I have to pay, i wont to cry')};
}
```
Что делает модификатор доступа public в классе? Обязательно ли его указывать?
> public - делает свойство класса доступным отовсюду, из экземпляра класса, у наследников класса , внутри класса, снаружи. Данный модификатор ставится по умолчанию.
```ts
class Puk {
	kek: string;
	public loh: string;
	constructor(kek: string, loh: string){
		this.kek = kek;
		this.loh = loh
	}
}

const puk = new Puk("kek","pidr")
console.log(puk.kek) // kek
console.log(puk.loh) // pidr
```
Что делает модификатор доступа private?
> private - модификатор, который делает свойство доступным только внутри методов самого класса, даже наследники к нему не могут иметь доступ.
```ts
class Human {
	name: string;
	private thoughts: number;
	thinking(){console.log(this.thoughts), this.thoughts++}
	constructor (name:string){
		this.thoughts = 0;
		this.name = name;
	}
}
class DotaPlayer extends Human{
	playDota(){console.log('Я твой отец')};
}

const human = new Human('bomjatina');
const dotaPlayer = new DotaPlayer("jojoba",)

human.thinking() // 0
human.thougths // ошибка, поле private, так просто к нему доступ не получить

dotaPlayer.thinking() // 0
dotaPlayer.thougths // ошибка, поле private, так просто к нему доступ не получить
```
Чем модификатор доступа protected отличается от private?
> protected - тоже модификатор поля класса, но с более строгой политикой доступа

| Модификатор     | Доступ внутри класса | Доступ в наследниках | Доступ снаружи |
| --------------- | -------------------- | -------------------- | -------------- |
| **`private`**   | ✅ Да                 | ❌ Нет                | ❌ Нет          |
| **`protected`** | ✅ Да                 | ✅ Да                 | ❌ Нет          |
Зачем нужен модификатор readonly в классах?
readonly разрешает менять значение поля только в конструкторе, во всех остальных случаях будет ошибка.

# Абстрактные классы
Что такое абстрактный класс в TypeScript?
> Абстрактный класс это класс, у которого нельзя создать экземпляр класса, но от которого можно наследоваться.
```ts
abstract class Vehicle{
	wheels: number;
}
class Car extends Vehicle {
	mark: string
}
```
Можно ли создать экземпляр абстрактного класса? Почему?
	Нет, экземпляр абстрактного класса создать нельзя, потому что абстрактный класс предназначен только для наследования.
Что такое абстрактный метод и какие требования он накладывает на наследников?
> Абстрактный метод, это метод без реализации, он задаёт сигнатуру, которую должны реализовать все наследники.
```ts
abstract class Worker {
    abstract doWork(): void;           // абстрактный метод
    abstract getSalary(): number;      // абстрактный метод
}

// ❌ Ошибка: не реализован getSalary
class BadWorker extends Worker {
    doWork() {
        console.log("Working...");
    }
}

// ✅ Правильно: реализованы все абстрактные методы
class GoodWorker extends Worker {
    doWork() {
        console.log("Working...");
    }
    getSalary(): number {
        return 1000;
    }
}
```
Может ли абстрактный класс содержать реализованные методы и свойства?
	Да. Абстрактный класс это обычный класс, который нельзя инстанцировать и с возможностью создавать абстрактные методы.
```ts
abstract class Puk {
	makePuk(){console.log('puk')}
}
class Kek extends Puk {
	makeKek (){console.log('kek')}
}

const kek = new Kek();
kek.makePuk(); // puk
```
Чем абстрактный класс отличается от интерфейса?

| Характеристика                 | Абстрактный класс                                         | Интерфейс                                  |
| ------------------------------ | --------------------------------------------------------- | ------------------------------------------ |
| **Цель**                       | Шаблон для наследования с возможностью общей логики       | Контракт, описывающий форму объекта        |
| **Реализация методов**         | ✅ Может содержать реализованные методы                    | ❌ Только сигнатуры (кроме default в JS)    |
| **Состояние (свойства)**       | ✅ Может иметь свойства с значениями                       | ❌ Только объявления свойств (без значений) |
| **Модификаторы доступа**       | ✅ `public`, `protected`, `private`                        | ❌ Все члены неявно `public`                |
| **Множественное наследование** | ❌ Только один родительский класс                          | ✅ Множественная реализация                 |
| **Конструктор**                | ✅ Может иметь конструктор                                 | ❌ Не может                                 |
| **Когда использовать**         | Когда нужна общая логика, состояние или `protected` члены | Когда нужно описать только форму объекта   |

# Наследуй абстрактный класс
```ts
enum AnimalType {
  Reptile = "Reptile",
  Mammal = "Mammal"
}

abstract class Animal {
  private name: string
  protected abstract type: AnimalType

  constructor(name: string) {
    this.name = name
  }

  public getName(): string {
    return this.name
  }

  protected makeSound(sound: string): void {
    console.log(sound)
  }

  public abstract speak(): void
}

// Создай класс Cat, который наследуется от Animal. Задай тип животного как AnimalType.Mammal
// реализуй метод speak, который вызывает метод makeSound и выводит "meow"
class Cat extends Animal{
	protected type= AnimalType.Mammal
	speak(){
		this.makeSound("meow");
	}
}
const cat = new Cat("Sima");
```

# Сужение/расширение доступа в классах
Можно ли изменить модификатор доступа поля с public на private в наследуемом классе?
	Нельзя
Можно ли изменить модификатор доступа поля с private на protected в наследуемом классе?
	Нельзя
Что произойдет, если попытаться сделать protected поле public в подклассе?
	Всё нормально. Это не нарушает безопасность: если наследник хочет сделать что-то более доступным, это его право. Родительский класс всё равно работает корректно.

# d.ts
Что такое файл d.ts и для чего он используется?
> declaration file (d.ts) - файл содержащий описание типов.
```ts
// d.ts
// types.d.ts
interface User {
    id: number;
    name: string;
}

function greet(user: User): void;
```

Чем отличается обычный TypeScript-файл (.ts) от файла объявления (.d.ts)?

| Характеристика                | `.ts` (обычный)          | `.d.ts` (объявление)       |
| ----------------------------- | ------------------------ | -------------------------- |
| **Содержит реализацию**       | ✅ Да                     | ❌ Нет (только типы)        |
| **Компилируется в JS**        | ✅ Да                     | ❌ Нет                      |
| **Основное назначение**       | Код, который запускается | Описание типов для JS-кода |
| **Может содержать `declare`** | Редко (обычно не нужно)  | ✅ Постоянно                |
Что произойдёт, если в d.ts объявить функцию или переменную, но не дать её реализации?
	Всё нормально, это ожидаемое поведение.

Что делает ключевое слово declare и когда используется declare global?
> declare - объявление, которое не будет скомпилированно в JS. 
```ts
// Объявление глобальной переменной, которая определена в HTML
declare const API_KEY: str
```
> declare  global - расширение глобальных типов
```ts
// custom-globals.d.ts
export {};  // нужен, чтобы файл считался модулем
declare global {
    interface Window { // расширяем глобальный тип Window
        myCustomProperty: string;
    }
    
    function logToAnalytics(data: object): void;
    
    const APP_VERSION: string;
}
```
Как с помощью d.ts можно типизировать библиотеку без TypeScript-типов?
```ts
// my-library.d.ts
declare module "legacy-lib" {
    export function calculate(value: number): number;
    
    export interface Config {
        name: string;
        timeout?: number;
    }
    
    export function init(config: Config): void;
    
    export const version: string;
}
```

# Типизируй код
```ts
// Библиотека добавляет в глобальный объект window поле b с типом number, но в типах
// это не будет прописано, потому что библиотека написана на чистом js.
// Тебе надо написать d.ts файл, который будет указывать,
// что в поле b объекта window лежит число

// file1.tsx
window.b = 123

// file2.tsx
console.log(window.b)
```
```ts
// d.ts
declare global {
    interface Window { // расширяем глобальный тип Window
        b: number;
    }
}
```

# satisfies
Что делает оператор satisfies в TypeScript?
Чем satisfies отличается от явного указания типа через as или :?
> staisfies - оператор, проверяющий соответствие значение типу,  satisfies не меняет тип самой переменной, а только проверяет соответствие значению заданному типу. В отличие от ":", который проверяет соответствие переменной типу - satisfies сохраняет самый точный выведенный тип
```ts
type Color = string | {r:string, g:string, b:string};

const red:Color = "red"// тип Color => либо строка либо интерфейс {r,g,b}
const green = "green" as Color // тип Color => либо строка либо интерфейс {r,g,b}
const blue = "blue" satisfies Color // Тип "blue", потому-что это более точный тип, который является подмножестовм string, а значит может быть Color.

red.length // ошибка Color может не быть string и такого метода может не быть.
blue.length // нормально это же string.

// нужно быть аккуратнее с as потому что он принудительно задаёт тип.
```
Как satisfies помогает при работе с объектами и их типами?
	satisfies позволяет убедиться, что переменная соответсвтует определённой струкуре (типу) но при этом сохраняет литеральные типы.
```ts
type Theme = {
    colors: string[];
    fontSize: number;
};
const theme = {
    colors: [ "red", "blue" ],
    fontSize: 14
} satisfies Theme;

theme.colors;  // тип: string [] 
theme.fontSize;  // 14 

// Если нужны литеральные типы, то нужно использовать as const
/*
const theme = {
    colors: [ "red", "blue" ] as const, // тут будет кортеж
    fontSize: 14 as const // тут будет литеральный тип
} satisfies Theme;
*/
```
Можно ли использовать satisfies для проверки сложных типов, например с объединениями или пересечениями?
	**Да, `satisfies` можно использовать для проверки сложных типов, включая объединения (union) и пересечения (intersection).**.
```ts
type Status = 
    | { type: "loading" } 
    | { type: "success"; data: string } 
    | { type: "error"; error: Error };

// ✅ Проверяем, что объект соответствует одному из вариантов union'а
const state = {
    type: "loading"
} satisfies Status;

// Тип state остаётся { type: "loading" } (а не Status)
state.type;  // "loading" (литерал, а не "loading" | "success" | "error")
```

# Проверь с помощью satisfies
```ts
// Есть объект цветовой палитры, где цвета могут быть как строкой, так и RGB кортежем
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];

const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255],
} satisfies Record<Colors, string | RGB>;

// Почему при замене satisfies на прямую типизацию через
// const palette: Record<Colors, string | RGB>
// этот код будет выдавать ошибку?
const greenNormalized = palette.green.toUpperCase();

// Потому что palette.green может быть не только string но и RGB а у RGB нет метода toUpperCase
```