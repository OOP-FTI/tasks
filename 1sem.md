## Блок 1. Классы.

## Во всех заданиях намерено не поставлены спецификаторы static и const. Их необходимо добавить в нужных местах.
## Нельзя менять названия public методов класса, а также типы и количество аргументов в них.

### Задача 1. Битовый массив.

Общие сведения

Битовый массив (битовое множество) - это массив, компактно хранящий биты и позволяющий удобно ими оперировать. В этой задаче требуется реализовать битовый массив переменного размера (в отличие от std::bitset).

Более подробно о битовых массивах: http://en.wikipedia.org/wiki/Bit_array.

Пример промышленной реализации битового массива: http://www.boost.org/doc/libs/1_51_0/libs/dynamic_bitset/dynamic_bitset.html  
Задача
Реализовать битовый массив с заданным интерфейсом (см. раздел “Реализация”).

Написать юнит-тесты на все публичные методы класса с помощью любой специализированной библиотеки (рекомендуется Google Test Framework http://code.google.com/p/googletest/), либо без оной (на усмотрение преподавателя). Убедиться в полноте покрытия кода тестами (каждая строчка кода должна исполняться хотя бы одним тестом).

Методические указания

При написании кода особое внимание обращайте на обработку исключительных ситуаций и граничных случаев, в частности, на корректность аргументов методов. Продумывайте и документируйте обработку ошибок в ваших методах.

Дополнительно: попробуйте часть методов протестировать до их реализации.

*В этой задаче для простоты не требуется делать контейнер шаблонным, но это вполне допускается по желанию студента.*

```c++
class BitArray
{
public:
  BitArray();
  ~BitArray();
  
  //Конструирует массив, хранящий заданное количество бит.
  //Первые sizeof(long) бит можно инициализровать с помощью параметра value.
  explicit BitArray(int num_bits, unsigned long value = 0);
  
  BitArray(const BitArray& b);


  //Обменивает значения двух битовых массивов.
  void swap(BitArray& b);

  BitArray& operator=(const BitArray& b);


  //Изменяет размер массива. В случае расширения, новые элементы 
  //инициализируются значением value.
  void resize(int num_bits, bool value = false);
  //Очищает массив.
  void clear();
  //Добавляет новый бит в конец массива. В случае необходимости 
  //происходит перераспределение памяти.
  void push_back(bool bit);


  //Битовые операции над массивами.
  //Работают только на массивах одинакового размера.
  //Обоснование реакции на параметр неверного размера входит в задачу.
  BitArray& operator&=(const BitArray& b);
  BitArray& operator|=(const BitArray& b);
  BitArray& operator^=(const BitArray& b);
 
  //Битовый сдвиг с заполнением нулями.
  BitArray& operator<<=(int n);
  BitArray& operator>>=(int n);
  BitArray operator<<(int n) const;
  BitArray operator>>(int n) const;


  //Устанавливает бит с индексом n в значение val.
  BitArray& set(int n, bool val = true);
  //Заполняет массив истиной.
  BitArray& set();

  //Устанавливает бит с индексом n в значение false.
  BitArray& reset(int n);
  //Заполняет массив ложью.
  BitArray& reset();

  //true, если массив содержит истинный бит.
  bool any();
  //true, если все биты массива ложны.
  bool none();
  //Битовая инверсия
  BitArray operator~();
  //Подсчитывает количество единичных бит.
  int count() const;


  //Возвращает значение бита по индексу i.
  bool operator[](int i);

  int size();
  bool empty();
  
  //Возвращает строковое представление массива.
  std::string to_string();
};

bool operator==(const BitArray & a, const BitArray & b);
bool operator!=(const BitArray & a, const BitArray & b);

BitArray operator&(const BitArray& b1, const BitArray& b2);
BitArray operator|(const BitArray& b1, const BitArray& b2);
BitArray operator^(const BitArray& b1, const BitArray& b2);
```

### Задача 2. Кольцевой буффер. 

Общие сведения

Кольцевой буфер - это массив, начало и конец которого замкнуты. В частности, это означает, что при добавлении элементов в конец буфера, при исчерпании его ёмкости, начнут переписываться элементы из начала буфера, а “начало”, соответственно, сдвигаться.

Детальное описание этой структуры данных содержится в английской википедии: http://en.wikipedia.org/wiki/Circular_buffer

Пример промышленной реализации кольцевого буфера содержится в библиотеке Boost:
http://www.boost.org/doc/libs/1_51_0/libs/circular_buffer/doc/circular_buffer.html

Задача

Реализовать кольцевой буфер с заданным интерфейсом (см. раздел “Реализация”).

Тщательно задокументировать публичные члены класса на языке, приближенном к техническому английскому.

Написать юнит-тесты на все публичные методы класса с помощью любой специализированной библиотеки (рекомендуется Google Test Framework http://code.google.com/p/googletest/), либо без оной (на усмотрение преподавателя). Убедиться в полноте покрытия кода тестами (каждая строчка кода должна исполняться хотя бы одним тестом).

Методические указания: При написании кода особое внимание обращайте на обработку исключительных ситуаций и граничных случаев, в частности, на корректность аргументов методов. Продумывайте и документируйте обработку ошибок в ваших методах.

Дополнительно: попробуйте часть методов протестировать до их реализации.

*В этой задаче для простоты не требуется делать контейнер шаблонным, но это вполне допускается по желанию студента.*

```c++
typedef char value_type;

class CircularBuffer {
  value_type * buffer;
  /*... реализация ... */
public:
  CircularBuffer();
  ~CircularBuffer();
  CircularBuffer(const CircularBuffer & cb);

  //Конструирует буфер заданной ёмкости.
  explicit CircularBuffer(int capacity);
  //Конструирует буфер заданной ёмкости, целиком заполняет его элементом elem.
  CircularBuffer(int capacity, const value_type & elem);

  //Доступ по индексу. Не проверяют правильность индекса.
  value_type & operator[](int i);
  const value_type & operator[](int i);
  
  //Доступ по индексу. Методы бросают исключение в случае неверного индекса.
  value_type & at(int i);
  const value_type & at(int i) const;

  value_type & front(); //Ссылка на первый элемент.
  value_type & back();  //Ссылка на последний элемент.
  const value_type & front();
  const value_type & back();

  //Линеаризация - сдвинуть кольцевой буфер так, что его первый элемент
  //переместится в начало аллоцированной памяти. Возвращает указатель 
  //на первый элемент.
  value_type * linearize();
  //Проверяет, является ли буфер линеаризованным.
  bool is_linearized();
  //Сдвигает буфер так, что по нулевому индексу окажется элемент 
  //с индексом new_begin.
  void rotate(int new_begin);
  //Количество элементов, хранящихся в буфере.
  int size();
  bool empty();
  //true, если size() == capacity().
  bool full();
  //Количество свободных ячеек в буфере.
  int reserve();
  //ёмкость буфера
  int capacity();

  //Устанавливает новую емкость. Если емкость больше текущей, просто расширяем память. Если меньше, очищаем массив.	
  void set_capacity(int new_capacity);
  //Изменяет размер буфера.
  //В случае расширения, новые элементы заполняются элементом item.
  void resize(int new_size, const value_type & item = value_type());
  //Оператор присваивания.
  CircularBuffer & operator=(const CircularBuffer & cb);
  //Обменивает содержимое буфера с буфером cb.
  void swap(CircularBuffer & cb);

  //Добавляет элемент в конец буфера. 
  //Если текущий размер буфера равен его ёмкости, то переписывается
  //первый элемент буфера (т.е., буфер закольцован). 
  void push_back(const value_type & item = value_type());
  //Добавляет новый элемент перед первым элементом буфера. 
  //Аналогично push_back, может переписать последний элемент буфера.
  void push_front(const value_type & item = value_type());
  //удаляет последний элемент буфера.
  void pop_back();
  //удаляет первый элемент буфера.
  void pop_front();

  //Вставляет элемент item по индексу pos. Ёмкость буфера остается неизменной.
  void insert(int pos, const value_type & item = value_type());
  //Удаляет элементы из буфера в интервале [first, last).
  void erase(int first, int last);
  //Очищает буфер.
  void clear();
};

bool operator==(const CircularBuffer & a, const CircularBuffer & b);
bool operator!=(const CircularBuffer & a, const CircularBuffer & b);
```
### Задача 3. Строки.
Реализовать класс String для работы со строками c применением техники reference counting и copy-on-write. Класс представляет собой динамический массив char'ов с NULL-терминатором на конце.

Основная идея copy-on-write & reference counting следующая: при копировании и присваивании объектов не происходит полного копирования строк. Копируются только указатели на буфер и увеличивается счетчик ссылок. Но при модификации объекта мы уменьшаем счетчик ссылок и выполняем полное копирование. Важно отметить, что при вызове деструктора, мы уменьшаем счетчик ссылок и если он становится равен нулю, тогда освобождаем динамически выделенную память. В противном случае, ничего не делаем.

Методические указания: При написании кода особое внимание обращайте на обработку исключительных ситуаций и граничных случаев, в частности, на корректность аргументов методов. Продумывайте и документируйте обработку ошибок в ваших методах.

Дополнительно: попробуйте часть методов протестировать до их реализации.

```c++
//String.hpp
class String {
public:
    String();
    String(const char* str);
    String(const char* str, size_t n);
    String(size_t n, char c);
    String(const String& str);
    String(const String& str, size_t pos, size_t len = npos);
    virtual ~String();

    size_t size();
    size_t capacity();
    void reserve(size_t n = 0);
    void clear();
    bool empty();

    char& at(size_t pos);
    const char& at(size_t pos) const;

    char& operator[](size_t pos);
    const char& operator[](size_t pos) const;

    char& back();
    const char& back() const;

    char& front();
    const char& front() const;

    String& operator+=(const String& str);
    String& operator+=(const char* str);
    String& operator+=(char c);

    String& operator=(const String& str);
    String& operator=(const char* str);

    //выполняет вставку строки str в позицию pos.
    //Если pos больше размера строки, то выбрасывается исключение throw std::out_of_range("message");.
    String& insert(size_t pos, const String&  str);
    String& insert(size_t pos, const char* str);

    //выполняет удаление подстроки в строке начиная с позиции pos и охватывая len символов.
    //Если len равняется npos, то удаление происходит с позиции pos до конца строки. Если pos больше размера строки, то выбрасывается исключение throw    . 
    //std::out_of_range("message");.
    String& erase(size_t pos = 0, size_t len = npos);

    //выполняет замену подстроки в строке начиная с позиции pos и охватывая len символов.
    //Если len равняется npos, то удаление происходит с позиции pos до конца строки.
    //Если pos больше размера строки, то выбрасывается исключение throw std::out_of_range("message");.
    String& replace(size_t pos, size_t len, const String& str);
    String& replace(size_t pos, size_t len, const char* str);
    String& replace(size_t pos, size_t len, size_t n, char c);

    void swap(String& str);
    const char* data();

    //возвращает индекс подстроки str (или символа c) в строке. pos - задает позицию с которой начинать поиск.
    //Если подстроку не нашли или pos больше размера строки, метод возращает npos.
    size_t find(const String& str, size_t pos = 0);
    size_t find(const char* str, size_t pos = 0);
    size_t find(char c, size_t pos = 0);

    String substr(size_t pos = 0, size_t len = npos);

    int compare(const String& str);
  //статическая константа с максимальным значением size_t.
  // Это значением может быть использовано в методах для параметра len, что означает "до конца строки".
  //Также эта константа может быть использована в качестве возвращаемого значения для указания отсутствия совпадений.
    static const size_t npos = -1;

    size_t countRef();
};
```
### Задача 4. Изображение.
Реализовать класс Image для работы с изображениями c применением техники reference counting. Класс представляет собой многоканальный двумерный массив из unsigned char для хранения пикселей изображения.

Справочная информация

Цифровые изображения состоят из пикселей, а пиксели состоят из комбинаций основных цветов. Набор основных цветов называются каналами. Например, RGB изображения состоят из 3-х каналов: R (red), G (green), B (blue), а изображения в градациях серого являются одноканальными.

Методические указания: При написании кода особое внимание обращайте на обработку исключительных ситуаций и граничных случаев, в частности, на корректность аргументов методов. Продумывайте и документируйте обработку ошибок в ваших методах.

Основная идея copy-on-write & reference counting следующая: при копировании и присваивании объектов не происходит полного копирования строк. Копируются только указатели на буфер и увеличивается счетчик ссылок. Но при модификации объекта мы уменьшаем счетчик ссылок и выполняем полное копирование. Важно отметить, что при вызове деструктора, мы уменьшаем счетчик ссылок и если он становится равен нулю, тогда освобождаем динамически выделенную память. В противном случае, ничего не делаем.

Дополнительно: попробуйте часть методов протестировать до их реализации.

```c++
//Image.hpp

enum class MirrorType {
  Vertical,
  Horizontal
}

class Image {

public:
    Image();
    Image(int rows, int cols, int channels);
    Image(int rows, int cols, int channels, unsigned char* data);

    //конструктор копий. Данный конструктор не выделяет новую память, а применяет технику reference counting. Сложность создания копии объекта O(1).
    Image(const Image& image);
    virtual ~Image();

    //оператор присваивания. В некотором роде похож на конструктор. Т.е. не выделяет новую память, а применяет технику reference counting. Сложность данной операции O(1).
    Image& operator=(const Image& image);

    // Вернуть клон изборажения, создает полную копию изображения. Выделяет новую память и производит копирование пикселей. Сложность операции O(n), где n - количество пикселей.
    Image clone();
    //Скопировать изображение.
    void copyTo(Image& image);
    void create(int rows, int cols, int channels);
    bool empty();

    //декрементирует счетчик ссылок и в случае необходимости освобождает ресурсы (память).
    void release();

//возвращает новое изображение, которое содержит один столбец по индексу x.
    Image col(int x);

//аналог метода col(int x) для строк.
    Image row(int y);

    const unsigned char* data() const;
    unsigned char* data();

    int rows();
    int cols();
    int total();
    int channels();

    //Вернуть ЧАСТЬ пикселя
    unsigned char& at(int index);
    const unsigned char& at(int index) const;

//создает новое изображение, которое инициализируется нулями.
    Image zeros(int rows, int cols, int channels);

//создает новое изображение, которое инициализируется значением value.
    Image values(int rows, int cols, int channels, unsigned char value);

    //Отразить изображение по вертикали или по горизонтали
    void Mirror(MirrorType type);

    //Повернуть на угол кратный 90
    void Rotate(double angle);

    //Возвращает текущее количество ссылок на изображение.
    //Т.е. количество объектов, которые ссылаются на данное изображение. Этот метод нужен для unit test'ов.
    size_t countRef();
};
```
## Блок 2. STL.
### Задача 1. Поиск пути в лабиринте.
Найти наикратчайший путь для выхода из лабиринта, либо указать, что пути нет.

Входные данные
Текстовый файл с закодированным лабиринтом. Обозначения: 0 - пусто, 1 - стена, 2 - герой, 3 - выход.

Пример:

```
1 1 1 1 1 1 1 1 1 1
1 2 0 1 0 0 0 0 0 1
1 0 0 1 0 0 1 0 0 1
1 0 0 1 0 0 1 0 0 3
1 0 0 0 0 0 1 0 0 1
1 0 0 0 0 0 0 0 0 1
1 1 1 1 1 1 1 1 1 1
```

Задача
Найти наикратчайший путь от позиции героя до точки выхода из лабиринта используя волновой алгоритм.

Передачу имён файлов реализовать через аргументы командной строки.

Выходные данные

Файл аналогичный исходному, в котором символами * обозначен путь от позиции героя до точки выхода.

Например:

1 1 1 1 1 1 1 1 1 1
1 2 0 1 0 0 0 0 0 1
1 * * 1 0 0 1 0 0 1
1 0 * 1 0 0 1 * * 3
1 0 * * 0 0 1 * 0 1
1 0 0 * * * * * 0 1
1 1 1 1 1 1 1 1 1 1

### Задача 2. Сложение чисел в разных системах счисления.

Разработать программу для сложения чисел в разных системах счисления (от 2 до 36).

Входные данные
Текстовый файл:

<система_A>: <строка_с_числом_в_системе_А>

<система_B>: <строка_с_числом_в_системе_B>

<система_C>

система A, B — системы счисления входных чисел от 2 до 36

<строкасчисломвсистеме_А> - строка с числом в указанной системе счисления

система C — система счисления результата сложения

Файл может содержать пустые и невалидные строки. Программа должна корректно это обрабатывать.

Пример: сложить FF16 и HELLO36, результат вывести в двоичной системе счисления

```
16: "FF"
36: "HELLO"
2
```

Выходные данные

Файл с результатом в виде строки в том же формате записи как и исходные данные. Приведенный выше пример должен дать следующий ответ:

```
2: "1101111100001011011011011"
```

### Задача 3. Обход графа
Реализовать обход графа для поиска необходимой вершины с сохранением пути обхода.

Входные данные
Программа через консоль принимает путь до файла, где описан граф (возможно с несколькими компонентами связности). После того, как программа загрузила граф, программа запрашивает у пользователя две вершины графа (откуда и куда нужно построить путь).

Текстовый файл содержащий набор ребер графа:

```
<вершина N>-<вершина K>
```

Типы данных:

```
string - string
```
Постановка задачи

Необходимо реализовать программу, которая по двум вершинам может определить, есть ли путь между ними в имеющемся графе, и вывести этот путь в консоль. По сути, требуется реализовать обход графа для поиска необходимой вершины с сохранением пути обхода.

### Задача 4. Построение минимального остовного дерева

Построить минимальное остовное дерево из точек на плоскости.

Входные данные
Текстовый файл с набором пар координат на плоскости.

```
<pont_id1>, <x1>, <y1>
<pont_id2>, <x2>, <y2>
...
```
Пример:

```
1, 7, 5
2, 3, -11
3, 2, -2
4, -1, -8
5, 14, -14
```

Файл может содержать пустые и невалидные строки. Программа должна корректно это обрабатывать.

Задача
Необходимо связать множестве точек на плоскости таким образом, чтобы сумма длин рёбер была минимальна. Для решения задачи необходимо использовать Алгоритм Крускала. Длинна ребра между любой парой точек вычисляется стандартно, как геометрическое расстояние между точками.

Выходные данные
Текстовый файл, описывающий ребра построенного дерева:

```
<pont_id1> - <pont_id2>
...
```
Пример:
```
1 - 3
2 - 4
2 - 5
3 - 4
```

### Задача 5. Частотный словарь

Частотный словарь (или частотный список) — набор слов данного языка (или подъязыка) вместе с информацией о частоте их встречаемости.

Необходимо разработать программу, составляющую частотный словарь на основе входного текста.

Входные данные
Текстовый файл, содержащий большое количество слов.

Задача
Разработать программу, которая подсчитывает, сколько раз каждое из слов встречается во входном файле, и выводит результаты в другой файл. Алгоритм должен иметь возможность использования различных STL-контейнеров для своей работы (std::list, std::vector, std::map, std::set, std::unordered_set, std::unordered_map).

Пример вывода
Вывод в текстовый файл:

Время работы программы.
Вывод всех слов и количества их вхождений в формате: “слово” - “количество вхождений”.

```
Time - 100.2 sec
-----------------

my - 10
hey - 100
student - 1
...

```
## Блок 3. Классы.

### Задача 1. Алгоритм Дейкстры.

Текстовый файл содержит описание всех ребер графа в формате:

```
<вершина N>-<вершина K>-<вес ребра>
```

Типы данных:

```
string - string - unsigned int
```

Пример:

```
Moscow Novosibirsk 7
Moscow Toronto 9
Moscow Krasnoyarsk 14
Novosibirsk Toronto 10
Novosibirsk Omsk 15
Omsk Toronto  11
Toronto Krasnoyarsk 2
Krasnoyarsk Kiev 9
Kiev Omsk  6
```

Можно использовать любые разделительные знаки

Постановка задачи

Разработать модуль для расчета наикратчайших путей от точки А до точки B.

Выходные данные

Выводить данные можно в формате:

```
Вершины - {кратчайший путь} - общий вес пути
```

Пример

```
{Moscow, Toronto, Krasnoyarsk} - 11
```

### Задача 2. Разбор SRT - субтитров

Разработать программу, которая осуществляет разбор и упорядочивание субтитров, прочитанных из файла в простом формате SRT.

Входные данные

Файл в формате SRT.

Пример.

```
1 
00:00:01,000 --> 00:00:05,500
Раз
два
три

2 
00:00:06,000 --> 00:00:09,500
четыре пять

3
00:00:10,000 --> 00:00:15,500
вышел 
зайчик 
погулять

4
00:00:11,000 --> 00:00:19,500
белый зайчик

5
00:00:12,000 --> 00:00:14,500
наглый зайчик

6
00:00:13,000 --> 00:00:16,500
вышел
и ушел

7
00:00:15,000 --> 00:00:17,500
топтун

8
00:00:19,500 --> 00:00:19,600
вот и всё
```
Задача

Необходимо реализовать программу, которая вычитывает субтитры из файла, и превращает их в отсортированный по времени набор команд SHOW TEXT \ HIDE TEXT (см. пример в выходных данных). При этом не гарантируется, что во входном файле субтитры будут в корректном, отсортированном по времени порядке. Так же допускается наличие во входном файле "пересекающихся" и даже "вложенных" по времени субтитров (см. пример во входных данных).

Тестирование

Необходимо реализовать набор тестов, проверяющих работу модулей. В частности необходимо проверить корректность методов чтения и упорядочивания субтитров для ряда частных случаев:

• некорректные номера записей,

• некорректные времена показа / скрытия,

• отсутствие в конце файла 2 пустых строк,

• пересечение времён показа двух и более записей (обратить внимание на порядок объединения строк),

• "вложенность" времён показа двух и более записей,

• и другие случаи на усмотрение студента.

Выходные данные

Файл с командами показа / скрытия субтитров.

Для приведённого выше примера вывод должен быть примерно следующим:

```
at 1,00 show 'Раз
два
три'
at 5,50 show ''
at 6,00 show 'четыре пять'
at 9,50 show ''
at 10,00 show 'вышел
зайчик
погулять'
at 11,00 show 'вышел
зайчик
погулять
белый зайчик'
at 12,00 show 'вышел
зайчик
погулять
белый зайчик
наглый зайчик'
at 13,00 show 'вышел
зайчик
погулять
белый зайчик
наглый зайчик
вышел
и ушел'
at 14,50 show 'вышел
зайчик
погулять
белый зайчик
вышел
и ушел'
at 15,00 show 'вышел
зайчик
погулять
белый зайчик
вышел
и ушел
топтун'
at 15,50 show 'белый зайчик
вышел
и ушел
топтун'
at 16,50 show 'белый зайчик
топтун'
at 17,50 show 'белый зайчик'
at 19,50 show 'вот и всё'
at 19,60 show ''
Действия
 К списку задач
 Другие задачи того же блока

```

### Задача 3. Внешняя сортировка

Разработать программу, реализующую алгоритм внешней сортировки.

Примечание: внешняя сортировка применяется при сортировке больших объемов данных, которые невозможно целиком поместить в оперативную память.

Входные данные

1. Файл, заполненный большим количеством целых чисел.
2. Максимальное количество элементов, которое разрешается хранить в памяти.

Задание

Реализовать модуль, с помощью которого возможно провести сортировку слиянием (merge sort) и вывести отсортированный массив в другой файл.

Выходные данные

Файл с отсортированным списком чисел.

Тестирование

Разработайте систематический набор тестов с массивами разной длины и разным количеством элементов, который разрешается хранить в памяти.

### Задача 4. Friends of friends

Задача «Friends of Friends»

Моделируем работу движка социальной сети.

Дан граф френдования в формате IDюзера IDфренда1 IDфренда2 …:

```
vasya kolya petya sasha
sasha vasya cool123 petya
cool123 kolya
kolya cool123
```

Здесь у юзера vasya 3 френда и т.д. Примем, что IDюзера может содержать цифры и латинские буквы обоих регистров.

Также дан список постов в социальной сети: Время IDавтора Текст:

```
2014-10-20T08:00:00 vasya Блин, сегодня опять понедельник :(
2014-10-20T08:05:00 cool123 Ура, понедельник!!!
2014-10-20T08:39:00 kolya Какой сегодня день?
```

(время в формате ISO8601)

Задача

Для заданного userID построить френдленту (все посты френдов, отсортированные по времени в порядке убывания, т.е. самые свежие в начале), а также ленту «friends of friends», т.е. посты френдов и френдов френдов, также отсортированные по времени в порядке убывания. Посты не должны повторяться.

### Задача 5. Obeder

При совместном обеде в кафе или ресторане часто возникает проблема расчётов: поиск сдачи и т.д. Если одна и та же компания людей обедает регулярно, проще каждый раз платить за всех кому-то одному, по очереди. Но в этом случае необходимо вести учёт, чтобы периодически сводить все долги друг другу к нулю.

В рамках данной задачи необходимо разработать программу, автоматизирующую подсчёт таких долгов.

Входные данные

Файл журнала обедов, каждая строчка которого содержит следующую информацию:

• Время обеда ts — POSIX timestamp.

• Идентификатор пользователя user_id – строковый никнейм (например, superIgor2000)

• Сумма sum – число в копейках. Может быть как положительной (заплатил за всех), так и отрицательной (долг за еду).

• Таким образом, формат одной строчки:

```
ts user_id sum
```
Задача

Реализовать модуль, который инкапсулирует записи обо всех тратах и долгах пользователей и может выдать рекомендации в формате «кто, кому и сколько должен отдать» за определенный период времени (при заданных timestamp начала и конца периода).

Примечания

1.К реализуемому модулю могут производиться множественные запросы. Каждый раз читать файл не следует.

2.В реализуемом классе лучше хранить простые структуры с нужной информацией. Необходимо будет реализовать парсер, который преобразует текстовую строку в такую структуру.

3.При выборе контейнера для хранения записей стоит задуматься о производительности приложения.

4.Интересные усовершенствования функционала приветствуются и будут оцениваться дополнительно.

Пример вывода в консоль

```
user_1 -> user_2  - 301 р 50 коп
user_1 -> user_5  - 101 р 34 коп
user_4 -> user_3  - 1 р 01 коп
```
## Блок 4. SOLID

### Задача 1. Упорядоченные множества

Реализовать библиотеку для выполнения классических операций над объектами:
множество с , частично-упорядоченное множество(ЧУМ), решёточно-упорядоченное
множество(РУМ), линейно-упорядоченное множество(ЛУМ):

1) проверка выполнения транзитивность для операции сравнения между элементами
массива;

2) топологическая сортировка массива по возрастанию (текущий элемент
отсортированного массива больше или не сравним чем все предыдущие);

3) поиск максимальных элементов в массиве.

С помощью созданной библиотеки решите несколько прикладных задач:

1) Найти самых умных студентов в группе.

2) Зная функцию вероятности победы каждой команды любого футбольного матча на
турнире. Составить турнирную таблицу по олимпийской системе так, чтобы
заданная команда выиграла как можно больше матчей.

### Задача 2. Кто где был?

Задача «Кто где был?»

Справка

Timestamp - https://ru.wikipedia.org/wiki/Timestamp

Входные данные

1) Файл логов, каждая строчка которого содержит следующую информацию:

Момент времени ts (в формате timestamp)

Идентификатор пользователя user_id (в формате целого положительного числа)

Географические координаты (два числа - широта, долгота)

2) Файл географических координат известных мест, каждая строчка которого содержит следующую информацию:

Название места (в строковом представлении)

Географические координаты (четыре числа - координаты(широта, долгота) 2 точек, по которым можно построить прямоугольник)

Задача

Реализовать набор классов, который позволит:

1) Сгенерировать отдельный файл для каждого user_id, в котором будут записи, касающиеся только этого user_id, упорядоченные по времени, в которых, к тому же будет содержаться информация о названии места, в котором находился пользователь (вычисленное по координатам по второму файлу). Считается, что пользователь был в некотором месте, если его координаты попадают в прямоугольник, соответствующий месту.

2) Вывести в этот же файл маршрут всех перемещений пользователя в формате <место1> - <место2> - ... - <местоN>

### Задача 3. Записная книжка

Сделать записную книжку с набором функций доступных с командной строки:
• Добавить запись

• Удалить запись

• Вывести все записи, отсортированные по времени добавления

• Вывести отсортированные по времени добавления записи из заданного

интервала времени, содержащие в заголовке ключевые слова.
Данные записной книги сериализуются в файл формата JSON. Для работы с форматом
JSON рекомендуется использовать следующую header-only библиотеку: https://github.com/nlohmann/json. Для анализа параметров 
коммандной сроки, так же рекомендуется использовать сторонние библиотеки.

### Задача 4. Анализ GPS трека

Провести анализ GPS трека

Входные данные
GPS-трек в формате GPX с информацией о высоте. [Пример](https://gist.github.com/be9/bad0196a86175ea95028).

Задача
Необходимо для этого трека провести анализ и вычислить следующие показатели:

Общая продолжительность по времени
Расстояние
Средняя скорость
Время движения и время стоянок
Средняя скорость движения (без учёта стоянок)
Максимальная скорость
Минимальная высота
Максимальная высота
Общий набор высоты
Общий спуск
Распределение скорости по времени (набор пар: диапазон скоростей, длительность). Например, пара 8-14, 15:37 будет означать, что движение со скоростью от 8 до 14 км/ч шло в течение 15 минут 37 сек. Предусмотреть задание параметров для вычисления гистограммы.
Вычисление расстояний по координатам

[Выходные данные](https://github.com/be9/oopcode/tree/master/wgs_84)

Текстовый файл с результатом анализа.

[Пример анализа трека](http://www.gpslib.ru/tracks/info/50467/Gonka-IV-Etap-velokavkaza.html).

### Задача 5. Бюджет.

Сделать программу, которая проверяет исполнение бюджет.

Входные данные

Файл с расходами

Формат - позиция, ожидаемое значение, реальное значение, процент исполнения

```
24.11.2014 Авто:Бензин  1400
23.11.2014 Продукты:Мясо 500
23.11.2014 Продукты:Хлеб 90.33
14.11.2014 Общепит:Столовка 190
```

Статья расходов представляет собой «путь в дереве», где имена узлов от корня разделяются двоеточием. Даты идут не по порядку.

Файл бюджета

Формат - статья расходов, сумма

```
Авто               6000
Продукты + Общепит 10000
Алкоголь:Пиво      900
```

Здесь статьи расходов — это одно или несколько «поддеревьев» в общем «дереве расходов» (несколько поддеревьев сочетаются знаком +). Например, для приведённого выше примера в узле Продукты сумма составит 590.33, а вместе с Общепитом – 780.33. Эту сумму и нужно сопоставить с ожидаемым значением 10000.

Задача

Программа должна получать через аргументы командной строки следующую информацию: файл бюджета, файл расходов, две даты (начало и конец) и вычислять для каждой позиции бюджета реальную сумму.

Выходные данные

Текстовый файл

Формат -позиция, ожидаемое значение, реальное значение, процент исполнения

```
Авто               6000   1400    23.3%
Продукты + Общепит 10000  780.33  7.80%
Алкоголь:Пиво      900    0       0%
<Прочие расходы>   0      0
```

Если расходы по статье будут превышать ожидаемые, процент будет более 100%. В графу <Прочие расходы> попадут все расходы, которые не были отражены ни в одной статье бюджета.



