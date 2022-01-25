# АЯ Экзамен

## 1. Двусвязный список: поиск элемента по значению, вставка элемента, удаление элемента. Реализация функции удаления элемента из двусвязного списка. Основные методы std::list. Пример работы с std::list.

### Двухсвязный список - список в котором у каждого элемента есть два указателя: на следующий элемент и на предыдущий. Двухсвязный список бывает двух видов:
1. Линейный - В этом списке у первого элемента нет числа позади него, поэтому указатель на предыдущий элемент указывает на nullptr, также у последнего элемента нет следующего элемента, поэтому указатель на следующий элемент указывает на nullptr.
2. Циклический - В этом списке первый элемент указывает на последний, а последний на первый.

### Поиск элемента по значению:
Чтобы найти элемент по значению, нам нужно "пробежаться" по элементам списка с помощью указателей, и найти нужный нам элемент.

### Вставка элемента:
Чтобы вставить элемент в список нам нужно "переопределить указатели", тут надо рассматривать две возможных списка:
1. Линейный: 

А) Если вставляемый элемент станет последним, то указатель прошлого элемента, указывающего на nullptr, будет указывать на наш новый элемент, новый элемент будет указывать на предыдущий и т.к. нет следующего, будет указывать на nullptr. 

Б) Если вставляемый элемент станет первым, то указатель прошлого элемента, указывающего на nullptr, будет указывать на наш новый элемент, новый элемент будет указывать на следующий и т.к. нет пердыдущего, будет указывать на nullptr.

В) Если вставляемый элемент не станет первым и не станет последним, то он должен будет указывать на следующий и предыдущий элементы, в свою очередь два элемент до этого стоящие рядом, больше не должны указывать друг на друга, теперь они указывают на новый элемент.

2. Двухсвязный:

Вставляемый элемент должен будет указывать на следующий и предыдущий элементы, в свою очередь два элемент до этого стоящие рядом, больше не должны указывать друг на друга, теперь они указывают на новый элемент.

### Удалением элемента:
Чтобы найти элемент по значению, нам нужно "пробежаться" по элементам списка с помощью указателей, найти нужный нам элемент и удалить его, после чего, два соседних элемента должны будут указывать друг на друга. Если одного соседнего элемента нет, то указатель будет указывать на nullptr. (Не забываем, что нам нужно будет почистить память удаляемого элемента)

### Сложности основных алгоритмов

1. Поиск - O(n)
2. Вставка - O(n)
3. Удалениие - O(n)
4. Сортировка - O(n^2)

### Реализация функции удаления элемента:
```cpp
#include <iostream>

struct node { //узел
    int data;
    node* next;
    node* prev;
};

struct List {
    size_t size;
    node* head;
    node* tail;
};

void deleteNth(List* _list, int n_) {
    node* fir = _list->head;
    while (fir != nullptr || fir->data != n_) {//Тут мы ищем наш элемент
        fir = fir->next;
    }
    if (fir == _list->head) {//Если наш элемент первый
        _list->head = fir->next; //указатель на голову становится равным указателю на второй элемент
        _list->head->prev = nullptr;//Указатель второй элемента в списке равен nullptr
    }
    else if (fir == _list->tail) {
        _list->tail = fir->prev;
        _list->tail->next = nullptr; //Указатель предпоследнего элемента в списке равен nullptr

    }
    else if (fir != nullptr) {
        fir->prev->next = fir->next;//боковой элемент слева от удаляемого элемент указывает на правый элемент
        fir->next->prev = fir->prev; //Тоже самое наоборот
    }
    delete fir;
    _list->size--;
}
```

### std::list - контейнер, двухсвязный.

| Метод | Функция |
| --- | --- |
| empty | Проверяет отсутствие элементов в контейнере |
| size | Возвращает количество элементов в контейнере |
| max_size | Возвращает максимально допустимое количество элементов в контейнере |
| clear | Очищает контейнер |
| insert | Вставляет элементы |
| emplace  (C++11)  | Конструирует элементы "на месте" и вставляет их начиная с заданной позиции pos |
| erase | Удаляет элементы
| push_back | добавляет элемент в конец |
| emplace_back  (C++11) | Конструирует элементы "на месте" в конце контейнера |
| pop_back | Удаляет последний элемент |
| push_front | вставляет элементы в начало списка |
| emplace_front  (C++11) | конструирует элементы "на месте" в начало списка |
| pop_front | удаляет первый элемент |
| resize | Изменяет количество хранимых элементов |
| swap | Обменивает содержимое |
| merge | слияние двух отсортированных списков |
| splice | перемещает элементы из другого list |
| remove / remove_if | удаляет элементы, удовлетворяющие определенным критериям |
| reverse | инвертирует порядок элементов |
| unique | удаляются последовательно повторяющиеся элементы |
| sort | сортирует элементы |

### Пример работы:
```cpp
#include <iostream>
#include <list>

int main()
{
    std::list<int> L = { 1,2,3,4,3,2,1,10,11,12 };

    if (L.empty()) {
        std::cout << "Empty" << std::endl;
    }
    else {
        std::cout << "Not empty" << std::endl;
    }

    int size1 = L.size(); // 10

    int size2 = L.max_size(); //357913941

    L.clear(); //{}

    L = { 1,2,3,4,3,2,1,10,11,12 };

    auto iter1 = L.cbegin(); //указывает на первый элемент
    auto iter2 = L.cbegin(); //указывает на первый элемент существует ещё cend()
    L.insert(iter1, 1); //Добавляем 1 в начало списка
    L.insert(++iter1, 1); //Добавляем 1 в после 1 элемента
    L.insert(++iter2, 3, 4);//Три 4 после первого элемента

    std::list<int> L = { 1, 2, 3, 4, 5 };
    auto iter = ++L.cbegin(); // итератор указывает на второй элемент
    L.emplace(iter, 8); // добавляем после первого элемента  numbers = { 1, 8, 2, 3, 4, 5};

    L = { 1, 2, 3, 4, 5 };
    auto iter = L.cbegin(); // указатель на первый элемент
    L.erase(iter);    // удаляем первый элемент
    // numbers = { 2, 4, 5, 6 }

    L = { 1, 2, 3, 4, 5 };
    auto begin = L.begin(); // указатель на первый элемент
    auto end = L.end();       // указатель на последний элемент
    L.erase(++begin, --end);  // удаляем со второго элемента до последнего

    std::list<int> numbers = { 1, 2, 3, 4, 5 };
    numbers.push_back(23);  // { 1, 2, 3, 4, 5, 23 }
    numbers.push_front(15); // { 15, 1, 2, 3, 4, 5, 23 }
    numbers.emplace_back(24);   // { 15, 1, 2, 3, 4, 5, 23, 24 }
    numbers.emplace_front(14);  // { 14, 15, 1, 2, 3, 4, 5, 23, 24 }

    std::list<int> numbers = { 1, 2, 3, 4, 5, 6 };
    numbers.resize(4);  // оставляем первые четыре элемента - numbers = {1, 2, 3, 4}
    numbers.resize(6, 8);    // numbers = {1, 2, 3, 4, 8, 8}

    std::list<int> list1 = { 1, 2, 3, 4, 5 };
    std::list<int> list2 = { 6, 7, 8, 9 };
    std::list<int> list3 = {};
    list1.swap(list2);
    // list1 = { 6, 7, 8, 9};
    // list2 = { 1, 2, 3, 4, 5 };

    list3.merge(list1, list2); //{1, 2, 3, 4, 5, 6, 7, 8, 9}
    list3.reverse();//{От 9 до 1}

    std::list<int> list4 = { 1, 1, 2, 4, 2 };
    list4.unique(); // 1 2 4 2

    list4.sort(); // 1 2 2 4

    int array[10] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 };
    std::list<int>ilist1(array, array + 10);
    std::list<int>ilist2(array, array + 2); // содержит 0, 1
}

```
## 2. Класс std::map. Внутренняя реализация map, его основные методы. Сложность поиска, сортировки, удаления элемента, добавления элемента. Пример работы с std::map

### std::map
Контейнер map, очень похож на остальные контейнеры, такие как vector, list, deque, но с небольшим отличием. В этот контейнер можно помещать сразу два значения. Если у Вас когда-то была мечта написать свой словарь, то лучше чем map, вам альтернативы не найти.

Умным языком:

std::map — отсортированный ассоциативный контейнер, который содержит пары ключ-значение с неповторяющимися ключами. Порядок ключей задаётся функцией сравнения Compare. Данный тип, как правило, реализуется как красно-чёрное дерево.

Красно-чёрное дерево — один из видов из самобалансирующихся двоичных деревьев поиска, гарантирующих логарифмический рост высоты дерева от числа узлов и позволяющее быстро выполнять основные операции дерева поиска: добавление, удаление и поиск узла. Сбалансированность достигается за счёт введения дополнительного атрибута узла дерева — «цвета». Этот атрибут может принимать одно из двух возможных значений — «чёрный» или «красный».

### Внутренняя реализация 
Внутренняя реализация map представляет собой двоичное сбалансированное дерево (красно-черное дерево); внутренняя hash_map - это hash_table, которая обычно реализуется с помощью большого вектора, узлы элементов вектора могут быть связаны со связанным списком для разрешения конфликтов.

 ![image](https://user-images.githubusercontent.com/74144153/150693576-3541539d-44b1-4676-bfab-3a153a3b34b2.png)


Процесс вставки hash_map:
1. Получите ключ
2. Получите хеш-значение с помощью хеш-функции.
3. Получите номер сегмента (обычно по модулю количества сегментов для хеш-значения).
4. Сохраните ключ и значение в сегменте.

Ценностный процесс:
1. Получите ключ
2. Получите хеш-значение с помощью хеш-функции.
3. Получите номер сегмента (обычно по модулю количества сегментов для хеш-значения).
4. Сравните, равны ли внутренние элементы сегмента ключу. Если они не равны, он не будет найден.
5. Получите значение одинаковой записи.

### Основыне методы

| Метод | Функция |
| --- | --- |
| at  (C++11) | Предоставляет доступ к указанному элементу с проверкой индекса |
| operator[] | Предоставляет доступ к указанному элементу |
| empty | Проверяет отсутствие элементов в контейнере |
| size | Возвращает количество элементов в контейнере |
| max_size | Возвращает максимально допустимое количество элементов в контейнере |
| clear | Очищает контейнер |
| insert | Вставляет элементы |
| emplace  (C++11) | Конструирует элементы "на месте" и вставляет их начиная с заданной позиции pos |
| emplace_hint  (C++11) | Элементы конструкций на месте использования подсказки |
| erase | Удаляет элементы |
| swap | Обменивает содержимое |
| count | Возвращает количество элементов, соответствующих определенному ключу |
| find | находит элемент с конкретным ключом |
| equal_range | возвращает набор элементов для конкретного ключа |
| lower_bound | возвращает итератор на первый элемент не меньше, чем заданное значение |
| upper_bound | возвращает итератор на первый элемент больше, чем определенное значение |
| key_comp | возвращает функцию, сравнивающую ключи |
| value_comp | возвращает функцию, сравнивающую значения |

### Сложности основных алгоритмов

1. Поиск по ключу - O(log(n))
2. Сортировка - O
3. Вставка - O(log(n))
4. Удаление - O(log(n))

### Пример использования

```cpp
int main()
{
    std::map<std::string, std::string>book = { {"Hi", "Привет"},
                             {"Student", "Студент"},
                             {"!", "!"} 
    };
    std::cout << book["Hi"] << std::endl;

    std::map<std::string, int> mymap = {
                { "alpha", 0 },
                { "beta", 0 },
                { "gamma", 0 } 
    };
    mymap.at("alpha") = 10;
    mymap.at("beta") = 20;
    mymap.at("gamma") = 30;
    for (auto& x : mymap) {
        std::cout << x.first << ": " << x.second << '\n';
    }

    std::map<char, int> letter_counts{ {'a', 27}, {'b', 3}, {'c', 1} };
    letter_counts['b'] = 42;  // updates an existing value
    letter_counts['x'] = 9;  // inserts a new value
    for (auto& x : letter_counts) {
        std::cout << x.first << ": " << x.second << '\n';
    }

    std::map<char, int> mymap{ {'a', 27}, {'b', 3}, {'c', 1} };
    mymap['a'] = 10;
    while (!mymap.empty())
    {
        std::cout << mymap.begin()->first << " => " << mymap.begin()->second << '\n';
        mymap.erase(mymap.begin());
    }


    std::map<char, int> mymap;
    mymap['a'] = 101;
    mymap['b'] = 202;
    mymap['c'] = 302;
    std::cout << "mymap.size() is " << mymap.size() << '\n';

    int i;
    std::map<int, int> mymap;
    if (mymap.max_size() > 1000)
    {
        for (i = 0; i < 1000; i++) mymap[i] = 0;
        std::cout << "The map contains 1000 elements.\n";
    }
    else std::cout << "The map could not hold 1000 elements.\n";

    mymap.clear();

    std::map<char, int> mymap;

    // first insert function version (single parameter):
    mymap.insert(std::pair<char, int>('a', 100));
    mymap.insert(std::pair<char, int>('z', 200));
    std::pair<std::map<char, int>::iterator, bool> ret;
    ret = mymap.insert(std::pair<char, int>('z', 500));
    if (ret.second == false) {
        std::cout << "element 'z' already existed";
        std::cout << " with a value of " << ret.first->second << '\n';
    }

    std::map<char, int> mymap;
    mymap.emplace('x', 100);
    mymap.emplace('y', 200);
    mymap.emplace('z', 100);
    std::cout << "mymap contains:";
    for (auto& x : mymap)
        std::cout << " [" << x.first << ':' << x.second << ']';
    std::cout << '\n';

    std::map<char, int> mymap;
    auto it = mymap.end();
    it = mymap.emplace_hint(it, 'b', 10);
    mymap.emplace_hint(it, 'a', 12);
    mymap.emplace_hint(mymap.end(), 'c', 14);
    std::cout << "mymap contains:";
    for (auto& x : mymap)
        std::cout << " [" << x.first << ':' << x.second << ']';
    std::cout << '\n';

    std::map<char, int> mymap;
    std::map<char, int>::iterator it;

    // insert some values:
    mymap['a'] = 10;
    mymap['b'] = 20;
    mymap['c'] = 30;
    mymap['d'] = 40;
    mymap['e'] = 50;
    mymap['f'] = 60;
    it = mymap.find('b');
    mymap.erase(it);                   // erasing by iterator
    mymap.erase('c');                  // erasing by key
    it = mymap.find('e');
    mymap.erase(it, mymap.end());    // erasing by range

    std::map<char, int> foo, bar;
    foo['x'] = 100;
    foo['y'] = 200;
    bar['a'] = 11;
    bar['b'] = 22;
    bar['c'] = 33;
    foo.swap(bar);

    std::map<char, int> mymap;
    char c;
    mymap['a'] = 101;
    mymap['c'] = 202;
    mymap['f'] = 303;
    for (c = 'a'; c < 'h'; c++)
    {
        std::cout << c;
        if (mymap.count(c) > 0)
            std::cout << " is an element of mymap.\n";
        else
            std::cout << " is not an element of mymap.\n";
    }

    std::map<char, int> mymap;
    std::map<char, int>::iterator it;
    mymap['a'] = 50;
    mymap['b'] = 100;
    mymap['c'] = 150;
    mymap['d'] = 200;
    it = mymap.find('b');
    if (it != mymap.end())
        mymap.erase(it);
    // print content:
    std::cout << "elements in mymap:" << '\n';
    std::cout << "a => " << mymap.find('a')->second << '\n';
    std::cout << "c => " << mymap.find('c')->second << '\n';
    std::cout << "d => " << mymap.find('d')->second << '\n';

    std::map<char, int> mymap;
    mymap['a'] = 10;
    mymap['b'] = 20;
    mymap['c'] = 30;
    std::pair<std::map<char, int>::iterator, std::map<char, int>::iterator> ret;
    ret = mymap.equal_range('b');
    std::cout << "lower bound points to: ";
    std::cout << ret.first->first << " => " << ret.first->second << '\n';
    std::cout << "upper bound points to: ";
    std::cout << ret.second->first << " => " << ret.second->second << '\n';

    std::map<char, int> mymap;
    std::map<char, int>::iterator itlow, itup;

    mymap['a'] = 20;
    mymap['b'] = 40;
    mymap['c'] = 60;
    mymap['d'] = 80;
    mymap['e'] = 100;
    itlow = mymap.lower_bound('b');  // itlow points to b
    itup = mymap.upper_bound('d');   // itup points to e (not d!)
    mymap.erase(itlow, itup);        // erases [itlow,itup)
    // print content:
    for (std::map<char, int>::iterator it = mymap.begin(); it != mymap.end(); ++it)
        std::cout << it->first << " => " << it->second << '\n';

    std::map<char, int> mymap;
    std::map<char, int>::key_compare mycomp = mymap.key_comp();
    mymap['a'] = 100;
    mymap['b'] = 200;
    mymap['c'] = 300;
    std::cout << "mymap contains:\n";
    char highest = mymap.rbegin()->first;     // key value of last element
    std::map<char, int>::iterator it = mymap.begin();
    do {
        std::cout << it->first << " => " << it->second << '\n';
    } while (mycomp((*it++).first, highest));
    std::cout << '\n';

    std::map<char, int> mymap;
    mymap['x'] = 1001;
    mymap['y'] = 2002;
    mymap['z'] = 3003;
    std::cout << "mymap contains:\n";
    std::pair<char, int> highest = *mymap.rbegin();          // last element
    std::map<char, int>::iterator it = mymap.begin();
    do {
        std::cout << it->first << " => " << it->second << '\n';
    } while (mymap.value_comp()(*it++, highest));
}
```

## 3. Класс std::set. Внутренняя реализация set, его основные методы. Сложность поиска, сортировки, удаления элемента, добавления элемента. Пример работы с std::set
### std::set

## 4. Класс std::unordered_map. Внутренняя реализация unordered_map, его основные методы. Сложность поиска, сортировки, удаления элемента, добавления элемента. Пример работы с std::unordered_map
















































































































































































































































































































































































































