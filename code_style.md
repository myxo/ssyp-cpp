# Code style

* В строке должно быть не более 120 символов (сделайте настройку в редакторе)
* Пробелы между операторами
```c++
int a = 3+b+foo();         // плохо
int c = 3 + b + bar();     // ok
do_something(a, b, c, d);   // NOT: doSomething(a,b,c,d);
```
* Названия классов и типов - PascalCase, названия переменных и функций - underscore_case.
* Названия должны быть понятными, без сокращений (да пребудет с вами автокомплит). Названия функций начинаются с глагола
```c++
bool id(int i){...}                                             // непонятно, плохо
bool set_id_if_not_equal(int object_id) {}     // хорошо
```
* Если у вас длинный if - разбивайте его по строчкам
* внутри namespaces не делаем отступов
```c++
namespace example {

code;
moreCode;

} // namespace example
```
* Объявление класса должно иметь следующий вид
```c++
class SomeClass : public BaseClass
{
public:
  ... <public methods> ...
protected:
  ... <protected methods> ...
private:
  ... <private methods> ...

public:
  ... <public data> ...
protected:
  ... <protected data> ...
private:
  ... <private data> ...
};
```
* При множественном наследовании
```c++
class SomeClass 
    : public BaseClass1
    , public BaseClass2
    , public BaseClass3
{
};
```
* Фигурная скобка на этой же линии
```c++
for (initialization; condition; update) {
  statements;
}
```
* Исключение - конструкторы с инициализацией параметров
```c++
SomeClass::SomeClass(int value, const std::string& string)
  : value_(value)
  , string_(string)
  ...
{
}
```
* Приватные переменные класса должны иметь подчеркивание в конце
```c++
class SomeClass {
public:
    int public_variable;
private:
    int private_variable_;
};
```
* Если метод класса не меняет внутренних переменных, то он должен быть константным
```c++
struct SomeClass {
    bool is_deleted() const {
        return deleted_;
    }
};
```
