<!--
NOTE: Requires Markdown Extra. See http://michelf.ca/projects/php-markdown/extra/
 --> 

# 10. Рекомендации по оформлению

### <a name="av2400"></a> Используйте общие правила оформления (AV2400) ![](images/1.png)

- Держите длину строк в пределах 130 символов.

- В качестве отступов используйте 4 пробела и не используйте табуляцию.

- Вставляйте один пробел между ключевым словом (например `if`) и выражением, но не используйте пробелы после `(` и перед `)`. Например: `if (condition == null)`.

- Добавляйте пробел перед и после операторов (например `+`, `-`, `==` и т.д.)

- Всегда используйте конструкции `if`, `else`, `do`, `while`, `for` и `foreach` с парными фигурными скобками, даже если без этого можно обойтись. 

- Открывайте и закрывайте парные скобки всегда в новой строке.

- Не инициализируйте объект/коллекцию построчно. Используйте следующий формат:
	
		var dto = new ConsumerDto
		{
			Id = 123,
			Name = "Microsoft",
			PartnerShip = PartnerShip.Gold,
			ShoppingCart =
			{
				["VisualStudio"] = 1
			}
		};

- Не разделяйте объявление лямбда-выражения на несколько строк. Используйте формат, как показано ниже:

		methodThatTakesAnAction.Do(x =>
		{ 
			// какие-то действия
		}
		
- Не переносите выражение по частям на новую скроку. Разбивайте длинные строки как в следующем случае:

		private string GetLongText =>
					"ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC ABC";

- Объявляйте LINQ-запрос в одной строке или объявляйте каждое ключевое слово этого запроса в новой строке, как показано ниже:
		
		var query = from product in products where product.Price > 10 select product;

  	или
	
		var query =  
		    from product in products  
		    where product.Price > 10  
		    select product;

- Начинайте LINQ-запрос с объявления всех выражений `from` и не перемешивайте их с другими выражениями.

- Добавляйте скобки вокруг каждого сравнения в условном выражении, но не добавляйте их вокруг одиночного выражения. Например: `if (!string.IsNullOrEmpty(str) && (str != "new"))`

- Добавляйте пустую строку между многострочными выражениями, многострочными членами класса, после закрытия парных скобок, после несвязанных друг с другом блоков кода, перед и после ключевого слова `#region` и между объявлениями `using`, если пространства имен имеют различные корни (корневые пространства имен).


### <a name="av2402"></a> Располагайте и группируйте пространства имен в соответствии с названием компании (AV2402) ![](images/3.png)

	// Пространства имен Microsoft первые 
	using System;
	using System.Collections.Generic;	
	using System.XML;
	
	// Другие пространства имен идут в алфавитном порядке
	using AvivaSolutions.Business;
	using AvivaSolutions.Standard;
	using Telerik.WebControls;
	using Telerik.Ajax;
	
Статические директивы и псевдонимы должны быть объявлены после всех остальных.
Всегда помещайте директивы `using` вверху файла перед объявлением пространства имен (не внутри их).

### <a name="av2406"></a> Располагайте члены класса в строго определенном порядке (AV2406) ![](images/1.png)
Сохранение общего порядка позволяет другим членам команды легче ориентироваться в вашем коде. В общем случае, чтение файла с исходным кодом должно идти сверху вниз, как если бы вы читали книгу. Это предотвращает ситуацию, когда приходится просматривать файл вверх и вниз в поисках нужного фрагмента.

1. Приватные поля и константы (в `#region`)
2. Публичные константы
3. Публичные read-only статические поля
4. Фабричные методы
5. Конструкторы и финализаторы
6. События 
7. Публичные свойства
8. Прочие методы и приватные свойства в порядке их вызова

Объявляйте локальные функции в конце метода в котором они объявлены (после всего остального кода).

### <a name="av2407"></a> Будьте осторожны с использованием директивы `#region` (AV2407) ![](images/1.png)
Ключевое слово `#region` может быть полезно, но оно также может скрывать основное предназначение класса. Следовательно, используйте `#region` только для:

- Приватных полей и констант (предпочтительно в Private Definitions region).
- Вложенных классов
- Реализаций интерфейса (только если реализация интерфейса не является основной целью этого класса)
