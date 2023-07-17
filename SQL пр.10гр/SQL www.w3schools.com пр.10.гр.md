Создание запросов по описанию для сервиса [https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_in](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_in) со скриншотами результатов.

**Customers**

SELECT * FROM [Customers] where  City  like "B%"

Выбрать все города, которые начинаются с буквы “B”


![img_6.1.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.1.png)


SELECT * FROM [Customers] where City like "B%" and Country = "Germany"

Выбрать все города, которые начинаются с буквы “B”, находящиеся в Германии


![img_6.2.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.2.png)


SELECT City, Country FROM [Customers] order by Country

Выбрать столбец с городами и столбец со странами и отсортировать по странам


![img_6.3.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.3.png)



**Categories**

SELECT * from Categories where CategoryName like "C%"

Выбрать все категории на букву "C"


![img_6.4.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.4.png)


SELECT CategoryName from Categories

Показать наименования категорий


![img_6.5.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.5.png)


SELECT * from Categories where Description like "%breads%"

Показать категории, где в описании есть слово "breads"


![img_6.6.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.6.png)


**Employees**

SELECT * from employees

Показать таблицу полностью


![img_6.7.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.7.png)


SELECT * from employees order by FirstName

Показать таблицу, отсортированную по фамилии


![img_6.8.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.8.png)


SELECT * from employees where (BirthDate >= "1968-12-08") order by FirstName

Показать таблицу работников, рожденных после 1968-12-08 и отсортировать по фамилии


![img_6.9.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.9.png)


**Orderdetails**

SELECT * from Orderdetails where Quantity > 80

Показать все строки, в которых количество больше 80


![img_6.10.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.10.png)


Select ProductID, sum(Quantity) from Orderdetails group by ProductID

Вывести количество каждого товара и его ID


![img_6.11.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.11.png)


Alter table Orderdetails rename column OrderDetailID to ID;

select * from  Orderdetails

Данный запрос в тренажере можно вывеси двумя запросами поочередно.

- Сначала переименовываем столбец

- далее выводим таблицу полностью: столбец переименован


![img_6.12.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.12.png)


SELECT * FROM OrderDetails where Quantity > 70 order by ProductID

Вывести все строки, где количество больше 70 и отсортировать по ProductID


![img_6.13.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.13.png)


**Orders**

SELECT * FROM [Orders] where CustomerID = 85

Выбрать строки, где CustomerID = 85


![img_6.14.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.14.png)


SELECT EmployeeID, count(EmployeeID) as employee_orders from orders group by EmployeeID having employee_orders >=2

Выбрать ID  работников, количество заказов и вывести тех работников, у которых количество заказов больше или равно 2


![img_6.15.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.15.png)


SELECT EmployeeID, count(EmployeeID) as employee_orders from orders where OrderDate like "1997%"  group by EmployeeID having employee_orders >=2

Выбрать ID  работников, количество заказов и вывести работников, у которых количество заказов после 1997 г. больше или равно 2


![img_6.16.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.16.png)


**Products**

SELECT * FROM [Products] where CategoryID = 1 and Price <= 10

Выбрать товары с ID=1 и ценой меньше 10


![img_6.17.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.17.png)


SELECT CategoryID, count(CategoryID) FROM [Products] group by CategoryID

Выбрать ID категории, подсчитать количество товаров в категории и сгруппировать по категории (сколько в каждой категории товаров)


![img_6.18.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.18.png)


SELECT * FROM [Products] where ProductName like "%Louisiana%"

Выбрать все товары, в названии которых есть «Louisiana»


![img_6.19.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.19.png)


Shippers

SELECT * FROM [Shippers] where Phone like "%503%"

Выбрать всех грузоотправителей в номере телефона которых есть 503


![img_6.20.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.20.png)


SELECT Phone FROM [Shippers] where ShipperName = "Federal Shipping"

Вывести телефон грузоотправителя Federal  Shipping


![img_6.21.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.21.png)


SELECT  ShipperName  FROM  Shippers

Выбрать наименования грузоотправителей


![img_6.22.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.22.png)


**Suppliers**

SELECT SupplierName, Address, City, Country FROM [Suppliers] order by Country

Выбрать грузоотправителя, адрес, город и страну и отсортировать по странам


![img_6.23.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.23.png)


SELECT Country, count(Country) as count_cities FROM Suppliers group by Country order by Country desc

Вывести страны и подсчитанное количество городов в них, сгруппированное по странам и отсортированное в убывающем порядке


![img_6.24.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.24.png)


SELECT Country, count(Country) as count_cities FROM Suppliers group by Country having count_cities >= 3 order by Country desc

Вывести страны и подсчитанное количество городов в них, сгруппированное по странам и отсортированное в убывающем порядке, где количество городов не меньше 3-х


![img_6.25.png](https://gitlab.com/school21_tangelos/10_project/-/raw/main/6.25.png)

