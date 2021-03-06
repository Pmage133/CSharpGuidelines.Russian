<!--
NOTE: Requires Markdown Extra. See http://michelf.ca/projects/php-markdown/extra/
 --> 

# 8. Рекомендации по использованию фреймворка

### <a name="av2201"></a> Используйте псевдонимы типов C# вместо типов из пространства имен `System` (AV2201) ![](images/1.png)
Например, используйте `object` вместо `Object`, `string` вместо `String` и `int` вместо `Int32`. Эти псевдонимы были введены для того, чтобы сделать примитивные типы первоклассными членами языка C#, так что используйте их должным образом.

**Исключение:** При ссылке на статические элементы таких типов обычно принято использовать полное CLS имя, например, `Int32.Parse()` вместо `int.Parse()`. То же самое применимо к методам, которые подчеркивают конкретный тип возвращаемого значения. Например: `ReadInt32`, `GetUInt16`. 

### <a name="av2205"></a> Тщательно задавайте названия свойств, переменных или полей, ссылающихся на локализованные ресурсы (AV2205) ![](images/3.png)
Рекомендации в этом разделе применимы к локализуемым ресурсам, таким как сообщения об ошибках и текст меню.

- Используйте нотацию паскаль для ключей ресурсов.
- Предпочитайте описательные названия коротким. Старайтесь делать их лаконичными, но не потеряйте читаемость.
- Используйте только буквенно-цифровые знаки при именовании ресурсов.

### <a name="av2207"></a> Не оставляйте в коде строки, которые должны быть изменены во время развертывания приложения (AV2207) ![](images/3.png)
Например, строки подключения, адреса серверов и т.д. Используйте файлы ресурсов, свойство `ConnectionStrings` класса `ConfigurationManager` или класс `Settings`, генерируемый Visual Studio. Поддерживайте актуальные значения настроек через `app.config` или `web.config` (а не в каком-либо другом месте).

### <a name="av2210"></a> Осуществляйте сборку с наивысшим уровнем предупреждений (AV2210) ![](images/1.png)
Сконфигурируйте свое рабочее окружение таким образом, чтобы использовать уровень предупреждений 4 для компилятора C# и включите опцию «Treat warnings as errors» (считать предупреждения за ошибки). Это позволит компилировать код с наивысшим уровнем качества из возможного.

### <a name="av2215"></a> Тщательно заполняйте атрибуты в файле `AssemblyInfo.cs` (AV2215) ![](images/3.png)
Удостоверьтесь, что атрибуты для названия компании, описания, предупреждения об авторском праве и т.д. заполнены. Одним из способов добиться того, чтобы номер версии и другие поля, общие для всех сборок, были всегда одинаковыми, является перемещение соответствующих атрибутов из `AssemblyInfo.cs` в файл `SolutionInfo.cs`, который совместно используется всеми проектами внутри решения. 

### <a name="av2220"></a> Избегайте использования LINQ для простых выражений (AV2220) ![](images/3.png)
Вместо:

	var query = from item in items where item.Length > 0 select item;

Лучше воспользоваться методом из пространства имен System.Linq:

	var query = items.Where(item => item.Length > 0);

К тому же LINQ-запросы должны быть разбиты на несколько строк для читаемости. Таким образом, второе выражение выглядит более компактно.

### <a name="av2221"></a> Используйте лямбда-выражения вместо анонимных функций (AV2221) ![](images/2.png)

Лямбда-выражения служат более красивой альтернативой анонимным функциям. Таким образом, вместо:

	Customer customer = Array.Find(customers, delegate(Customer customer)
	{
		return customer.Name == "Tom";
	});

используйте лямбда-выражение:

	Customer customer = Array.Find(customers, customer => customer.Name == "Tom");

или даже лучше это: 

	var customer = customers.Where(c => c.Name == "Tom");

### <a name="av2230"></a> Используйте ключевое слово `dynamic` только при работе с объектами этого типа (AV2230) ![](images/1.png)
Ключевое слово `dynamic` было введено для работы с динамическими языками. Их использование создает серьезный затор производительности (performance bottleneck), поскольку компилятор вынужден сгенерировать некоторое количество дополнительного кода.

Используйте dynamic только при обращении к членам динамически созданного экземпляра класса (при использовании класса `Activator`) в качестве альтернативы `Type.GetProperty()` и `Type.GetMethod()` или при работе с типами COM Iterop.

### <a name="av2235"></a> Старайтесь использовать `async`/`await` вместе с `Task` (AV2235) ![](images/1.png)
Использование новых ключевых слов C# 5.0 упрощает создание и чтение кода, что, в свою очередь, упрощает его поддержку. Даже если вам требуется создавать многочисленные асинхронные операции. Например, вместо того, чтобы делать так:

	public Task GetDataAsync()
	{
	  return MyWebService.FetchDataAsync()
	    .ContinueWith(t => new Data(t.Result));
	}

объявляйте метод следующим образом: 

	public async Task<Data> GetDataAsync()
	{
	  string result = await MyWebService.FetchDataAsync();
	  return new Data(result);
	}

**Подсказка:** Если даже вам необходим .NET Framework 4.0 вы все равно можете использовать `async` и `await`. Просто установите [Async Targeting Pack](http://www.microsoft.com/en-us/download/details.aspx?id=29576).
