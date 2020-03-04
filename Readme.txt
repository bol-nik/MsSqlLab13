Лабораторная работа № 13
1.	Написать функцию, которая возвращает количество сотрудников, которые ничего не продавали в феврале 2011 года.
2.	Написать функцию, которая возвращает самый дорогой товар из таблицы PRODUCT.
3.	Создайте последовательный курсор, в котором определяется, сколько свитеров продал каждый сотрудник из Брюсселя с 10 по 20 февраля 2011 года.
4.	Создайте локальный динамический курсор с прокруткой, который содержит список клиентов фирмы. С помощью курсора и связанных переменных:
a)	выведите отдельно последнюю запись;
b)	выведите отдельно 2-ю запись с начала;
c)	выведите отдельно 3-ю запись с конца.

Примеры:
Последовательный курсор:
Declare MyCursor cursor local fast_forward
for
select sp_name, comm from sperson;
declare @CursorVariable cursor;
open MyCursor;
set @CursorVariable=MyCursor;
close MyCursor;
deallocate MyCursor;
С помощью динамического курсора возвращается запись в его текущей позиции
declare @v1 varchar(40);
declare spers_cursor cursor
for select sp_name from sperson;
open spers_cursor;
fetch next from spers_cursor into @v1;
close spers_cursor;
deallocate spers_cursor;
print @v1;

declare spers_cursor cursor
for select sp_name from sperson;
open spers_cursor;
fetch next from spers_cursor;
close spers_cursor;
deallocate spers_cursor;
Возвращается все множество данных курсора
Declare spers_cursorcursor
For select sp_name from sperson
Where of_id=(select of_id from office
where office='Chicago');
open spers_cursor;
fetch next from spers_cursor;
while @@fetch_status=0
fetch next from spers_cursor;
close spers_cursor;
deallocate spers_cursor;

Курсор с прокруткой
Declare spers_cursor cursor local scroll
For select s.sp_name, o.office
from office o join sperson s on o.of_id=s.of_id
order by 1;
open spers_cursor;
fetch last from spers_cursor;
fetch prior from spers_cursor;
fetch absolute 2 fromspers_cursor;
fetch absolute 3 fromspers_cursor;
fetch relative-2 fromspers_cursor;
close spers_cursor;
deallocate spers_cursor;

Позиционированное обновление данных
declare cn_cursor cursor local keyset
for
select cn_id, country from country for update;
open cn_cursor;
fetch absolute 6 from cn_cursor;
update country set country='USA'
where current of cn_cursor;
select * from country where country='USA';
close cn_cursor;
deallocate cn_country;
Позиционированное удаление  данных.
В курсоре набор данных о товарах, цена которых выше средней цены товаров.
declare prod_cursor cursor
for
select p_id, p_desc, price from product 
where price >(selectavg(price)from product)order by 3 desc;
open prod_cursor;
fetch from prod_cursor;
delete from product where
current of prod_cursor;
close prod_cursor;
deallocate prod_cursor;
select * fromproduct;

