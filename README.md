# Лабораторная работа №4: Наследование в C++
Здесь вы сможете ознакомиться с одной из парадигм ООП - наследованием.  
Ниже представлен пример кода на C++, демонстрирующий основные механизмы наследования: базовый класс, производный класс, переопределение методов, виртуальные функции и полиморфизм

## Описание
Данный проект демонстрирует механизм наследования в языке C++ на примере иерархии классов:
- `Base` – базовый класс с виртуальными методами.
- `Derived` – класс, публично наследующий от `Base` с переопределением метода `showType()`.
- `ExtendedDerived` – класс, наследующий от `Derived` (второй уровень).

## Что показывает код
- Синтаксис публичного наследования (`class Derived : public Base`).
- Вызов методов базового класса из производного.
- Переопределение виртуальных методов (ключевое слово `override`).
- Полиморфизм: использование указателей и ссылок на базовый класс для работы с производными объектами.
- Виртуальный деструктор для корректного освобождения памяти.
- Возможность создания полиморфных контейнеров (массив указателей на `Base`).

## Компиляция и запуск
### Требования
- Компилятор с поддержкой C++11 (g++, clang++, MSVC).

### Команды (Linux)
```bash
g++ main.cpp -o inheritance_demo
./inheritance_demo
```

**`main.cpp`**
```main.cpp
//Пример наследования в C++: Базовый класс Animal и производные Dog, Cat


#include <iostream>
#include <string>
#include <vector>

// Базовый класс
class Animal {
protected:
    std::string name;
public:
    Animal(const std::string& n) : name(n) {
        std::cout << "Animal constructor: " << name << std::endl;
    }

    // Виртуальный деструктор для корректного удаления через указатель на базовый класс
    virtual ~Animal() {
        std::cout << "Animal destructor: " << name << std::endl;
    }

    // Виртуальный метод, будет переопределён в производных классах
    virtual void speak() const {
        std::cout << name << " makes a sound." << std::endl;
    }

    // Не виртуальный метод – не переопределяется (скрывается)
    void eat() const {
        std::cout << name << " is eating." << std::endl;
    }

    std::string getName() const { return name; }
};

// Производный класс Dog
class Dog : public Animal {
private:
    std::string breed;
public:
    Dog(const std::string& n, const std::string& b) : Animal(n), breed(b) {
        std::cout << "Dog constructor: " << name << " (" << breed << ")" << std::endl;
    }

    ~Dog() override {
        std::cout << "Dog destructor: " << name << std::endl;
    }

    void speak() const override {
        std::cout << name << " barks: Woof! Woof!" << std::endl;
    }

    void wagTail() const {
        std::cout << name << " wags tail." << std::endl;
    }
};

// Производный класс Cat
class Cat : public Animal {
public:
    Cat(const std::string& n) : Animal(n) {
        std::cout << "Cat constructor: " << name << std::endl;
    }

    ~Cat() override {
        std::cout << "Cat destructor: " << name << std::endl;
    }

    void speak() const override {
        std::cout << name << " meows: Meow! Meow!" << std::endl;
    }

    void purr() const {
        std::cout << name << " purrs." << std::endl;
    }
};

// Демонстрация полиморфизма – функция принимает указатель на базовый класс
void makeAnimalSpeak(const Animal* animal) {
    animal->speak();
}

int main() {
    std::cout << "=== Demonstration of inheritance ===" << std::endl;

    // Прямое использование
    Dog dog("Rex", "German Shepherd");
    Cat cat("Whiskers");

    dog.speak();
    cat.speak();
    dog.eat();      // унаследованный метод
    cat.eat();
    dog.wagTail();
    cat.purr();

    std::cout << "\n=== Polymorphism via base pointer ===" << std::endl;
    // Указатели на базовый класс, реально указывающие на производные объекты
    Animal* ptr1 = new Dog("Buddy", "Labrador");
    Animal* ptr2 = new Cat("Mittens");

    makeAnimalSpeak(ptr1);
    makeAnimalSpeak(ptr2);

    // Вызов не виртуального метода – определяется типом указателя
    ptr1->eat();   // Animal::eat()
    ptr2->eat();

    // Чтобы вызвать специфичные методы, нужно привести тип (опасно, но для демонстрации)
    // static_cast<Dog*>(ptr1)->wagTail();

    std::cout << "\n=== Vector of base pointers ===" << std::endl;
    std::vector<Animal*> animals;
    animals.push_back(new Dog("Max", "Beagle"));
    animals.push_back(new Cat("Lucy"));
    animals.push_back(new Dog("Rocky", "Bulldog"));

    for (const auto& a : animals) {
        a->speak();   // полиморфный вызов
    }

    // Освобождение памяти (в реальном коде лучше использовать умные указатели)
    for (auto a : animals) {
        delete a;
    }
    delete ptr1;
    delete ptr2;

    return 0;
}
```


Код компилируется с любым современным компилятором (требуется поддержка C++11, но можно убрать override и сузить до C++98).
