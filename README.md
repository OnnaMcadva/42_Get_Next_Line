# Get_Next_Line
Reading a line from a fd is way too tedious

* This project is about programming a function that returns a line read from a file descriptor.

* This project will not only allow you to add a very convenient function to your collection, but it will also make you learn a highly interesting new concept in C programming: static variables.

## Функция get_next_line:

## [`get_next_line`](get_next_line.c)
- Эта функция предназначена для чтения следующей строки из файла с помощью дескриптора файла `fd`. 

- Принимает дескриптор файла `fd` в качестве аргумента.
- Использует статическую переменную `saved_str` для хранения необработанных данных между вызовами.
- Проверяет допустимость входных данных: если дескриптор файла неверный или размер буфера меньше или равен нулю, функция возвращает `0`.
- Вызывает функцию `ft_read_and_append` для чтения данных из файла и добавления их к `saved_str`.
- Если чтение данных не удалось, функция возвращает `NULL`.
- Использует функцию `ft_getline_fromleftstr` для извлечения следующей строки из `saved_str`.
- Обновляет `saved_str` с помощью функции `ft_update_leftstr`, удаляя прочитанную строку.
- Возвращает прочитанную строку.

Обратите внимание: строка, возвращаемая этой функцией, должна быть освобождена после использования.

### Функция ft_read_and_append :

- Эта функция читает данные из файла, описанного дескриптором fd, и добавляет их к строке saved_str. 
- Это продолжается до тех пор, пока не будет найден символ новой строки ('\n') в saved_str или пока не будет достигнут конец файла. 
- Возвращает обновленное значение saved_str или NULL в случае ошибки.

  *При использовании read для чтения из файла, операционная система "помнит", сколько байтов уже было прочитано из этого файла, и при следующем вызове read продолжает чтение с того места, где остановилась в прошлый раз. Это называется "позицией чтения" и она ассоциирована с файловым дескриптором.*

## [`get_next_line_utils`](get_next_line_utils.c)

### Функция ft_strlen_v :

- определяет и возвращает длину строки, переданной ей в качестве аргумента.

### Функция ft_strchr_v :

- ищет первое вхождение символа c в строке s.

### Функция ft_strjoin_v :

- объединяет две строки (left_str и buff) в одну новую строку.

### Функция ft_getline_fromleftstr :

- извлекает строку из left_str до символа перевода строки (\n) или до конца строки, если символ перевода строки отсутствует.

Описание:

- Функция принимает в качестве аргумента строку left_str.
- Сначала проверяется, не является ли строка пустой. Если это так, функция возвращает NULL.
- Подсчитывается количество символов до символа перевода строки или до конца строки.
- Выделяется память под новую строку line, размер которой равен количеству найденных символов плюс один или два символа (в зависимости от наличия символа перевода строки).
- Символы left_str копируются в line до символа перевода строки или до конца строки.
- Если в left_str присутствует символ перевода строки, он также копируется в line.
- В конце строки line добавляется нуль-терминатор.
- Функция возвращает указатель на новую строку line.
- В итоге, ft_getline_fromleftstr — это функция, которая извлекает строку из входной строки до символа перевода строки или до её конца и возвращает эту строку в виде новой строки.

### Функция ft_update_leftstr :
- обновляет содержимое строки left_str, удаляя её часть до первого символа перевода строки (\n), включая этот символ.

Описание:

- Функция принимает в качестве аргумента строку left_str.
- С помощью цикла определяется позиция первого символа перевода строки или конца строки.
- Если символ перевода строки не найден, освобождается память, выделенная под left_str, и функция возвращает NULL.
- Если символ перевода строки найден, выделяется память под новую строку new_left_str размером с оставшуюся часть left_str после символа перевода строки.
- Оставшаяся часть left_str после символа перевода строки копируется в new_left_str.
- Освобождается память, выделенная под исходную left_str.
- Функция возвращает указатель на новую строку new_left_str.
- В итоге, ft_update_leftstr — это функция, которая обновляет входную строку, удаляя из неё часть до первого символа перевода строки (включая его) и возвращая оставшуюся часть в виде новой строки.
- Если во входной строке нет символа перевода строки, функция освобождает память и возвращает NULL.

### Как пользоваться gdb

Вот базовые шаги для использования gdb:

Компилируйте вашу программу с флагом -g, чтобы добавить информацию для отладки:

>gcc -Wall -Wextra -Werror -g *.c -o get_next_line

Запустите программу в gdb:

>gdb ./get_next_line

В интерактивном режиме gdb запустите вашу программу:

arduino
>(gdb) run

После того, как программа завершится из-за ошибки сегментации, введите следующую команду, чтобы увидеть место, где произошло сегментационное нарушение:

scss
>(gdb) backtrace

Результатом этой команды будет стек вызовов, который покажет вам, в каком месте вашего кода произошло сегментационное нарушение. Это поможет локализовать и исправить проблему.

### Does your function still work if the BUFFER_SIZE value is 9999? If it is 1? 10000000? Do you know why?

    Работает ли ваша функция, если значение BUFFER_SIZE равно 9999?
    Да, функция должна продолжать работать, даже если значение BUFFER_SIZE равно 9999. Это связано с тем, что BUFFER_SIZE представляет собой размер буфера для чтения, и более большое значение просто означает более высокий предел для однократного чтения из файла.

    Работает ли функция, если BUFFER_SIZE равно 1?
    Да, функция должна работать, даже если значение BUFFER_SIZE равно 1. В этом случае каждый символ будет читаться по очереди, и хотя производительность может страдать из-за большого количества вызовов функций чтения, функциональность не будет нарушена.

    Работает ли функция, если BUFFER_SIZE равно 10000000?
    Да, функция будет работать, но такое большое значение BUFFER_SIZE может привести к излишнему использованию памяти. Это может привести к проблемам с производительностью и использованием памяти, особенно при работе с большим количеством файлов или большими файлами.

    
