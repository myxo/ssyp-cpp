# Классы в C++

Первый уровень сложности: классы - просто более удобные структуры из си.

Вот пример того, как мы работали со структурами в си.

```C
typedef struct SupperDupper{
    int a;
    double b;
    int *array;
} SupperDupper;

bool initialize(SupperDupper *sd, int size){
    sd->array = (int*)malloc(sizeof(int)*size);
}
bool deinitialize(SupperDupper *sd){
    free(ds->array);
}

int main(){
    SupperDupper sd;
    initialize(&sd, 5);
    ...
    deinitialize(&sd)
}
```

---

Аналогичная конструкция, но теперь с классами

```C++
class SupperDupper{
    int a;
    double b;
    int *array;

    SupperDupper(int size){
        array = new int[size];
    }
    ~SupperDupper(){
        delete[] array;
    }
};

int main(){
    SupperDupper sd(5);
}
```

---

## Конструктор

* Конструкторы - метод класса, который **гарантированно** вызовется при создании класса, то есть в этих случаях.
```c++
SupperDupper sd(5);
SupperDupper *sd = new SupperDupper(5);
```
* Конструктор всегда имеет то же имя, что и класс (такой синтаксис)
* Конструкторы могут принимать различные параметры, по которым им дают разные имена

```c++
SupperDupper();                         // Конструктор по-умолчанию
SupperDupper(size_t size);              // Конструктор с параметрами
SupperDupper(const SupperDupper& sd);   // Конструктор копирования

String(const char*);                    // Конструктор преобразования
```

* Конструктор по-умолчанию и конструктор копирования создается всегда, просто если вы его не задали, компилятор считает, что он пустой. 

Стоп, а так можно вообще?

---

### Перегрузка функций

В С++ можно иметь множество функций (и методов класса), которые имеют одно и то же название. Как их различают? По входящим параметрам (именно по входящим, а не исходящим)
```C++
int sum(int a, int b);
double sum(double a, double b);
int sum(double a, double b); // а вот так уже нельзя
```

--- 

## Деструктор

* Деструктор - функция, которая вызывается в месте, где нужно уничтижить объект. То есть при закрытии фиг. скобок или при вызове delete
```C++
{
    SupperDupper sd;
} // здесь вызовется деструктор SupperDupper
{
    SupperDupper *sd = new SupperDupper();
    delete sd; // вызов деструктора SupperDupper
} // а здесь вызвается деструктор SupperDupper*, а не самого класса
```

* Синтаксис - тильда + название класса

## public, private, protected

* У классов можно определять какие методы и поля класса могут получить извне
```c++
class A{
    int v1;
public:
    int v2;
private:
    int v3;
};

int main(){
    A a;
    a.v1 = 5; // ошибка, по-умолчанию все в классах private
    a.v2 = 6; // ок
    a.v3 = 7; // ошибка, попытка обращения к private
}
```
* А в структурах по-умолчанию все public
* Про protected поговорим потом (в теме  наследование)
