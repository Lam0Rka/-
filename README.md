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

### std::set - это контейнер, который автоматически сортирует добавляемые элементы в порядке возрастания. Но при добавлении одинаковых значений, set будет хранить только один его экземпляр. По другому его еще называют множеством. "Плюсы" set: быстрая сортировка, "Минусы" set: нельзя обратиться к конкретной ячейке по индексу. В таких случаях лучше std::vector
![image](https://user-images.githubusercontent.com/74144153/150984948-4ee96ae2-1166-420b-ab6b-681814c9005d.png)
![image](https://user-images.githubusercontent.com/74144153/150986747-1dc62e30-f046-4526-a404-8a336bd5023e.png)
std::set - множество. Элементы уникальны, а так же сравниваются и сортируются при добавлении. Чаще всего реализовано так же как и std::map с помощью красно-черных деревьев.

| Метод | Функция |
| --- | --- |
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
| [] greater | изменить строку в обратную сторону при сортировке |
| [] copy | вывод элементов контейнера |

### Сложности основных алгоритмов

1. Поиск - O(log(n))
2. Вставка - O(log(n))
3. Удаление - O(log(n))
4. Добавление элемента - O(log(n))

### Пример работы
```cpp
#include <iostream>
#include <set>

int main ()
{
  std::set<int> myset;
  myset.insert(20);
  myset.insert(30);
  myset.insert(10);
  std::cout << "myset contains:";
  while (!myset.empty())
  {
     std::cout << ' ' << *myset.begin();
     myset.erase(myset.begin());
  }
  std::cout << '\n';
  
  for (int i=0; i<10; ++i) myints.insert(i);
  std::cout << "1. size: " << myints.size() << '\n';
  
  int i;
  std::set<int> myset;
  if (myset.max_size()>1000)
  {
    for (i=0; i<1000; i++) myset.insert(i);
    std::cout << "The set contains 1000 elements.\n";
  }
  else std::cout << "The set could not hold 1000 elements.\n";
  
  myset.clear();
  
  std::set<std::string> myset;
  myset.emplace("foo");
  myset.emplace("bar");
  auto ret = myset.emplace("foo");
  if (!ret.second) std::cout << "foo already exists in myset\n";
  
  std::set<std::string> myset;
  auto it = myset.cbegin();
  myset.emplace_hint (it,"alpha");
  it = myset.emplace_hint (myset.cend(),"omega");
  it = myset.emplace_hint (it,"epsilon");
  it = myset.emplace_hint (it,"beta");
  std::cout << "myset contains:";
  for (const std::string& x: myset)
    std::cout << ' ' << x;
  std::cout << '\n';
  
  std::set<int> myset;
  std::set<int>::iterator it;
  // insert some values:
  for (int i=1; i<10; i++) myset.insert(i*10);  // 10 20 30 40 50 60 70 80 90
  it = myset.begin();
  ++it;                                         // "it" points now to 20
  myset.erase (it);
  myset.erase (40);
  it = myset.find (60);
  myset.erase (it, myset.end());
  std::cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';
  
  int myints[]={12,75,10,32,20,25};
  std::set<int> first (myints,myints+3);     // 10,12,75
  std::set<int> second (myints+3,myints+6);  // 20,25,32
  first.swap(second);
  
  std::set<int> myset;
  // set some initial values:
  for (int i=1; i<5; ++i) myset.insert(i*3);    // set: 3 6 9 12
  for (int i=0; i<10; ++i)
  {
    std::cout << i;
    if (myset.count(i)!=0)
      std::cout << " is an element of myset.\n";
    else
      std::cout << " is not an element of myset.\n";
  }
  
  std::set<int> myset;
  for (int i=1; i<=5; i++) myset.insert(i*10);   // myset: 10 20 30 40 50
  std::pair<std::set<int>::const_iterator,std::set<int>::const_iterator> ret;
  ret = myset.equal_range(30);
  std::cout << "the lower bound points to: " << *ret.first << '\n';
  std::cout << "the upper bound points to: " << *ret.second << '\n';

  std::set<int> myset;
  std::set<int>::iterator itlow,itup;
  for (int i=1; i<10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90
  itlow=myset.lower_bound (30);                //       
  itup=myset.upper_bound (60);                 //                   
  myset.erase(itlow,itup);                     // 10 20 70 80 90
  
  std::set<int> myset;
  int highest;
  std::set<int>::key_compare mycomp = myset.key_comp();
  for (int i=0; i<=5; i++) myset.insert(i);
  std::cout << "myset contains:";
  highest=*myset.rbegin();
  std::set<int>::iterator it=myset.begin();
  do {
    std::cout << ' ' << *it;
  } while ( mycomp(*(++it),highest) );
  std::cout << '\n';
  
  std::set<int> myset;
  std::set<int>::value_compare mycomp = myset.value_comp();
  for (int i=0; i<=5; i++) myset.insert(i);
  std::cout << "myset contains:";
  int highest=*myset.rbegin();
  std::set<int>::iterator it=myset.begin();
  do {
    std::cout << ' ' << *it;
  } while ( mycomp(*(++it),highest) );
  std::cout << '\n';
  
  set<[тип], greater[тип]> [name];
  set<long long> greater <long long> st;
  
  copy([начало],[конец], ostream_iterator<[тип]>(cout,[отступ]));
}
```

## 4. Класс std::unordered_map. Внутренняя реализация unordered_map, его основные методы. Сложность поиска, сортировки, удаления элемента, добавления элемента. Пример работы с std::unordered_map

## 5. Класс std::vector. Внутренняя реализация vector, его основные методы. Сложность поиска, сортировки, удаления элемента, добавления элемента. Пример работы с std::vector. Особенность std::vector<bool>.
    
### Класс std::vector. Внутренняя реализация vector.

std::vector - у каждого элемента есть свои индекс, индексы в векторе по умолчанию начинается с 0. В векторе может хранится только один тип данных int/char/string/bool и так далее. Классе вектор состоит из *data, size, max_size, где *data - это данные, size - размер количества элементов в векторе, max_size размер вектора на данный момент.
    ![image](https://user-images.githubusercontent.com/74144153/151165632-c48717f0-431e-40d6-b59f-b5e6256b3505.png)

### Основные методы
    
| Метод | Функция |
| --- | --- |
| at | Предоставляет доступ к указанному элементу с проверкой индекса |
| operator[] | Предоставляет доступ к указанному элементу |
| front | Предоставляет доступ к первому элементу |
| back | предоставляет доступ к последнему элементу |
| data  (C++11) | Предоставляет прямой доступ к внутреннему содержимому |
| empty | Проверяет отсутствие элементов в контейнере |
| size | Возвращает количество элементов в контейнере |
| max_size | Возвращает максимально допустимое количество элементов в контейнере |
| reserve | Зарезервировать память. |
| capacity | Возвращает количество элементов, которые могут одновременно храниться в выделенной области памяти |
| shrink_to_fit  (C++11) | Уменьшает использование памяти, высвобождая неиспользуемую |
| clear | Очищает контейнер |
| insert | Вставляет элементы |
| emplace  (C++11) | Конструирует элементы "на месте" и вставляет их начиная с заданной позиции pos |
| erase | Удаляет элементы |
| push_back | добавляет элемент в конец |
| emplace_back  (C++11) | Конструирует элементы "на месте" в конце контейнера |
| pop_back | Удаляет последний элемент |
| resize | Изменяет количество хранимых элементов |
| swap | Обменивает содержимое |    
    
### Сложности основных алгоритмов

1. Поиск: O(N)
2. Удаление: O(M+K), где M - количество удалённых, K - количество сдвинутых из-за удаления 
3. Добавление в середину: O(N), Добавление в конец: О(1) амортизированное 
4. Сортировка: O(N * log(N)) - если использовать std::sort
    
### Пример работы
```cpp
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector (10);   // 10 zero-initialized ints
  // assign some values:
  for (unsigned i=0; i<myvector.size(); i++)
    myvector.at(i)=i;
  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); i++)
    std::cout << ' ' << myvector.at(i);
  std::cout << '\n';

  std::vector<int> myvector (10);   // 10 zero-initialized elements
  std::vector<int>::size_type sz = myvector.size();
  // assign some values:
  for (unsigned i=0; i<sz; i++) myvector[i]=i;
   
  //front - доступ к ппервому элементу
  std::vector<int> myvector;
  myvector.push_back(78);
  myvector.push_back(16);
  // now front equals 78, and back 16
  myvector.front() -= myvector.back();
  std::cout << "myvector.front() is now " << myvector.front() << '\n';
  
  //back - доступ к послледнему элементу (всё аналогично)
  
  std::vector<int> myvector (5);
  int* p = myvector.data();
  *p = 10;
  ++p;
  *p = 20;
  p[2] = 100;
  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';
               
  std::vector<int> myvector;
  int sum (0);
  for (int i=1;i<=10;i++) myvector.push_back(i);
  while (!myvector.empty())
  {
     sum += myvector.back();
     myvector.pop_back();
  }
  std::cout << "total: " << sum << '\n';
  
  std::vector<int> myints;
  std::cout << "0. size: " << myints.size() << '\n';
  for (int i=0; i<10; i++) myints.push_back(i);
  std::cout << "1. size: " << myints.size() << '\n';
                                               
  std::vector<int> myvector;
  // set some content in the vector:
  for (int i=0; i<100; i++) myvector.push_back(i);
  std::cout << "size: " << myvector.size() << "\n";
                                              
  std::vector<int> myvector;
  // set some content in the vector:
  for (int i=0; i<100; i++) myvector.push_back(i);
  std::cout << "size: " << myvector.size() << "\n";
}
  
  std::vector<int>::size_type sz;
  std::vector<int> foo;
  sz = foo.capacity();
  std::cout << "making foo grow:\n";
  for (int i=0; i<100; ++i) {
    foo.push_back(i);
    if (sz!=foo.capacity()) {
      sz = foo.capacity();
      std::cout << "capacity changed: " << sz << '\n';
    }
  }
  std::vector<int> bar;
  sz = bar.capacity();
  bar.reserve(100);   // this is the only difference with foo above
  std::cout << "making bar grow:\n";
  for (int i=0; i<100; ++i) {
    bar.push_back(i);
    if (sz!=bar.capacity()) {
      sz = bar.capacity();
      std::cout << "capacity changed: " << sz << '\n';
    }
  } 
    
  std::vector<int> myvector;
  // set some content in the vector:
  for (int i=0; i<100; i++) myvector.push_back(i);
  std::cout << "size: " << (int) myvector.size() << '\n';
  std::cout << "capacity: " << (int) myvector.capacity() << '\n';
  std::cout << "max_size: " << (int) myvector.max_size() << '\n';                                               
                                                 
  std::vector<int> myvector (100);
  std::cout << "1. capacity of myvector: " << myvector.capacity() << '\n';
  myvector.resize(10);
  std::cout << "2. capacity of myvector: " << myvector.capacity() << '\n';
  myvector.shrink_to_fit();
  std::cout << "3. capacity of myvector: " << myvector.capacity() << '\n';
    
  myvector.clear();  
  
  std::vector<int> myvector (3,100);
  std::vector<int>::iterator it;
  it = myvector.begin();
  it = myvector.insert ( it , 200 );
  myvector.insert (it,2,300);
  // "it" no longer valid, get a new one:
  it = myvector.begin();
  std::vector<int> anothervector (2,400);
  myvector.insert (it+2,anothervector.begin(),anothervector.end());
  int myarray [] = { 501,502,503 };
  myvector.insert (myvector.begin(), myarray, myarray+3);
  std::cout << "myvector contains:";
  for (it=myvector.begin(); it<myvector.end(); it++)
    std::cout << ' ' << *it;
  std::cout << '\n'; 
               
  std::vector<int> myvector = {10,20,30};
  auto it = myvector.emplace ( myvector.begin()+1, 100 );
  myvector.emplace ( it, 200 );
  myvector.emplace ( myvector.end(), 300 );
  std::cout << "myvector contains:";
  for (auto& x: myvector)
    std::cout << ' ' << x;
  std::cout << '\n';  
    
  std::vector<int> myvector;
  // set some values (from 1 to 10)
  for (int i=1; i<=10; i++) myvector.push_back(i);
  // erase the 6th element
  myvector.erase (myvector.begin()+5);
  // erase the first 3 elements:
  myvector.erase (myvector.begin(),myvector.begin()+3);
  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';  
   
  //emplace_back  
  std::vector<int> myvector;
  int myint;
  std::cout << "Please enter some integers (enter 0 to end):\n";
  do {
    std::cin >> myint;
    myvector.push_back (myint);
  } while (myint);
  std::cout << "myvector stores " << int(myvector.size()) << " numbers.\n"; 
    
  std::vector<int> myvector;
  int sum (0);
  myvector.push_back (100);
  myvector.push_back (200);
  myvector.push_back (300);
  while (!myvector.empty())
  {
    sum+=myvector.back();
    myvector.pop_back();
  }
  std::cout << "The elements of myvector add up to " << sum << '\n';
    
  std::vector<int> myvector;
  // set some initial content:
  for (int i=1;i<10;i++) myvector.push_back(i);
  myvector.resize(5);
  myvector.resize(8,100);
  myvector.resize(12);
  std::cout << "myvector contains:";
  for (int i=0;i<myvector.size();i++)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  std::vector<int> foo (3,100);   // three ints with a value of 100
  std::vector<int> bar (5,200);   // five ints with a value of 200
  foo.swap(bar); 
```
    
    
### Особенность std::vector<bool>

Способ, которым std::vector< bool > сделан компактным, определяется реализацией. Одной из потенциальных оптимизаций является сливание векторных элементов таким образом, что каждый элемент занимает один бит, а не байт, как обычный элемент типа bool.

std::vector< bool > ведет себя аналогично std::vector, но для того, чтобы быть компактным, он:
- Не обязательно хранит свои данные в одном непрерывном куске памяти.
- Предоставляет std::vector<bool>::reference как метод доступа к отдельным битам.
- Не использует std::allocator_traits::construct чтобы построить битовые значения.

## 6. Парадигмы ООП. Полиморфизм (статический, динамический). Инкапсуляция. Наследование. Примеры.
    
## 7. Разработка обобщенных типов: шаблоны С++. Инстанцирование. Спецификация шаблонов. Примеры.
    
### Шаблоны - средство C++, предназначенное для разработки обобщённых алгоритмов, без привязки к некоторым параметрам (например, типам данных). 
    
Шаблон — это конструкция, которая создает обычный тип или функцию во время компиляции на основе аргументов, предоставленных пользователем для параметров шаблона. Например, можно определить шаблон функции следующим образом:
```cpp
template <typename T>
T minimum(const T& lhs, const T& rhs)
{
    return lhs < rhs ? lhs : rhs;
}
```
Приведенный выше код описывает шаблон для универсальной функции с одним параметром типа T, возвращаемое значение и параметры вызова (LHS и RHS) всех этих типов. Вы можете присвоить параметру типа любое имя, но, по правилам, наиболее часто используются буквы в одном верхнем регистре. T является параметром шаблона; ключевое слово говорит о том, что этот параметр является заполнителем для типа. При вызове функции компилятор заменит каждый экземпляр T с конкретным аргументом типа, который либо задается пользователем, либо выведенным компилятором. Процесс, в котором компилятор создает класс или функцию из шаблона, называется созданием экземпляра шаблона. — это экземпляр шаблона minimum<T> .
    
Ключевые слова typename и class взаимозаменяемы в шапке шаблона. Кроме класса в шаблон можно передавать вообще что угодно, даже числа (если они константы или могут быть расчитаны на этапе компиляции
    
### Инстанцирование шаблона:
    
Это генерация кода функции или класса по шаблону для конкретных параметров. Различают неявное инстанцирование, которое происходит при вызове функции или создании объекта класса, и явное инстанцирование с помощью резервированного слова template. Инстанцирование можно делать только в точке программы, где доступна реализация шаблона функции или методов шаблонного класса.
    
```cpp
#include <iostream>

template < typename T >
void my_swap(T& first, T& second)
{
    T temp(first);
    first = second;
    second = temp;
}
int main()
{
    int a = 5;
    int b = 10;
    std::cout << a << " " << b << std::endl;
    my_swap<int>(a, b);
    std::cout << a << " " << b << std::endl;
    double c = 77.89;
    double d = 54.22;
    std::cout << c << " " << d << std::endl;
    my_swap<double>(c, d);
    std::cout << c << " " << d << std::endl;
}    
```
Как видите, после имени функции в угловых скобках мы указываем тип, который нам необходим, он то и будет типом T. Шаблон — это лишь макет, по которому компилятор самостоятельно будет генерировать код. При виде такой конструкции: my_swap<тип> компилятор сам создаст функцию my_swap с необходимым типом. Это называется инстанцирование шаблона. То есть при виде my_swap<int> компилятор создаст функцию my_swap в которой T поменяет на int, а при виде my_swap<double> будет создана функция с типом double. Если где-то дальше компилятор опять встретит my_swap<int>, то он ничего генерировать не будет, т.к. код данной функции уже есть(шаблон с данным параметром уже инстанцирован).

Таким образом, если мы инстанцируем этот шаблон три раза с разными типами, то компилятор создаст три разные функции
    
### Спецификация шаблонов:
    
Спецификация - это задание более одного тела функции или класса для определенных параметров в шаблоне. То есть шаблон говорит: "вот толпа классов и все они одинаковые", а спецификация - это когда он добавляет: ", но вот ты будешь отличаться от них"

```cpp
template <typename T, typename D>
struct PointerWrapper<void>     // спецификация для шаблона из примера в 7.1: если у нас T=void, то деструктор обычный, без приколов
{
    T* pointer;
    
    virtual ~PointerWrapper() = default;   // подробнее про default - вопрос 10.3
};    
```
Нормальный пример: std::vector - это спецификация шаблона std::vector

## 8. Итераторы: определение, назначение, преимущества. Итераторы прямого доступа, итераторы ввода, итераторы вывода, двунаправленные итераторы, прямой итератор. Примеры.

**Итератор** - указатель на определённый элемент контейнерного класса с дополнительным набором перегруженных операторов для выполнения чётко определённых функций, итераторы обеспечивают доступ к элементам контейнера. С помощью итераторов очень удобно перебирать элементы. Итератор описывается типом iterator. Но для каждого контейнера конкретный тип итератора будет отличаться. Так, итератор для контейнера list представляет тип list::iterator, а итератор контейнера vector представляет тип vector::iterator и так далее.

-   Оператор * возвращает элемент, на который в данный момент указывает итератор.
-   Оператор ++ перемещает итератор к следующему элементу контейнера. Большинство итераторов также предоставляют оператор −− для перехода к предыдущему элементу.
-   Операторы == и != используются для определения того, указывают ли два итератора на один и тот же элемент или нет. Для сравнения значений, на которые указывают два итератора, нужно сначала разыменовать эти итераторы, а затем использовать оператор == или !=.
-   Оператор = присваивает итератору новую позицию (обычно начало или конец элементов контейнера). Чтобы присвоить значение элемента, на который указывает итератор, другому объекту, нужно сначала разыменовать итератор, а затем использовать оператор =.

Каждый контейнерный класс имеет 4 основных метода для работы с оператором =:
-   begin() возвращает итератор, представляющий начало элементов контейнера.
-   end() возвращает итератор, представляющий элемент, который находится после последнего элемента в контейнере.
-   cbegin() возвращает константный (только для чтения) итератор, представляющий начало элементов контейнера.
-   cend() возвращает константный (только для чтения) итератор, представляющий элемент, который находится после последнего элемента в контейнере.

### Преимущества итератора
    
По сравнению с индексацией итераторы имеют ряд преимуществ:
    
1. Индексация подходит не для всех типов данных (например std::list)
2. Итераторы предоставляют возможность последовательного перебора любых структур данных, поэтому делают код более читаемым, удобным для повторного использования и менее чувствительным к изменениям структур данных.
3. Итераторы могут предоставлять дополнительные возможности при навигации по элементам. Например, проверку отсутствия пропусков элементов или защиту от повторного перебора одного и того же элемента.
4. Некоторые контейнеры могут предоставлять возможность модифицировать свои объекты без влияния на сам итератор. Например, после того, как итератор уже «прошёл» первый элемент, можно вставить дополнительные элементы в начало контейнера без каких-либо нежелательных последствий. При использовании индексации это проблематично из-за смены номеров индексов.    

### Типы итераторов

##### Итератор ввода

***Input iterator*** предназначен только для однократного чтения (ввода) последовательности значений.
```cpp
Value value = *it++; // прочитать следующее значение, it - итератор
```
Итератор можно передвигать на одну позицию вперед (инкремент) и разыменовывать (операции * и ->), получая доступ к текущему значению. Итераторы можно сравнивать между собой на равенство и неравенство.

##### Итератор вывода

***Output iterator*** предназначен только для однократной записи (вывода) последовательности. В остальном аналогичен итератору ввода.
```cpp
*it++ = value;
```
##### Однонаправленный итератор

***Forward iterator*** является расширением концепции “итератор ввода”, т.е. предоставляет возможности итератора ввода (и, возможно, но не гарантированно, итератора вывода). Кроме того, однонаправленный итератор допускает многократное чтение и запись линейной последовательности, по которой можно двигаться только в одну сторону (как по односвязному списку — “вперёд” с помощью операции ++).

##### Двунаправленный итератор

***bidirectional iterator*** является расширением концепции “однонаправленный итератор”. Двунаправленный итератор допускает движение в двух направлениях: вперед (с помощью ++) и назад (с помощью операции --).

##### Итератор произвольного доступа

***random access iterator*** является расширением концепции “двунаправленный итератор” и наиболее похож по своему поведению на обычный указатель на элемент массива (который является частным случаем итератора произвольного доступа).

Итератор произвольного доступа допускает адресацию по индексу (оператор []), сдвиг в обе стороны на некоторое количество позиций (добавление и вычитание целого числа), вычисление расстояния с помощью вычитания и сравнение на “меньше” и “больше” (согласованное с расстоянием, которое имеет знак).
                    
## 9. Современный С++: auto, decltype, range base loop, nullptr, constexpr, enum class, if constexpr.

### auto

До С++11, ключевое слово auto использовалось как спецификатор хранения переменной (как, например, register, static, extern). В С++11 auto позволяет не указывать тип переменной явно, говоря компилятору, чтобы он сам определил фактический тип переменной, на основе типа инициализируемого значения.
    
```cpp
for (auto i : vec) {
    cout << i << endl;
}
```

### decltype

***Decltype*** позволяет статически определить тип по типу другой переменной.

```C++
int x = 5;
double y = 5.1;

decltype(x) foo;    // int
decltype(y) bar;    // double
decltype(x+y) baz;  // double
```

### Range based loop

***Range-Based for*** — это цикл по контейнеру.

```C++
for (int& x : foo)
    x *= 2;

for (const int& x : foo)
    std::cout << x << std::endl;
```

### nullptr

Раньше, для обнуления указателей использовался макрос NULL, являющийся нулем — целым типом, что, естественно, вызывало проблемы (например, при перегрузке функций). Ключевое слово nullptr имеет свой собственный тип std::nullptr_t, что избавляет нас от бывших проблем. Существуют неявные преобразования nullptr к нулевому указателю любого типа и к bool (как false), но преобразования к целочисленных типам нет.

```C++
void foo(int* p) {}

void bar(std::shared_ptr<int> p) {}

int* p1 = NULL;
int* p2 = nullptr;   

if(p1 == p2)
{}

foo(nullptr);
bar(nullptr);

bool f = nullptr;
int i = nullptr; // ошибка: для преобразования в int надо использовать reinterpret_cast
```

### Constexpr

С помощью него можно создавать переменные, функции и даже объекты, которые будут рассчитаны на этапе компиляции. Это удобно, ведь раньше для таких целей приходилось использовать шаблоны.

##### constexpr-функция

Ключевое слово constexpr, добавленное в C++11, перед функцией означает, что если значения параметров возможно посчитать на этапе компиляции, то возвращаемое значение также должно посчитаться на этапе компиляции. Если значение хотя бы одного параметра будет неизвестно на этапе компиляции, то функция будет запущена в runtime (а не будет выведена ошибка компиляции).

##### constexpr-переменная

Ключевое слово в данном случае означает создание константы. Причем expression должно быть известно на этапе компиляции.

```C++
int sum (int a, int b)
{
	return a + b;
}

constexpr int new_sum (int a, int b)
{
	return a + b;
}

void func()
{
	constexpr int a1 = new_sum (5, 12); // ОК: constexpr-переменная
	constexpr int a2 = sum (5, 12); // ошибка: функция sum не является constexp-выражением
	int a3 = new_sum (5, 12); // ОК: функция будет вызвана на этапе компиляции
	int a4 = sum (5, 12); // ОК
}
```

### enum-class

Перечисление представляет собой особый тип, значение которого ограничивается одной из нескольких явно именованных констант ("счетчики"). Значения констант - это значения целого типа, известного также как базовый тип перечисления.

```C++
enum color {
    red,            //присваевается 0
    yellow,         //присваевается 1
    green = 20,     //присваевается 20
    blue            //присваевается 21
};  
```
    
### if constexpr
    
if constexpr вычисляется во время компиляции, а if - нет. Это означает, что ветви могут быть отклонены во время компиляции и, следовательно, никогда не будут скомпилированы.
    
    ```cpp
    template <typename T>
void mixStaticWithDynamicIncorrect(T val)
{
    if constexpr(std::is_integral<T>::value)
        std::cout << "Integral passed.";
    else if(val == std::string{"clone"})
        std::cout << "Known string passed.";
    else if constexpr(std::is_same_v<T, std::string>)
        std::cout << "General string passed.";
    else
        std::cout << "Unknown type variable passed.";
    std::cout << "\n";
}
    ```






























































































































































































































































































































































































