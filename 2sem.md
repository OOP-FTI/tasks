# OOP_FTI_TASKS
Семестровые задачи для курса ООП кафедры ФТИ
## Блок первый: Шаблоны

### Сортировка (2 балла)
Реализовать сортировку, работающую за O(nlog(n)) с задаваемым компаратором. Примерный интерфейс:
```
template <class RandomAccessIterator, class Compare>
  void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

### Ultimate SQRT (2 балла)

Реализовать функцию ultimate_sqrt, которая позволяла бы вычислять квадратный корень из практически любого типа, который ей передан.
ultimate_sqrt будет представлять из шаблон функции с явной специализацией для таких типов как: ```vector<T>, list<T>, forward_list<T>, set<T>, unordered_set<T>, map<K,V>, unordered_map<K,V> ```. Для map и unordered_map вычисляем только по V.

Пример входных данных:

```
int i_value = 4;
double d_value = 9;
std::vector<double> vector_values= {16, 25};
```
Пример использования функции:
```
ultimate_sqrt(i_value);   // 2
ultimate_sqrt(d_value); // 3.0
ultimate_sqrt(vector_values); // {4.0, 5.0}
```
### Filter Iterator (2 балла)
boost::filter_iterator - интерфейс класса + примеры использования.

http://www.boost.org/doc/libs/1_55_0/libs/iterator/doc/filter_iterator.html

Задача:

Реализовать класс, аналогичный boost::filter_iterator - итерируется только по тем элементам, которые удовлетворяют заданным условию.

### Transform Iterator (2 балла)
boost::transform_iterator - интерфейс класса + примеры использования
https://www.boost.org/doc/libs/1_74_0/libs/iterator/doc/transform_iterator.html

Задача:

Реализовать класс, аналогичный boost::filter_iterator - адаптирует итератор, изменяя оператор*, чтобы применить объект функции к результату разыменования итератора и возврата результата.

### Integer Range (2 балла)
Реализовать класс Range который создает последовательность. С задаваемым началом, концом и шагом.
Пример использования:
```
for (int i : Range<int>(1, 10))
        std::cout << i << ' '; // 1 2 3 4 5 6 7 8 9
for (int i : Range<int>(10))
        std::cout << i << ' '; // 0 1 2 3 4 5 6 7 8 9
for (int i : Range<int>(1, 10, 3))
        std::cout << i << ' '; // 1 4 7
```

### Метапрограммирование ч.1 (2 балла)
Реализуйте метафункцию, которая бы позволила вызвать определенную функцию многократно. Пример:
```
deep<f, 3>(10);    // То же, что и f(f(f(10)))
deep<g, 1>(x);     // g(x)
```

### Shuffle Iterator (4 балла)
Реализовать класс shuffle_range и соответствующий ему iterator, который позволяет обойти диапазон (набор) значений в случайном (перемешанном) порядке.
```
int a[] = {0, 4, 5, 2, 8, 1};
std::forward_list < int > b = { 0, 4, 5, 2, 8, 1 };
std::unordered_map < int, int > c = {
    { 0, 0 }, { 4, 0 }, { 5, 0 }, { 2, 0 }, { 8, 0 }, { 1, 0 }
};

auto shuffle_a = make_shuffle(a, a+6);
auto shuffle_b = make_shuffle(b.begin(), b.end());
auto shuffle_c = make_shuffle(c.begin(), c.end());

for (auto i : shuffle_a)
    std::cout << (*i) << " ";

std::cout << std::endl;

for (auto i : shuffle_c)
 std::cout << (*i).first << " " << (*i).second << std::endl;

```
### Оптимизированный copy (3 балла) 
Реализовать собственную версию шаблонной функции copy с ускорением работы для частных случаев.

Мотивация:

Шаблонная функция std::copy реализует простой цикл. При копировании, например, массивов байтов (char или unsigned char) процесс будет происходить побайтово. Однако в силу архитектуры памяти и процессора копирование одного байта требует столько же времени, сколько и копирование машинного слова (4 байта для 32-разрядной архитектуры, 8 байт — для 64-разрядной). Функции побайтового копирования из стандартной библиотеки C (memcpy, memmove, strcpy и т.д.) учитывают этот нюанс, и мы могли бы воспользоваться ими.

#### Описание задачи

Возьмите два частных случая:

1. Итераторы являются указателями.

2. Итераторы относятся к std::vector.

И при условии, что элемент в принципе допускает побайтовое копирование (для проверки можно воспользоваться is_fundamental), реализуйте вызов memmove в соответствующих частных специализациях.

#### Тестирование

Проверьте, что копирование работает корректно, а также то, что частные специализации достаточно ограничены, чтобы не использовать memmove для более сложных типов данных. Например, для итераторов std::vector<std::string> мы не можем использовать memmove. Здесь должен сработать общий алгоритм копирования.


### Сериализация и дессериализация (4 балла)
Реализовать фреймворк для бинарной сериализации и десериализации произвольных структур данных.
Поддерживаемые контейеры: std::string;
vector<T> для любого сериализуемого типа T;
map<K, V> для любых сериализуемых типов K, V;

## Блок 2: Вывод типов и классы

### Многомерный вектор (1 балл)

Реализуйте класс многомерного вектора и всю необходимую математику для него (переход из одного базиса в другой, поворот и тд...).

### Unique pointer (2 балла)
Самостоятельно реализовать "умный указатель", осуществляющий стратегию единоличного владения ресурсом, аналогичный std::unique_ptr.

Интерфейс указателя должен:

•  Инкапсулировать переданный в него указатель.

•  При удалении "умного указателя", по умолчанию, он должен удалить переданный в него указатель.

•  Должны быть определены операторы разыменования (* и ->).

•  Должен быть следующие методы: очистка указателя, обмен с другим "умным указателем", инкапсуляция нового указателя.

•  Конструктор копирования и оператор присваивания должны быть запрещены.

Reference: https://en.cppreference.com/w/cpp/memory/unique_ptr

Примерный интерфейс:

```
template<class Type, class TDeleter>
class UniquePointer {
    using t_UniquePTR = UniquePTR<Type, TDeleter>;
public: // Constructors and destructor.
    UniquePointer();
    UniquePointer(Type *pObject);
    UniquePointer(t_UniquePTR &&uniquePTR); // Move constructor.
    ~UniquePointer();
public: // Assignment.
    UniquePointer& operator=(t_UniquePTR &&uniquePTR);
    UniquePointer& operator=(Type *pObject);
public: // Observers.
    Type& operator*() const; // Dereference the stored pointer.
    Type* operator->() const; // Return the stored pointer.
    Type* get() const; // Return the stored pointer.
    TDeleter& get_deleter(); // Return a reference to the stored deleter.
    operator bool() const; // Return false if the stored pointer is null.
public: // Modifiers.
    Type* release(); // Release ownership of any stored pointer.
    void reset(Type *pObject = nullptr); // Replace the stored pointer.
    void swap(t_UniquePTR &uniquePTR); // Exchange the pointer with another object.
private: // Disable copy from lvalue.
    UniquePointer(const t_UniquePTR&) = delete;
    t_UniquePTR& operator=(const t_UniquePTR&) = delete;
};

```
### Shared pointer (3 балла)

Самостоятельно реализовать "умный указатель", осуществляющий стратегию общего владения ресурсом (с подсчётом ссылок), аналогичный std::shared_ptr.

"Умный указатель" должен удовлетворять следующим требованиям:

• Инкапсулировать переданный в него указатель.

• Хранить внутри себя неотрицательное целое, счетчик внешних указателей, ссылающихся на объект.

• Когда "умный указатель" ссылается на объект, то он увеличивает счетчик объекта на единицу.

• Когда "умный указатель" перестает ссылаться на объект, то он уменьшает счетчик на единицу.

• Если счетчик становится равен нулю, то "умный указатель" удаляет объект.

• Должны быть определены операторы разыменования (* и ->).

• Должен быть следующие методы: очистка указателя, обмен с другим "умным указателем", инкапсуляция нового указателя.

Reference: https://en.cppreference.com/w/cpp/memory/shared_ptr

```
template<class Type, class TDeleter>
class SharedPTR {
    using t_SharedPTR = SharedPTR<Type, TDeleter>;
public: // Constructors and destructor.
    SharedPTR();
    SharedPTR(Type *pObj);
    SharedPTR(t_SharedPTR &&uniquePTR); // Move constructor.
    SharedPTR(const t_SharedPTR&);
    ~SharedPTR();
public: // Assignment.
    t_SharedPTR& operator=(t_SharedPTR &&sharedPTR);
    t_SharedPTR& operator=(Type *pObject);
    t_SharedPTR& operator=(const t_SharedPTR&);
public: // Observers.
    Type& operator*() const; // Dereference the stored pointer.
    Type* operator->() const; // Return the stored pointer.
    Type* get() const; // Return the stored pointer.
    TDeleter& get_deleter(); // Return a reference to the stored deleter.
    operator bool() const; // Return false if the stored pointer is null.
public: // Modifiers.
    void release(); // Release ownership of any stored pointer.
    void reset(Type *pObject = nullptr); // Replace the stored pointer.
    void swap(t_SharedPTR &sharedPTR); // Exchange the pointer with another object.
};


```

### Пул объектов (4 балла)

Реализовать контейнер, содержащий в себе фиксированный набор объектов, готовых к использованию.

Пул объектов используется:

Для повышения производительности, когда создание объекта в начале работы и уничтожение его в конце приводит к большим затратам времени / памяти;

В случае, когда объекты часто создаются и уничтожаются, но одновременно существует лишь небольшое их количество.

В пуле объекты создаются / уничтожаются в 2 этапа. Первичная инициализация / деинициализация — это дорогостоящая операция и производится только при создании уничтожении всего пула объектов. Финальная инициализация / деинициализация считается более дешёвой операцией и выполняется непосредственно перед запросом из пула / после возвращения в пул объекта.

В рамках задачи считаем выделение памяти дорогой, а вызов конструктора — дешёвой операцией.

Примерный интерфейс контейнера:

• Конструктор: указывается количество объектов в пуле (в дальнейшем их количество не изменяется). Производится частичная инициализация объектов (выделяется память).

• alloc(): производится окончательная инициализация объекта (вызывается конструктор объекта), объект помечается как занятый, пользователю возвращается ссылка на объект. Если свободных объектов в пуле нет — генерируется исключение.

• free(): производится частичная деинициализация (вызывается деструктор), объект помечается как свободный.

• Деструктор: все объекты в пуле деинициализируются (у занятых объектов вызывается деструктор). Производится полное удаление всех объектов (освобождается память).

По аналогии с методом std::vector.emplace, процедура запроса должна предусматривать передачу дополнительных параметров, необходимых для окончательной инициализации объекта.

### Хеш-таблица с управляемым удалением элементов (4 балла)

Реализовать надстройку над unordered_map, которая бы позволяла автоматически удалять элементы при обращении к ним.

Это может быть полезным для реализации кэша, у которого каждый элемент «жив» только в течение определённого времени.

• Класс стратегии управляемого удаления должен являться параметром шаблона для создаваемого класса, объект класса стратегии должен передаваться в конструкторе и храниться в классе (композиция). Класс стратегии должен инкапсулировать необходимые данные для принятия решения о том, удалять ключ или нет.

• Должны работать базовые операции: чтение и запись по ключу, удаление элементов. Весь интерфейс unordered_map поддерживать не нужно.

• Должны работать итераторы (при посещении элемента он также должен удаляться, а итератор — «перепрыгивать» к следующему).

• Создать несколько стратегий: не удалять никогда, удалять по прошествии заданного времени, удалять после заданного количества обращений.


### Skip List (5 баллов)

Реализовать структуру данных Skip list (список пропусков).

Подробное описание, как она работает, есть на Хабре: https://habr.com/ru/articles/230413/

Интерфейс класса подобен интерфейсу std::map, с теми же параметрами шаблона:

```
template <typename Key,
          typename Value,
          typename Compare = std::less<Key>,
          typename Alloc = std::allocator<std::pair<const Key,Value> > >
class skip_list {
    // ...
public:
    typedef ... iterator;
    typedef ... const_iterator;
    typedef std::pair<const Key, Value> value_type;

    skip_list();
    explicit skip_list(const Compare &comp, const Alloc &a = Alloc());
    skip_list(const skip_list &another);

    skip_list &operator=(const skip_list &another);

    iterator begin();
    const_iterator begin() const;
    iterator end();
    const_iterator end() const;

    bool empty() const;
    size_t size() const;

    Value &operator[](const Key &key);
    Value &at(const Key &key);
    const Value &at(const Key &key);

    std::pair<iterator, bool> insert(const value_type &);

    void erase(iterator position);
    size_type erase(const Key &key);
    void erase(iterator first, iterator last);

    void swap(skip_list &another);
    void clear();

    iterator find(const Key &key);
    const_iterator find(const Key &key) const;
};

template <typename K, typename V, typename C, typename A>
inline bool operator==(const skip_list<K,V,C,A> &x, const skip_list<K,V,C,A> &y) {
    // ....
}

template <typename K, typename V, typename C, typename A>
inline bool operator!=(const skip_list<K,V,C,A> &x, const skip_list<K,V,C,A> &y) {
    // ....
}
```

### Дерево (5 баллов)

Реализовать класс контейнер «Tree» (дерево) с поддержкой следующих операций:
1) итератор для перемещения по дереву;
2) добавить/удалить элемент;
3) получить/добавить поддерево;
4) реализовать итераторы совместимые со стандартными реализующие обход в
глубину и ширину

### Variant (9 баллов)

Как известно, Union в C++ не является полностью type-safe, поскольку он позволяет хранить данные разных типов в одной области памяти. Это может привести к ошибкам, если вы попытаетесь получить доступ к данным через переменную, которая не соответствует их типу.
В данной задаче предлагается реализовать класс наподобие std::variant, который является type-safe, поскольку он позволяет хранить данные разных типов в структуре данных, которая гарантирует безопасность типов. Это означает, что при доступе к данным в std::variant можно проверить, соответствует ли тип запрошенным данным. Если тип не соответствует, то будет сгенерировано исключение. Это позволяет избежать ошибок, связанных с неправильным использованием типов данных, и обеспечивает безопасность данных во время выполнения программы.

Reference: https://en.cppreference.com/w/cpp/utility/variant

Обязательно реализовать следующие методы: 

• Конструкторы, оператор присваивания и их move версии.

• holds_alternative - проверяет, содержит ли Variant данный тип.

• index - возвращает основанный на нуле индекс альтернативы, который в данный момент хранится в варианте.

• get - используется для получения значения, хранящегося Variant, с указанием типа.

## Блок 3: Многопоточность

### Простая thread-safety очередь (1 балл)

Как известно, std::deque является не потокобезопасной.

Сделайте надстройку над std::deque, чтобы очередь стала потокобезопасной.

Reference: https://en.cppreference.com/w/cpp/container/deque

### Простое число (3 балла)
У вас имеется массив целых чисел, необходимо определить есть ли в этом массиве
хотя бы одно не простое (делится без остатка только на себя и единицу). Необходимо
предоставить две реализации с последовательным и параллельным решением. Удалось ли
ускорить решение задачи за счет применения параллельных вычислений?

### Сортировка (4 балла)

Реализовать параллельную версию алгоритма быстрая сортировка. Cхема разбиения должна определяться с помощью SchemePolicy классов. Нижеприведенные сигнатуры функций pquicksort фиксированы и не подлежат изменению.

```
template <class RandomIt, class SchemePolicy>
void pquicksort(RandomIt first, RandomIt last, SchemePolicy&& partition);

template <class RandomIt, class Compare, class SchemePolicy>
void pquicksort(RandomIt first, RandomIt last, Compare comp, SchemePolicy&& partition);
```

Параллельная версия алгоритма работает лучше, чем последовательная реализация, только если размер диапазона превышает определенный порог, который может варьироваться в зависимости от параметров компиляции, платформы или оборудования. Поэкспериментируйте с различными порогами и размерами диапазонов, чтобы увидеть, как изменяется время выполнения. И установите порог на минимальное количество элементов для выполнения многопоточной реализации.

### Многопоточные вычисления (5 баллов)
В директории лежат входные текстовые файлы, проименованные следующим образом: in_\<N>.dat, где N - натуральное число. Каждый файл состоит из одной строки. Формат строки следующий: Cmd value1 value2. В начале идет команда, обозначающая действие. Далее указываются два числа с плавающей точкой. В качестве разделителя используется пробел.

Команды могут быть следующими:

• add: сложение.

• mult: умножение.

• add_sq: сумма квадратов.

• sq_add: квадрат суммы.

• sub: вычитание.

• div: деление.
Необходимо написать многопоточное приложение, которое выполнит требуемые действия над числами и сумму результатов запишет в файл out.dat. Названия рабочей директории и выходного файла указываются в конфигурационном файле.

Кроме того, необходимо реализовать потокобезопасный (thread-safety) класс logger для логирования сообщений в поток (std::ostream). Вся информация должна логироваться. Необходимость логирования также указывается в конфигурационном файле.

### Многопоточные матрицы (5 баллов)
Реализовать шаблонный класс Matrix, способный выполнять многопоточные вычисления (сложение, вычитание, умножение, детерминант). Определение количества потоков выполнения является деталью реализации.

Параллельная версия работает лучше, чем последовательная реализация, только если размер матрицы превышает определенный порог, который может варьироваться в зависимости от параметров компиляции, платформы или оборудования. Поэкспериментируйте с различными порогами и размерами матрицы, чтобы увидеть, как изменяется время выполнения. И установите порог на минимальное количество элементов для выполнения многопоточной реализации.

Все операции (сложение, вычитание, умножение, детерминант) класса Matrix должны быть 2-х видов: синхронные и асинхронные.

### FIFO (6 баллов)

Разработать класс, позволяющий производить многопоточное буферизированное чтение данных. Данные читаются блоками произвольного размера, произвольное количество блоков за раз. Типичный пример подобных данных - последовательность сжатых изображений разного размера (например, в формате jpeg).

Задание предполагает одновременную работу двух потоков выполнения: один поток (поток-читатель) считывает данные в очередь, второй (поток-писатель) - с некоторой периодичностью проверяет очередь, выбирает из неё очередные данные, и записывает их в файлы.

Очередь выполняет здесь роль регулятора, который:

• позволяет согласовать скорости чтения и записи данных между потоками,
• гарантирует порядок и целостность передаваемых данных.
• синхронизирует доступ к данным из разных потоков выполнения.
• Класс очереди должен быть спроектирован в рамках концепции RAII, и являться владельцем буфера данных.


### Биржа (6 баллов)
Реализовать модель биржи. Биржа — место где «Брокеры» могут совершать операции
по продаже и покупке «Товара». Цель брокера за счет проведения операций увеличить свое
состояние. Каждый брокер в своем распоряжении имеет определенное количество денег и
товара. Операция выполняется за счет того, что вы размещаете свои предложения по покупке
и продаже товара на бирже и анализируя текущие предложения можете совершить сделку. В
качестве брокера может выступать как человек управляющий командами с консоли, так и
несколько типов искусственных интеллектов, реализующих свое поведение согласно
стратегии: «Большой куш» — покупают и продают только если есть предложение
превышающее стоимость затраченных денег на покупку заданный порог или выполняют
минимизацию убытков если в течении длительного времени заработать не получилось.
«Игрок» — покупает и продает как можно чаще. «Аналитик» — анализирует историю
предложений на бирже и исходя из этой информации совершает сделки. За каждую минуту
размещенного на бирже заказа, биржа забирает с брокера фиксированную сумму денег за
свои услуги. В системе каждый брокер должен быть представлен независимой нитью
исполнения в которой могут асинхронно приниматься решения.

### Reader-writer lock (6 баллов)

Разработать синхронизационный объект (rwlock), позволяющий одновременное чтение данных несколькими потоками, но разрешающий запись данных только одному потоку.
Разработать набор классов:

• class rwlock; - основной класс, реализующий всю логику работы.

• class reader_lock - вспомогательный класс захватывающий объект на чтение. Должен использовать концепцию RAII для захвата и освобождения ресурса.

• class writer_lock - вспомогательный класс захватывающий объект на запись. Должен использовать концепцию RAII для захвата и освобождения ресурса.

Примерные интерфейсы классов (публичные методы).
```
class rwlock {
public:
    rwlock();
    rwlock(const rwlock&) = delete;
    rwlock(rwlock&&);
    rwlock& operator=(const rwlock&) = delete;
    rwlock& operator=(rwlock&&);
    ~rwlock();

    bool read_lock(int64_t timeOut = -1); // попытка захвата объекта на чтение
    void read_unlock(); // освобождение объекта

    bool write_lock(int64_t timeOut = -1); // попытка захвата объекта на запись
    void write_unlock(); // освобождение объекта
}

```
• rwlock() конструктор

• rwlock(const rwlock&) = delete; - удалённый конструктор копирования

• rwlock(rwlock&&); - move конструктор

• rwlock& operator=(const rwlock&) = delete; - удалённый оператор =

• rwlock& operator=(rwlock&&); - оператор =

• ~rwlock();

• bool read_lock(int64_t timeOut = -1); - попытка захвата объекта на чтение. Если объект не захвачен на запись, осуществляет захват на чтение. На чтение разрешается захватывать 'много' раз. Если объект захвачен на запись, осуществляет ожидание в течении timeOut (мс) и пытается захватить объект.

• void read_unlock(); - освобождение ридера

• bool write_lock(int64_t timeOut = -1); - попытка захвата объекта на запись. Если объект не захвачен на запись и не захвачен на чтение, осуществляет захват на запись. На запись разрешается захватывать только один раз. Если объект захвачен на запись или чтение, осуществляет ожидание в течении timeOut (мс) и пытается захватить объект.

• void write_unlock(); // освобождение объекта

```
class reader_lock {
public:
    reader_lock() = delete;
    reader_lock(const rwlock&) = delete;
    reader_lock(rwlock&&) = delete;
    reader_lock& operator=(const rwlock&) = delete;
    reader_lock& operator=(rwlock&&) = delete;

    reader_lock(rwlock& parent, bool auto = true, int64_t timeout = -1);
    ~reader_lock();
    bool is_locked();
    bool lock(int64_t timeout = -1);
    void unlock();
}
```
• reader_lock() = delete;

• reader_lock(const rwlock&) = delete;

• reader_lock(rwlock&&) = delete;

• reader_lock& operator=(const rwlock&) = delete;

• reader_lock& operator=(rwlock&&) = delete;

• reader_lock(rwlock& parent, bool auto = true, int64_t timeout = -1); - конструктор. Параметры: rwlock& parent - ссылка на основной объект, bool auto = true - захватывать автоматически, в конструкторе, int64_t timeout = -1 - время ожитания

• ~reader_lock(); - деструктор. Если захвачен, освобождает основной объект (чтение)

• bool is_locked(); - возвращает флаг - захвачен, не захвачен.

• bool lock(int64_t timeout = -1); - попытка захвата объекта на чтение. Возврашает флаг успешности.

• void unlock(); - освобождение чтения.

```
class writer_lock {
public:
    writer_lock() = delete;
    writer_lock(const rwlock&) = delete;
    writer_lock(rwlock&&) = delete;
    writer_lock& operator=(const rwlock&) = delete;
    writer_lock& operator=(rwlock&&) = delete;

    writer_lock(rwlock& parent, bool auto = true, int64_t timeout = -1);
    ~writer_lock();
    bool is_locked();
    bool lock(int64_t timeout = -1);
    void unlock();
}
```

• writer_lock() = delete;

• writer_lock(const rwlock&) = delete;

• writer_lock(rwlock&&) = delete;

• writer_lock& operator=(const rwlock&) = delete;

• writer_lock& operator=(rwlock&&) = delete;

• writer_lock(rwlock& parent, bool auto = true, int64_t timeout = -1); - конструктор. Параметры: rwlock& parent - ссылка на основной объект, bool auto = true - захватывать автоматически, в конструкторе, int64_t timeout = -1 - время ожитания

• ~writer_lock(); - деструктор. Если захвачен, освобождает основной объект (запись)

## Блок 4: Паттерны проектирования

### Логгер (2 балла)

Реализовать логгер, используя паттерн Singleton. Примерный интерфейс:
```
class logstream {
    // реализация ...
public:
    logstream(const std::string & prefix = std::string());

    static void set_log_file(const std::string & name);

    logstream();
    ~logstream();

    template < typename T >
    inline logstream & operator << (const T & rhs) {
        // ...
        return *this;
    }
    // ...
};
```
• конструктор – конструируется "легковесный" объект, предоставляющий доступ к одному единственному файловому потоку, ведущему запись в журнал.

• деструктор – "легковесный" объект уничтожается, при этом файловый поток, ведущий запись в журнал сбрасывает все буферизованные данные в файл, но сам файл не закрывает и продолжает вести запись.

• set_log_file – статический метод класса, позволяющий назначить файл журнала. Если в момент вызова журнал уже был открыт, то старый файл сохраняется и закрывается, отрывается новый файл, и в дальнейшем, журнал ведётся уже в него.

• operator << – перегруженный оператор вывода в поток. При передаче в поток манипулятора endl, происходит принудительный сброс буферизованных данных в файл журнала.

• logstream(const std::string & prefix = std::string()) - назначение префикса при конструировании logstream. Добавляется в поток в начале каждой строки.

### Логгер с различным поведением (3 балла)
Паттерн стратегия

**1. Реализовать класс логгер. Примерный интерфейс:**
```
class LoggerStrategy {
public:
    virtual void write(const std::string &message) = 0;
};
```
**2. Реализовать 3 типа поведения (стратегий)**

ConsoleLogger - выводит сообщение в консоль

SimpleFileLogger - выводит сообщение в файл

TimedFileLogger - выводит текущее время + сообщение в файл

3. Реализовать класс, который будет использовать одну из стратегий

```
class Logger {
public:
    void set_strategy(LoggerStrategy& strategy);

    void log_message(const std::string &message);
};
```

### Численные методы (3 балла)

Хорошо известны методы численного решения одномерного уравнения $f(x)=0$ на интервале $[a, b]$ (в предположении, что на заданном интервале есть только один корень). Это:

[Метод бисекции (деление отрезка пополам)](https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4_%D0%B1%D0%B8%D1%81%D0%B5%D0%BA%D1%86%D0%B8%D0%B8).

[Метод хорд](https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4_%D1%85%D0%BE%D1%80%D0%B4).

[Метод Ньютона](https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4_%D0%9D%D1%8C%D1%8E%D1%82%D0%BE%D0%BD%D0%B0).

Необходимо реализовать эти три алгоритма, используя паттерн проектирования «Стратегия», чтобы они были взаимозаменяемы.

Обратите внимание, что метод Ньютона кроме основной функции $f$ требует задания её производной $f'$. Учтите этот факт при разработке интерфейса классов.

### Быстрый доступ к разрешению видео (4 балла)

Реализовать класс, позволяющий получать информацию о разрешении видео-файла со следующим предположением. Если имя файла заканчивается на _WWWxHHH, где WWW и HHH – десятичные числа (необязательно состоящие из трёх цифр), то WWW – ширина, а HHH – высота. В этом случае для получения размера необязательно открывать файл.

vasya_1024x768.avi – размер 1024x768.
kolya_300x30 – размер 30x30.
petya_200xmax.mp4 – неверный формат.
В последнем случае, поскольку формат не соответствует, модуль должен все же открыть файл и считать разрешение явным образом.

Работу с самим видео-файлом вынести в отдельный класс (можно воспользоваться библиотекой ffmpeg или opencv)

### Обход графа (5 баллов)

Реализовать два алгоритма обхода графа (DFS — поиск в глубину и BFS — поиск в ширину), используя паттерн проектирования «Шаблонный метод».

Для переопределения должны быть доступны следующие методы:

•Начало обхода.

•Конец обхода.

•Посещение узла V.

•Посещение ребра E.


•Оба алгоритма должны иметь унифицированный интерфейс (реализуя, таким образом, паттерн «Стратегия»).

Абстракцию графа желательно вынести в отдельный класс, сделав его либо параметром шаблона, либо передавая в алгоритм указатель/ссылку на объект базового класса представления графа.


### Данные о футбольном чемпионате (5 баллов)

Реализовать систему хранения информации о футбольном чемпионате. Информация опирается на следующие основные классы: Team (команда), Player (игрок), Match (матч). Эти классы связаны друг с другом посредством агрегации, ассоциации и проч.

Атрибуты классов

**Team**

• id — уникальный численный идентификатор.

• name — имя.

• players — игроки, играющие за данную команду в рамках чемпионата.

**Player**

• id — уникальный численный идентификатор.

• name — имя

• team – команда.

**Match**

• id — уникальный численный идентификатор.

• date — дата.

• location — место.

• result — счёт.

• team1 — первая команда.

• team2 — вторая команда.

• players – игроки, у частвовавшие в матче.

Обеспечить загрузку и сохранение этой базы данных в текстовый файл формата TSV (Tab-Separated Values). Каждая строка в этом файле должна начинаться со слов team, player, match. Далее будут идти необходимые атрибуты.

