# Наследование
## Виртуальные функции

 Пусть у нас есть два класса :

 ```c++
  struct base{
	  int a;
	  int b;
   }
 ```
 ```c++
   struct derived : base{
	   int c;
	   ind d;
   }
```
  **base** - базовый класс.
  **devived** - производный
     Компилятор ищет переменную сначала в производном классе, потом в базовом.
   ```c++
     derived &x =(derived&)y - ошибка
 ```
От производного к базовую мы может спокойно кастить.
Динамический тип -- то на что мы ссылаемся
Кастить можно, если то ,что мы кастим, является  базовым типом динамического типа.
|**base**| a | b |   |   |
|--|-- |-- | --| --|
|**derived**| a | b(base) |  b(derived)| c |

 ```c++
  struct base{
	  void f();
	  void g()
   }
 ```
 ```c++
   struct derived : base{
	   void  g();
	   void f();
   }
```
 Компилятор ищет функцию сначала в производном классе, потом в базовом.
 ```c++
 // in derived
 void g(){
 base:g();
 }

d.f() // base:f 
d.base::g(); // derived::g;
 ```
 Это означает, что я в  хочу вызвать функцию из определенного класса
 ## Механизм виртуалньой функции
 **virtual** функция, которая нашлась бы в динамическом типе.
  ``` c++
  struct derived : base{
	 virtual  void  g();
	   void f();
   }
   derived d;
   base& b2 = d;
   b2.f();
   b2.g(); //derived ::g;
   b2.h();
 ```
 ## Пример
 ``` c++
 struct data_source {
    virtual void read(buffer &);
    // virtual void read(buffer &) = 0;   чисто виртуальная функция абстрактная 
    virtual ~data_source(); 
    // без virtual его код некорректный
 }

 struct  local_file : data_source {
	 void read(buffer&)
 }

struct  http_connection : data_source {
	 void read(buffer&)
}

struct player{
   player(){
		current_source = nullptr;
   }
    
   void play_file(std:: string const& str){
		stop();
		delete current_source;
		current_source = new local_file(str);
	}
	
   data_source* current_source;
}
void play(data_source&);

 int main() {
	 local_file lf("1.flac");
	 play(if);
	 http_connection hc("2.flac");
	 play(hc);
 }
 ```
 ### Protected
 Модификатор доступа --  доступ имеют только производные класса
 ``` c++
struct x_doer{
    virtaul void do_x()=0;
 };
 
struct derived : x_doer{
     void do_x();
 };
 
 struct derived2: x_doer{
     void do_x();
 };

struct base{
	 base(x_doer&);
};
 
  ``` 

