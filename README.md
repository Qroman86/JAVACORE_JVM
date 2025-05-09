Код для исследования
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }

ClassLoader'ы (Bootstrap → Platform → System/Application) загружают класс JvmComprehension в Metaspace.

Строка 1:int i = 1;
В фрейме метода main() создается переменная i типа int.
Примитивное значение 1 записывается непосредственно в стековый фрейм.

Память: Stack (фрейм main)

Строка 2: Object o = new Object();
new Object():
В Heap (куче) создается новый объект Object.
Выделяется память под объект, инициализируются поля (в данном случае их нет).
Вызывается конструктор Object (пустой в данном случае).

Object o:
В стековом фрейме main создается ссылочная переменная o.
Эта переменная получает ссылку на созданный объект в куче.

Память:
Stack (фрейм main) - ссылка o
Heap - объект Object

Строка 3: Integer ii = 2;
Автоупаковка int в Integer.
Создается объект Integer со значением 2.


Integer ii:
В стековом фрейме main создается ссылочная переменная ii.
Переменная получает ссылку на объект Integer в куче.

Память:
Stack (фрейм main) - ссылка ii.
Heap - объект Integer. 

Строка 4: printAll(o, i, ii);
Подготовка к вызову метода printAll.
В Stack создается новый фрейм для printAll.
Параметры передаются в новый фрейм.

o - копируется ссылка на объект Object из кучи.
i - копируется примитивное значение 1.
ii - копируется ссылка на объект Integer.

Память:
Stack - новый фрейм для printAll с параметрами o, i, ii.
Heap - существующие объекты (Object и Integer).

Строка 5 (в методе printAll): Integer uselessVar = 700;
Создается новый объект Integer со значением 700. 
В стековом фрейме printAll создается переменная uselessVar с ссылкой на этот объект.

Память:
Stack (фрейм printAll) - новая ссылка uselessVar.
Heap - новый объект Integer(700).

Строка 6: System.out.println(o.toString() + i + ii);
o.toString():
Вызывается виртуальный метод toString() для объекта Object.
Создается новый объект String в куче с результатом.

Конкатенация строк:

i преобразуется в String (создается новый объект).
ii вызывает toString() (преобразуется в строку).
Все части соединяются в новый объект String.

Вывод:

Создается фрейм для метода println.
Передается ссылка на итоговый String.

Память:

Stack - временные фреймы для вызовов методов
Heap - новые объекты String для промежуточных и итоговой строк

Строка 7: System.out.println("finished");
Строковый литерал "finished" уже находится в String Pool (в куче), так как литералы загружаются при загрузке класса.
Вызывается println с ссылкой на этот объект.

Память:
Stack - фрейм для println
Heap - объект String из пула строк

Работа Garbage Collector (Сборщика мусора)
После завершения метода printAll:
Его фрейм удаляется из стека.
Переменная uselessVar (ссылка на Integer(700)) исчезает.
Объект Integer(700) становится недостижимым (нет ссылок).
GC может собрать этот объект в следующий цикл сборки мусора.

После завершения метода main:
Все созданные объекты (Object, Integer(2), Integer(700) если еще не собрано) становятся кандидатами на удаление.
Однако программа завершается, и вся память освобождается ОС.

