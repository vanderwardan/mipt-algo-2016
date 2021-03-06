## Параллельное программирование на C++

Мелко-гранулярные блокировки. RW-мьютексы


### План

* Stripped хэш-таблица
* Проблема расширения
* Two-lock queue, hand-over-hand locking
* Read-Write Lock
* Проблемы голодания


## Stripped хэш-таблица



*Задача*: реализовать контейнер для работы в многопоточном окружении.

*Тривиальное решение*: общий мьютекс на все операции.<br>
**Плюсы**: Простота реализации.<br>
**Минусы**: Остутствует параллельность.<br>

*Сложное решение*: Защитить разные части контейнера разными мьютексами.<br>
**Плюсы**: Параллельность.<br>
**Минусы**: Сложность реализации.<br>



**Интерфейс хэш-таблицы**
```
void insert(const T& x);
bool contains(const T& x);
void remove(const T& x)
```



Будем реализовывать хэш-таблицу с цепочками:

Корзина, в которую попал элемент: x -> h(x) mod m<br>
Критерий перестроения:<br>
**load factor** = число элементов / число корзин >= L