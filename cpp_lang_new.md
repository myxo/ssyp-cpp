# C++ lang (что внесено нового в С++)

## Standart IO
Аналог `fprintf()`  `scanf()`
```c++
#include <iostream>

int main(){
  std::cout << "hello world" << std::endl; // вместо std::endl вполне нормально писать "\n"
  int a;
  std::cin >> a;
}
```

## File IO
```c++
#include <fstream>
#include <iostream>

int main(){
  std::ifstream f_in("filename.txt");
  while(!f_in.eof()){
    std::string word;
    f_in >> word;
    std::cout << word << "\\n";
  }
  
  std::ofstream f_out("filename_out.txt");
  int a = 5;
  f_out << "Hello file\\n" << "var a == " << a;
}
```

## Пространства имен
```c++
#include <iostream>

using namespace std;

int main(){
  cout << "hello world" << endl;
}
```

Допустим у нас есть две структуры `Point`, одна наша, а другая из библиотеки.
```c++
#include "point.h"

struct Point{
  int x,y;
};

int main(){
  Point p; // какая это именно структура???
}
```

Такие вещи случаются часто когда у вас мноого кода и вы подключаете боольшие библиотеки. Решение - разнести по разным пространствам.

```c++
#Include "point.h" // внутри реализована namespace lib
namespace my_math{
  struct Point{
    int x,y;
  };
}
int main(){
  lib::Point p1;       // структура из библиотеки
  my_math::Point p2;   // а это наша
}
```

## Выделение памяти
```c++
int *a_malloc = (int*)malloc(sizeof(int));
*a_malloc = 5;
free(a_malloc);

int *a_new = new int(5);
delete a_new;
```
Но мы прямое выделение памяти использовать не будем (об этом будем говорить в контексте умных указателей)!

## Ссылки
Помимо указателей в c++ существует похожая сущность - ссылки.
```c++
int a = 5;
int *a_p = &a;
int &a_ref = a;

*a_p = 6;
a_ref = 7;  // значение переменной a также меняется
```
По-сути отличие от указателей в синтаксисе и в том, что ссылке обязательно должно быть присвоено значение. 
```c++
int *a; // так можно, в указателе будет мусор
int &a; // а так уже нельзя, ошибка компиляции
```

## Перегрузка функций
```c++
int foo(int a);
int foo(double a);
double foo(double a); // ошибка компиляции
```

## auto
```c++
  int a = 5;
  auto b = a;
  auto c = foo();
```