# Виртуальные функции


```C++
class Parent {
    void foo() { 
        printf("Foo from parent"); 
    }
};

class Child : public Parent {};

int main(){
    Child child;
    child.foo();    // Foo from parent
    Parent parent;
    parent.foo();   // Foo from parent
}
```

---

А вот так мы можем переопределить функцию с такой же сигнатурой.

```C++
class Parent {
    void foo() { 
        printf("Foo from parent"); 
    }
};

class Child : public Parent {
    void foo() { 
        printf("Foo from child"); 
    }
};

int main(){
    Child child;
    child.foo();    // Foo from child
    Parent parent;
    parent.foo();   // Foo from parent
}
```

---

## Ключевое слово virtual

```C++
class Parent {
    virtual void foo() { 
        printf("Foo from parent"); 
    }
};

class Child : public Parent {
    void foo() override {
        printf("Foo from child"); 
    }
};

int main(){
    Child child;
    child.foo();    // Foo from child
    Parent parent;
    parent.foo();   // Foo from parent

    Parent ref1& = parent;
    ref1.foo();     // Foo from parent
    Child ref2& = child;
    ref2.foo();     // Foo from child

    Parent ref3& = child;
    ref3.foo();     // Foo from child
    return 0;
}
```

---

## Для чего это может быть нужно?

```C++
struct Shape{ virtual void draw() = 0; };

struct Box : Shape { void draw() override { ... }};
struct Circle : Shape { void draw() override { ... }};
struct Line : Shape { void draw() override { ... }};

int main(){
    std::vector<Shape*> shape_vector;
    ...
    for (const auto& shape: shape_vector){
        shape->draw();
    }
}
```
