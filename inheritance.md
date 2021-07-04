# Наследование в С++

------------

```C++
class Parent {
    string parent_value;
    Parent() { parent_value = "parent"; }
};

class Child : public Parent {
    string child_value;
    Child() { child_value = "child"; }
};

int main(){
    Child child;
    cout << child.child_value << " and " << child.parent_value << endl;
}
```

```C++
class Parent {
    string parent_value;
    Parent() { parent_value = "parent"; }
};

class Child : public Parent {
    string child_value;
    Child(string child_val) { child_value = child_val; }
};

int main(){
    Child child("child");
    cout << child.child_value << " and " << child.parent_value << endl;
}
```

------------


```C++
class Parent {
    string parent_value;
    Parent() { parent_value = "parent"; }
};

class Child : public Parent {
    string child_value;
    Child(string child_val) : child_value(child_val) { }
};

int main(){
    Child child("child");
    cout << child.child_value << " and " << child.parent_value << endl;
}
```

------------


```C++
class Parent {
    string parent_value;
    Parent(string parent_val) 
        : parent_value(parent_val)
    { }
};

class Child : public Parent {
    string child_value;
    Child(string child_val, string parent_val) 
        : Parent(parent_val)
        , child_value(child_val)
    { }
};

int main(){
    Child child ("child", "parent");
    cout << child.child_value << " and " << child.parent_value << endl;
}
```
