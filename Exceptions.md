# Exceptions
```c++
void do_something()
{
 File* file = fopen("1.txt");
 if(!file)
	 return false;
  ssize_t bytes_read = fread(..., file);
  if(bytes_read<0)
	  return false;
}
```
Неудобно.
``` c++
if(...)
{
	throw runtome_error("f() error");
}
```
```c++
struct derived : base
{
	std::string msg() const
	{
		return "derived";
	}
}
```

```c++
try
{
throw derived();
}
catch(base const* e)
{
	std::cout<<e->msg();
}
```
Если убрать **const***, выведется не **derived**, а **base**
**!!!** throw; - **Пробросить исключение дальше**
```c++
try
{
	try
	{
		 throw derived();	
    }
   catch (base const& e1)
   {
	   std::cout << e1.msg();
	   throw e1;
   }
}
catch (base const& e2)
{
	std::cout << e2.msg();
}
```
В данном случае выведет **derived**, **base**
throw бросает исключение статического типа
## Best practices
* Можно хранить данные об ошибках
* Можем при необходимости останавливать вычисления
 ## Списки инициализации
  ``` c++
    struct config_file
    {
	    config_file()
		    :f1("1.txt");
		    ,f2("2.txt");
		    ,f3("3.txt");
		    {
		    //...
		    }
		config_file(int)
		    :f1("1.txt");
		    ,f2("2.txt");
		    ,f3("3.txt");
		    {
		    //...
		    }  
    }
    ```
    ```c++
    struct unique_ptr] 
    {
	   explicit  unique_ptr(T* p):p(){ }  //не учавствует в неявных превидениях
	  
    unique_ptr():p(nullptr) { }

    ~unique_ptr()
	{
	     delete p;
	}

	T& operator *() const
    {
		return *p;
    }

	T* operator ->() const
	{
	  return p;
    }

	T* get() const
	{
	  return p;
	}
	T* peleae()
	{
		T* tmp = p;
		p = nulptr;
		return tmp;
	}
	  private :
	  T* p;
 };
 int main()
 {
	 int* q=new int(5);
	f(q);
	f(q);
	delete q;
 }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MTQ5MTk2MDhdfQ==
-->