[[Закон Амдала - параллельное программирование]]
[[Планировщик Go]]
[[Kafka]]
[[Redis]]
### Functions
- Изначально переменные передаются по значению, поэтому когда мы принимаем аргументы в функции, создается копия этого аргумента.
- Чтобы вернуть несколько значений отдельно `func f() (int, int) {}`, то нужно обратить в скобки возвращаемые значения функции.
- Если мы назовем возвращаемые значения `func f() (x, y int) {}`, то они инициализируются с начальными значениями. Потом можно вернуть все значения оператором `return` без указания возвращаемых переменных.
- Если мы не хотим использовать какую-то возвращаемую переменную из функции, то мы может указать `_` и компилятор полностью удалить эту переменную.
- Функции высшего порядка - функции, которые принимают другие функции в качестве аргумента.
- Закрывающая функция (Closures function), мы закрываем переменную sum, но имеем доступ к переменной напрямую из вложенной функции, которая не копирует значение, а изменяет его напрямую.

```go
countAdder := adds()
countAdder(1)

func adds() func(x int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```

- Анонимная функция

```go
newX, newY, newZ := conversions(func(a int) int {
	return a + a
}, 1, 2, 3)
```

### Conditions
- Чтобы создать переменную, которая будет доступна только в пределах видимости if используется такая конструкция `if [INIT VAL]; [CONDITION] {}`
### Structs

- `type name struct {}` создание структуры
- `name := name{}` создание экземпляра структуры
- Непосредственное создание экземпляра анонимной структуры с присвоением значений. Используется в случае, если мне нужен всего один экземпляр.

```go
myCar := struct {
	name string
	model string
}{
	name: "Tesla",
	model: "Model 3",
}
```

- Вложенные структуры c использованием анонимных структур. Используют в случае, если эту структуру необходимо использовать один раз.

```go
type car struct {
	name string
	wheel struct {
		radius int
		material string
	}
}
```

- Встраиваемые структуры. Мы наследуем все поля из структуры car и можем напрямую обращаться к ним `truck.model`, происходит эффект всплытия, то есть не нужно обращаться сначала к полю `car`, если бы оно имело имя `type truck struct {car car}`

```go
type car struct {
	name string
	model string
}

type truck struct {
	car
	badSize string
}
```

- `func (sn sructName)f() () {sn.[field] [yourCode]}` - это метод, который имеет получателя, то есть мы принимаем экземпляр структуры и можем манипулировать далее в функции с этим экземпляром.
### Interfaces

- Интерфейсы нужны для того, чтобы определять набор методов для экземпляра структуры, который мы можем передавать в функции.  Объявление интерфейса `type IName interface {f(args) int}` Можно называть переменные в сигнатуре, также как в обычной функции, для более читабельного интерфейса.
- Можно перейти обратно к экземпляру `c, ok := s.(circle)` , то есть если s (экземпляр структуры) типа `circle`, тогда в переменную `c` записывается значение, а в переменную `ok` записывается булево значение (true). Можно пройтись по всем экземплярам, чтобы выбрать нужный метод:

```go
func f(ex Interface) int {
	switch t := ex.(type) {
	case something:
		return t.[your func]
	case something else:
		return t.[your func]
	default:
		return 0
	}
}
```

- С приходом дженериков интерфейсы нужны не только для описания сигнатур функций, но и для ограничений типов.

```go
type Ordered interface {
    ~int | ~int8 | ~int16 | ~int32 | ~int64 |
        ~uint | ~uint8 | ~uint16 | ~uint32 | ~uint64 | ~uintptr |
        ~float32 | ~float64 |
        ~string
}
```

### Errors

```go
func allUsers() (string, error) {
users, err := getUsers()
if err != nil {
	// handling error
	return "", err
}
return users, nil
}
```

```go
import "errors"

errors.New("Error!")
```

### For

`for [INITIAL]; [CONDITION]; [AFTER] {}`
`for [INDEX], [VALUE] := range [ITERABLE] {}`

- Можно пропускать условие и писать `for [INITIAL]; ;[AFTER] {}` и уже в теле определить условие выхода из цикла
- Чтобы написать while - сразу пишется условие `for [CONDITION] {}`

### Arrays

- Массивы в GO всегда фиксированной длины
- Передаются по значению, если не использовать указатели
- Объявление `var arr [666]int` или `arr := [3]int{}`
- Объявление с присвоением значений `arr := [3]int{6, 6, 6}`

### Slices

- Динамический массив
- Передается по ссылке
- Объявление `var mySlice []int` или `mySlice := []int{}` тут он еще не инициализирован, то есть имеет тип `nil`. Чтобы инициализировать используем `initSlice := make([]int, 0)`
- Объявление с присвоением значений `arr := []int{6, 6, 6}`
- Позволяет создавать срез массива `slice := arr[1:2] // [6]`
- Вручную создать slice - `mySlice := make([]int, 5, 10)` в этом случае размерность массива будет 5 `len()`, а зарезервировано под него будет емкость 10 `cap()`
- Обычно создают slice без зарезервированного места `mySlice := make([]int, len(someData))`
- variadic функции принимают любое количество аргументов `func f(...args int) {}`
- spread оператор разбивает slice на отдельные элементы и передает их все в функцию`printSomething(names...)`

```go
func printStrings(strings ...string) {
	for i := 0; i < len(strings); i++ {
		fmt.Println(strings[i])
	}
}

func main() {
    names := []string{"bob", "sue", "alice"}
    printStrings(names...)
}
```

- Чтобы добавить новые элементы в slice

```go
slice = append(slice, oneThing)
slice = append(slice, firstThing, secondThing)
slice = append(slice, anotherSlice...)
```


### Maps

- Используется для быстрого поиска O(1)
- Передается по ссылке
- Инициализация пустой хэш-таблицы

```go
ages := make(map[string]int)
ages["John"] = 37
```

- Объявление с инициализацией

```go
ages := map[string]int{
	"John": 37,
}
```

- Вставить `mapName[key] = elem`
- Получить `elem = mapName[key]`
- Удалить `delete(mapName, key)`
- Проверить на содержание `elem, ok := mapName[key]`

```go
func isExist(mapName map[string]int, key string) (int, error) {
	if _, ok := mapName[key]; !ok {
		return 1, errors.New("Does not exists")
	}
	return 0, nil
}
```

### Defer

- Откладывает выполнение какой-то функции на конец выполнения текущей функции (то есть отложенная функция будет выполнена перед тем, как текущая функция вернет значения). Может быть полезно для работы с файловой системой.

### Pointers

- Указатели мы используем только в том случае, если мы хотим оптимизировать использование памяти, потому что использование указателей может быть небезопасным (например он может ссылаться на `nil`). Чтобы избежать этого мы можем добавить простую проверку.

```go
func removeProfanity(message *string) {
	if message != nil {
		return
	}
	// some logic
}
```

- Первый пример более читаемый, но лучше использовать второй пример в одну строку, где мы сразу разыменовываем указатель и присваиваем значение.

```go
package main
import (
	"fmt"
	"strings"
)

func main() {
	msg := "This is a fuck example."
	removeProfanity(&msg)
	fmt.Println(msg)
}

func removeProfanity(message *string) {
	messageVal := *message // Разыменовываю указать
	messageVal = strings.ReplaceAll(messageVal, "fuck", "***") // Новое значение
	*message := messageVal // Разыменованному указателю присваиваю значение
}

// В одну строку
func removeProfanity2(message *string) {
	*message = strings.ReplaceAll(*message, "fuck", "****")
}
```

>[!note]
>Использование указателей в структурах часто встречается для того, чтобы принимать аргументы в методе и присваивать их экземпляру структуры.

- Приемник с использованием указателя

```go
type car struct {
	color string
}

func (c *car) setColor(color string) {
	c.color = color
}

func main() {
	c := car{
		color: "white",
	}
	c.setColor("blue")
	fmt.Println(c.color)
	// prints "blue"
}
```

- Приемник без использования указателя

```go
type car struct {
	color string
}

func (c car) setColor(color string) {
	c.color = color // Здесь можно только c.[field] [yourCode]
}

func main() {
	c := car{
		color: "white",
	}
	c.setColor("blue")
	fmt.Println(c.color)
	// prints "white"
}
```

### Developing Project

- `go mod {remote}/{username}/{directory}`
- `go install` компилирует файл, который доступен глобально
- `go get` установка новых зависимостей

### Concurrency

- С помощью ключевого слова `go` мы можем создать новый goroutine (поток). Мы перескакиваем в основной goroutine (поток) и оттуда запускаем новый поток для функции, где мы использовали ключевое слово `go`

```go
func sendEmail(message string) {
	go func() {
		time.Sleep(time.Millisecond * 250)
		fmt.Printf("Email received: '%s'\n", message) // будет напечатано позже
	}()
	fmt.Printf("Email sent: '%s'\n", message)
}
```

- #Канал - это потокобезопасная очередь, с помощью которой мы можем считывать и обрабатывать результаты выполнения goroutine (потоков). Мы может итерироваться по каналу с помощью `range`

```go
ch := make(chan int, len([bufferSize])) // создание канала
close(ch) // закрытие канала
```

- Пока goroutine поток не закончит свое выполнение оператор `<-` не будет помещать данные в канал, также как и не будет возвращать данные из канала, пока они не появились

```go
ch <- 69 // передача данных в канал
v := <-ch // чтение данных из канала
```

- Если мы хотим проверить канал, можно добавить переменную `ok`, которая выведет `false` в том случае, если канал пустой и закрыт

```go
v, ok := <-ch
```

- Использование канала с goroutine

```go
func checkEmailAge(emails [3]email) [3]bool {
	isOldChan := make(chan bool)
	
	go sendIsOld(isOldChan, emails) // создание goroutine
	
	isOld := [3]bool{}
	isOld[0] = <-isOldChan
	isOld[1] = <-isOldChan
	isOld[2] = <-isOldChan
	return isOld
}

func sendIsOld(isOldChan chan<- bool, emails [3]email) {
	for _, e := range emails {
		if e.date.Before(time.Date(2020, 0, 0, 0, 0, 0, 0, time.UTC)) {
			isOldChan <- true
			continue
		}
		isOldChan <- false
	}
}
```

- Иногда у нас есть одна goroutine, прослушивающая несколько каналов, и мы хотим обрабатывать данные в том порядке, в котором они поступают по каждому каналу. Оператор `select`используется для прослушивания нескольких каналов одновременно.

- Случай `default`в `select`операторе выполняется _немедленно,_ если ни один другой канал не имеет готового значения, этот функционал используется если мы хотим отменить блокировку каналов.

```go
select {
case v1 := <-ch1:
    // use v1
case v2 := <-ch2:
	// use v2
default:
    // so do something else
}
```

- Можно помечать каналы только для чтения или только для записи

```go
func readCh(ch <-chan int) {
    // на чтение
}

func writeCh(ch chan<- int) {
    // на запись
}
```

### Mutexes

- Мьютексы позволяют заблокировать доступ к определенным данным. Структура данных `map` потоко-небезопасная, поэтому необходимо блокировать доступ к данным, пока горутина работает с ней.

- Объявление мьютекса

```go
import "sync"

mu := &sync.Mutex{}
```

- Реализация защищенной функции, которая блокирует данные для всех остальных горутин. То есть асинхронные функции выполняются одна за одной.

```go
func protected(mu *sync.Mutex){
    mu.Lock()
    defer mu.Unlock()
    // логика защищенной функции
}
```

- Если нам нужно читать данные из `map` из нескольких горутин одновременно, то мы можем использовать `sync.RWMutex`, который позволяет читать данные сразу нескольким горутинам

```go
type safeCounter struct {
	counts map[string]int
	mu     *sync.RWMutex
}

func (sc safeCounter) val(key string) int {
	sc.mu.RLock()
	defer sc.mu.RUnlock()
	return sc.counts[key]
}
```

### Generics

- Дженерики нужны для того, чтобы мы могли не писать несколько отдельных функций с одинаковой логикой для разных типов данных

```go
func splitAnySlice[T any](slice T) ([]T, []T) {
	mid := len(slice) / 2
	return slice[:mid], slice[mid:]
}

splitAnySlice([]int{6, 6, 6})
```

- Также типом для дженерика может быть интерфейс и если он имплементирует метод, который привязан к структуре, то мы можем принимать параметр в качестве аргумента с типом нашего дженерика и использовать метод из интерфейса для манипуляции с параметром.

```go
type stringer interface {
    String() string
}

func concat[T stringer](vals []T) string {
    result := ""
    for _, val := range vals {
        result += val.String()
    }
    return result
}
```