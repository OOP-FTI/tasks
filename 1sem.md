### Задание №0 (вводное). Раздельная компиляция и пространства имен (namespaces)

Познакомьтесь с раздельной компиляцией и пространством имен в C++. В качестве примера используйте следующую программу:


File module1.h:
```c++
#include <string>

namespace Module1
{
	std::string getMyName();
}
```

File module1.cpp
```c++
#include "module1.h"

namespace Module1
{
	std::string getMyName()
	{
		std::string name = "John";
		return name;
	}
}

```


File module2.h:
```c++
#include <string>

namespace Module2
{
	std::string getMyName();
}
```


File module2.cpp
```c++
#include "module2.h"

namespace Module2
{
	std::string getMyName()
	{
		std::string name = "James";
		return name;
	}
}
```


File main.cpp:
```c++
#include "module1.h"
#include "module2.h"
#include <iostream>

int main(int argc, char** argv)
{
	std::cout <<  "Hello world!" << "\n";
	
	std::cout << Module1::getMyName() << "\n";
	std::cout << Module2::getMyName() << "\n";

	using namespace Module1;
	std::cout << getMyName() << "\n"; // (A)
	std::cout << Module2::getMyName() << "\n";

	//using namespace Module2; // (B)
	//std::cout << getMyName() << "\n"; // COMPILATION ERROR (C)

	using Module2::getMyName;
	std::cout << getMyName() << "\n"; // (D)

}
```

С тестовой программой нужно выполнить следующие действия:

1. Собрать программу и убедиться, что на каждый *.cpp файл создается отдельный объектный файл с тем же именем.

2. Убедиться, что при изменении одного *.cpp файла и пересборке проекта обновляется только соответствующий ему объектный файл (дата изменения других объектных файлов останется прежней)

3. Объяснить, что выведется при выполнении строк с комментариями (А) и (D) в main.cpp

4. Убедиться, что раскомментирование строк (B) и (C) в main.cpp приводит к ошибке компиляции. Объяснить, почему эта ошибка происходит, и предложить пути её устранения.

5. Добавить в программу еще одну функцию getMyName(), возвращающую имя Peter. Обернуть её в еще одно пространство имён.

6. Объяснить, как можно избавиться от необходимости писать std::cout и вместо этого писать просто cout.

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

Дополнительно: попробуйте часть методов протестировать до их реализации.

```c++
//Image.hpp
#include "Range.h"

enum class MirrorType {
  Vertical,
  Horizontal
}

class Image {

public:
    Image();
    Image(int rows, int cols, int channels);
    Image(int rows, int cols, int channels, unsigned char* data);
    Image(const Image& image);
    virtual ~Image();

    Image& operator=(const Image& image);

    // Вернуть клон изборажения
    Image clone();
    //Скопировать изображение.
    void copyTo(Image& image);
    void create(int rows, int cols, int channels);
    bool empty();

    //декрементирует счетчик ссылок и в случае необходимости освобождает ресурсы (память).
    void release();

    Image col(int x);

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

    Image zeros(int rows, int cols, int channels);
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


