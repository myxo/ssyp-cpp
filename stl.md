
# STL - Standart Template Library

STL - стандартная библиотека шаблонов. В ней находятся многие основные алгоритмы и структуры данных.

Прежде чем начать изучать её, нужно посмотреть на то, что такое шаблоны.

---

## Шаблоны

Идея шаблонов довольно проста. Допустим мы хотим написать функцию для поиска максимума в массиве. Причем в нашей программе мы используем массивы int и float. 

```c++
int max(int *array, size_t size){
    int max = array[0];
    for (size_t i = 1; i < size; i++){
        if (array[i] > max) {
            max = array[i];
        }
    }
    return max;
}

float max(float *array, size_t size){
    float max = array[0];
    for (size_t i = 1; i < size; i++){
        if (array[i] > max) {
            max = array[i];
        }
    }
    return max;
}
```

Заметьте, что данные функции различаются *только* типами переменных. Почему бы не сделать одну функцию? 

--- 

В си для этого нужно было делать различные шаманства с #define. В С++ для этого сделали следующий синтаксис.

```C++
template <typename T>
T max(T *array, size_t size){
    T max = array[0];
    for (size_t i = 1; i < size; i++){
        if (array[i] > max) {
            max = array[i];
        }
    }
    return max;
}

int main(){
    {
        int array[5] = {1, 2, 3, 4, 5};
        printf("max value = %d\\n", max(array, 5));
    }
    {
        float array[5] = {1., 2., 3., 4., 5.};
        printf("max value = %f\\n", max(array, 5));
    }
}
```

---

Тоже самое можно делать с классами.

```C++
template <typename T>
class Node{
    T value;
    Node *next;
};

int main(){
    Node<float> head;
}
```

---

С шаблонами есть такая стандартная ошибка. Тут у класса А нет оператора <, поэтому скомпилироваться код не сможет.

```C++
Class A{
    int value = 5;
};

int main(){
    A array[5];
    max(array, 5);
}
```

---

Шаблоны - большая и сложная тема. Так как скорее всего мы в нашем проекте не будем писать свои шаблонные объекты (будет только использовать чужое), здесь и закончим. 

---

## STL
Основные составляющие STL
* контейнеры (хранение объектов в памяти)
* итераторы (доступ к элементам контейнера)
* алгоритмы
* адаптеры (обертки над контейнером)
* функциональные объекты, функторы (объекты, у которых перегружен operator() )

Общие методы контейнеров
* конструкторы + swap
* size, max_size, empty, clear
* begin, end, cbegin, cend, rbegin, rend
* == , !=

---

## Array
Класс-обертка над статическим массивом.
* operator[], at
* back, front
* fill
* data
Предоставляет интерфейс контейнера для обычного массива
```C++
#include <array>

std::array<std::string, 3> a = {"one", "two", "three"};
std::cout << a.size() << std::endl; // 3
std::cout << a[1] << std::endl;     // two
std::cout << a.at(3) << std::endl;  // runtime error
```
## Vector
Динамический массив. Если ему не хватает памяти, то автоматически выделяет.
* operator[], at
* resize
* capacity, reserve
* push_back, pop_back 
* data

```C++
#include <vector>

std::vector<std::string> v = {"one", "two"};
v.reserve(100);     // теперь в нем место для 100 элементов. Делать так необязательно,
                    // push_back выделил бы память, если ему не хватит
v.push_back("three");
std::cout << v.size()   << std::endl; // 3
std::cout << v[1]        << std::endl; // two
std::cout << v.at(3)    << std::endl; // runtime error
std::cout << v.at(100)  << std::endl; // runtime error
```

Зачастую vector эффективние любых других контенеров (на малых данных, подробнее когда будем говорить о кеш-памяти). 

**vector - ваша рабочая лошадка**, он используется в 90% случаев (ну может не 90, но очень часто).

---

## Итераторы
Итераторы нужны для доступа к элементам контейнеров более-менее единым образом. Для вектора итератор практически идентичен указателю.

Эти три куска кода семантически идентичны.
```C++
const int N = 10;
int array[N];
<...>
for (int i = 0; i < N; i++){
    cout << array[i];  // раскрывается как *(array + i)
}

int *begin = array, *end = array+N;
for (int *it = begin; it != end; it++){
    cout << *it;
}

std::vector<int> vec;
<...>
for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); it++){
    cout << *it;
}
```
А ещё мы можем написать аналогичное для list
```c++
std::list<int> list;
<...>
for (std::list<int>::iterator it = list.begin(); it != list.end(); it++){
    cout << *it;
}
```

---

## std::map
Ассоциативный массив, хранит пару (std::pair) key-value

```C++
std::map<std::string, int> student_to_ws
student_to_ws["Nord"] = 4;
student_to_ws["Vasya"] = 4;
student_to_ws["Pupkin"] = 6;
std::map<std::string, int>::iterator it = student_to_ws.find("Pupkin");
if (it != student_to_ws.end())
    std::cout << "Pupkin in " << it->second << " workshop\\n";
```

Кстати, вместо `std::map<std::string, int>::iterator it = ...` можно (и даже нужно) писать `auto it = ...`.

## std::map

Разница между `map<T, Y>` и `vector<std::pair<T, Y>>` в том, что map реализованна через двоичное дерево поиска, то поиск в мапе будет идти за O(log(n)), когда как в vector поиск идет за O(n).

```c++
auto it = student_to_ws.find("Nastya");
if (it != student_to_ws.end()){
    it->second = 4;
} else {
    student_to_ws.emplace("Nastya", 4);
}
// Это эквивалентно
student_to_ws["Nastya"] = 4;

// но тогда такая операция будет опасной, если поля Nastya нет
int nastya_ws = student_to_ws["Nastya"];
```

Если вы хотите использовать в мапе какой-то свой класс, то для него нужно определить operator< 

---
## std::shared_ptr

"Умный указатель". Работает как обычный указатель, но внутри подсчитывает сколько клиентов его использует. Когда количество клиентов достигает 0, вызывает delete. Крайне полезный класс, не допускает утечек памяти (кроме случая кольцевой зависимости)
```c++
{
    auto s = std::make_shared<std::string>("hello world");
    s->size(); // доступ через ->, как у указателей
}
// тут переменной s нет, память подчистилась

```

---

## Какие контейнеры ещё есть?

* list
* forward_list
* deque
* stack (адаптер)
* queue (адаптер)
* set
* unordered_map

---

## Range-based for
```C++
vector<int> vec();
<...>
for (int x: vec)
    cout << x;
```
Могут быть
* int x
* auto x
* auto &x
* const auto& x

---

# алгоритмы
`#include <algorithm>`

* min, max, minmax
* min_element, max_element, minmax_element
* find, find_if
* count, count_if
* fill, fill_n, generate, generate_n
* random_shuffle
* copy, copy_if
* remove, remove_if
* transform
* unique
* sort
