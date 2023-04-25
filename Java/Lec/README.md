# Инициализация 
Java поддерживает инварианты переменных. Читать переменную неинициализированную нельзя.

поля инициализируются конструкторами по умолчанию! для инта 0. можно прям в описании класса вызвать методы-инициализаторы. порядок инициализации сверху вниз (проверяется при компиляции)

## Секции инициализации
Выполняется до вызова конструктора сверху вниз.
Зачем их много? инициализация полей происходит до вызова конструктора. но вызывается по порядку сверху вниз, причем в очередности учитывается переменные (те они все в очереди). секуии инициализации не видят поля, которые объявлены после него
### Статическая секция
Статического конструктора нет, но есть статическая секция инициализации.

* вызывается не при загрузке класса, а когда дальше откладывать уже нельзя (обращение к полям)
* при этом если несколько статических блоков - они вызываются все (статическая инициализация неделима)
* статическая инициализация конкретного класса в рантайме вызывается единожды (имеем для класса флажочек)

* является элементом зла. Как написать программу, выводящую helloworld без main
```java
class A{
    static {
        s.o.p("...");
        system.exit(1);
        }
}
```
при попытке загрузить класс jvm вызывает main. до ее вызова нужно выполнить статическую инициализацию. но когда main не найден - вывод (в буфере) подавляется и не показывается пользователю.

system.exit(1); - показывает, что java запустилась с ошибкой и информация была полезной .поэтому ее бы вывести. сейчас исправили


### Нестатическая секция
Вызывается при инициализации объекта класса. 
Есть нестатическая секция - туда можно вынести общий код от всех конструкторов (а коснтурктор без парамтеров обладает отдельной логикой)

```java
class A {
    static final int i = 1;
    static{
        int j;
        ...
    }
}

class B{
    main(){
        A.i // константа времени компиляции, ее значение не меняется из-за того запущена инициализация или нет
        A.j //запустит?
        A a = new A();
    }
}
```
В плюсах все классы собрались бы в 1 бинарник и все статическое - общее.

1. JVM загрузила класс В, А у нее нет. 
2. Когда она ее видит - то загружает класса. Но статическую инициализацию делает не сразу. Она ее откладывает на как можно позже.

Можно запутсить явно как-то

инициализаци яполей и окнстуркторов выполняется перед вызовом конструктора при создании объекта класса

###  Наследование
при загрузке класса - 
1. загружаются все его родиели, 
2. сперва статические секции иницилизации родителей(НЕ ПРИ ЗАГРУЗКЕ! когда дальше откладывать уже нельзя)
3. статическая инициализация объекта (когда нужно)
4. инициализация полей и секции инициализации у родителей
5. инициализация полей и секций ребенка
6. конструктор родителя
7. конструктор ребенка

# Исключение
Причрины ошибок 
1. Непроверяемые исключения
* Ошибки программирввания
* runtime exception 
2. Проверяемые исключения
* системные ошибки (ошибки VM)

## Интересные жизненные ситуации с finally
 ```java
 try{
    throw new E1();
 }
 finally{
    throw E2(); //наружу торчит это, Е исчезает
    //отвратительный код, лучше 
    //throw E2(Е1)
 }
 ```

 try with resources. ошибки
 1. При открытии
 2. При работе
 3. При закрытии (finally)
 4. Одновременно (часто с файлами)    
 отркыли файл, место кончилось. при закрытии буферы сбрасывают на диск, а места нет  - еще одно искдючение. Поэтому кто-то из них должен вывалиться наружу.

 try with resources если есть несколько исключений - бросается то, что было при работе. то, что было при закрытии будет добавлено как подавленное (supressed)

 * Если в конструкторе бросатеся проверяемое исключение - нужно ли его помечать? Да

 * Если в секции инициализации - все плохо, АМ не знает, но скорее всего не скомпилируется
* метод библиотеки бросает исключение, потом на горячую меняем на другую, которая бросает другое проверяемое исключение? для смены нужен reflection, поговорим про это когда пройдем его

Почему работа на исключениях плоха?
* долго: разворачивается стек для поиска обработчика
* плохочитаемо: не понятно, докуда эта штука будет ползти