# Соглашение о принципах кодирования
## Соглашение о именовании
В следующих примерах все указания, относящиеся к элементам с пометкой `public`, также применимы при работе с `protected` и `protected internal` элементами, которые должны быть видны внешним вызывающим пользователям.

### Pascal case
- Используется при именовании `class`, `record`, `struct`.
    ```csharp
    public class DataService
    {
    }

    public record PhysicalAddress(
        string Street,
        string City,
        string StateOrProvince,
        string ZipCode);
        
    public struct ValueCoordinate
    {
    }
    public interface IWorkerQueue
    {
    }
    ```

- В интерфейсах **Pascal case** используется вместе с буквой I в начале имени.
    ```csharp
    public interface IWorkerQueue
    {
    }
    ```

- При именовании `public` членов типов, таких как поля, свойства, события, методы и локальные функции, используйте **Pascal case**.
    ```csharp
    public class ExampleEvents
    {
        // A public field, these should be used sparingly
        public bool IsValid;

        // An init-only property
        public IWorkerQueue WorkerQueue { get; init; }

        // An event
        public event Action EventProcessing;

        // Method
        public void StartEventProcessing()
        {
            // Local function
            static int CountQueueItems() => WorkerQueue.Count;
            // ...
        }
    }
    ```

- При написании `record` используйте **Pascal case** для параметров, так как они являются публичными свойствами записи.
    ```csharp
    public record PhysicalAddress(
        string Street,
        string City,
        string StateOrProvince,
        string ZipCode);
    ```


### Camel case
- Используйте **Camel case** для именования `private` и `internal` полей и ставьте перед их именем `_`.
    ```csharp
    public class DataService
    {
        private IWorkerQueue _workerQueue;
    }
    ```

- При написании параметров метода и локальных переменных используйте **Camel case**
    ```csharp
    public void SomeMethod(int someNumber, bool isValid)
    {
        int anotherNumber = 47;
    }
    ```

## Соглашение о компоновке
- В качестве отступов используем табы, а не пробелы. 
- Одно выражение на строку.
- Одно объявление на строку.
- Один отступ при переносе выражения на следующую строку, знак операции не переносится.
    ```csharp
    Value = x1 + x2 + x3 + x4 +
        x5 + x6;
    ```
- Помещаем скобку, связанную с управляющим оператором, на следующую строку с отступом на том же уровне, что и управляющий оператор. Утверждения внутри скобок отступают на следующий уровень ([Allman style](https://en.wikipedia.org/wiki/Indentation_style#Allman_style)).
- При переносе аргументов функции необходимо начинать следующую строку ...
    ```csharp
    new Value() { Compute(
        x1, x2, x3, x4, 
        x5, x6, x1, x2,
        x3, x4, x5, x6
    ) };
    ```

- Сначала поля и свойства, а затем методы.
- Одна пустая линия между объявлением свойств и методов.
- Используйте круглые скобки, чтобы сделать выражение понятнее, как показано в следующем коде.
    ```csharp
    if ((val1 > val2) && (val1 > val3))
    {
        // Take appropriate action.
    }
    ```
- Старайтесь избегать более одной пустой строки.
- Избегайте ложных свободных пробелов. Например, избегайте `if (someVar == 0)...`, где точки отмечают ложные свободные пробелы. Если вы используете Visual Studio, включите функцию "View White Space (Ctrl+R, Ctrl+W)" или "Edit -> Advanced -> View White Space", чтобы облегчить обнаружение пробелов.


### Циклы и условия
- Если в теле `if` выражения находится только одна строка, то нельзя ставить фигурные скобки.
- Если в одном из `if/else if/else' условий используются фигурные скобки, то во всех остальных условиях, за исключением завершающего else, обязательно должны использоваться фигурные скобки.
- Избегайте больших `if`.
    ```csharp
    // Плохой вариант
    if (isFlag) 
    {
        // a lot of code
    }

    // Хороший вариант
    if (!isFlag) return;
    // a lot of code
    ```
- Допускается написание циклов и условий в одну строку если ..., в ином случае оно запрещено.
    ```csharp
    if (isPossible) return Object;
    ```

## Соглашение о комментариях
- Размещайте комментарий на отдельной строке, а не в конце строки кода.
- Начинайте текст комментария с заглавной буквы.
- Завершите текст комментария точкой.
- Вставьте один пробел между ограничителем комментария (`//`, `/*`, `*/`) и текстом комментария.
- *Убедитесь, что все `public` члены имеют необходимые комментарии, содержащие соответствующие описания их поведения.*

## Руководство по языку
### Неявно типизированные локальные переменные
- Используйте неявную типизацию для локальных переменных, когда тип переменной очевиден из правой части присваивания, или когда точный тип не важен.
    ```csharp
    var var1 = "This is clearly a string.";
    var var2 = 27;
    ```

- Не используйте `var`, если тип не очевиден из правой части присваивания. Не считайте, что тип ясен из имени метода. Тип переменной считается ясным, если это оператор new или явное приведение.
    ```csharp
    int var3 = Convert.ToInt32(Console.ReadLine()); 
    int var4 = ExampleClass.ResultSoFar();
    ```

- Используйте неявную типизацию для определения типа переменной цикла в циклах `for`. В следующем примере используется неявная типизация в операторе `for`.
    ```csharp
    var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
    var manyPhrases = new StringBuilder();
    for (var i = 0; i < 10000; i++)
    {
        manyPhrases.Append(phrase);
    }
    //Console.WriteLine("tra" + manyPhrases);
    ```

- Не используйте неявную типизацию для определения типа переменной цикла в циклах `foreach`. В большинстве случаев тип элементов в коллекции не очевиден сразу. Не следует полагаться только на имя коллекции для вывода типа ее элементов.
    ```csharp
    foreach (char ch in laugh)
    {
        if (ch == 'h')
            Console.Write("H");
        else
            Console.Write(ch);
    }
    Console.WriteLine();
    ```  

### Беззнаковые типы данных
В целом, используйте типы `int`, а не `unsigned`. Использование `int` является общим для всего C#, и при использовании `int` проще взаимодействовать с другими библиотеками.

### BCL типы
Мы используем ключевые слова языка вместо типов BCL (например, `int`, `string`, `float` вместо `Int32`, `String`, `Single` и т.д.) как для ссылок на типы, так и для вызовов методов (например, `int.Parse` вместо `Int32.Parse`).

### `try-catch` и `using` выражения в обработке исключений
- Для обработки большинства исключений используйте оператор `try-catch`.
  ```csharp
  static string GetValueFromArray(string[] array, int index)
    {
        try
        {
            return array[index];
        }
        catch (System.IndexOutOfRangeException ex)
        {
            Console.WriteLine("Index is out of range: {0}", index);
            throw;
        }
    }
  ```
- Упростите свой код с помощью оператора C# `using`. Если у вас есть оператор `try-finally`, в котором единственным кодом в блоке `finally` является вызов метода `Dispose`, используйте вместо него оператор `using`.
  - В следующем примере оператор `try-finally` вызывает метод `Dispose` только в блоке `finally`.
    ```csharp
    Font font1 = new Font("Arial", 10.0f);
    try
    {
        byte charset = font1.GdiCharSet;
    }
    finally
    {
        if (font1 != null)
        {
            ((IDisposable)font1).Dispose();
        }
    }
    ```

  - То же самое можно сделать с помощью оператора `using`.
    ```csharp
    using (Font font2 = new Font("Arial", 10.0f))
    {
        byte charset2 = font2.GdiCharSet;
    }
    ```

  - В C# 8 и более поздних версиях используйте новый синтаксис `using`, который не требует скобок:
    ```csharp
    using Font font3 = new Font("Arial", 10.0f);
    byte charset3 = font3.GdiCharSet;
    ```   

### Операторы `&&` и `||`
Чтобы избежать исключений и повысить производительность за счет пропуска ненужных сравнений, используйте && вместо & и || вместо | при выполнении сравнений.

### Оператор `new`
- Используйте следующий синтаксис создания объектов:
  ```csharp
  ExampleClass instance2 = new();
  ```

### Оператор `using`
- Используем (в данном случае `namespace` проекта - `RP`) `using RP;`, а не `using RP {}`.
- Импорты пространств имен должны быть указаны в верхней части файла, вне объявлений пространств имен, и должны быть отсортированы в алфавитном порядке, за исключением пространств имен System.*, которые должны располагаться выше всех остальных.

### Видимость
Мы всегда указываем видимость, даже если она по умолчанию (например, `private string _foo` не `string _foo`). Видимость должна быть первым модификатором (например, `public abstract`, а не `abstract public`).

## Правильные числа
Рекомендуются для отладки(в том числе любое число с плавающей точкой, комбинированное из правильных чисел):
1. 30
2. 47
3. 3
4. 12
