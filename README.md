# nodeschool.io-learnyoubash
nodeschool.io leaernyoubash
https://likegeeks.com/bash-script-easy-guide/
 
  Сценарии командной строки — это наборы тех же самых команд, которые можно вводить с клавиатуры, собранные в файлы и объединённые некоей   общей целью. При этом результаты работы команд могут представлять либо самостоятельную ценность, либо служить входными данными для        других команд. Сценарии — это мощный способ автоматизации часто выполняемых действий.
  
  Итак, если говорить о командной строке, она позволяет выполнить несколько команд за один раз, введя их через точку с запятой:

      pwd ; whoami
      
На самом деле, если вы опробовали это в своём терминале, ваш первый bash-скрипт, в котором задействованы две команды, уже написан. Работает он так. Сначала команда pwd выводит на экран сведения о текущей рабочей директории, потом команда whoamiпоказывает данные о пользователе, под которым вы вошли в систему.

Используя подобный подход, вы можете совмещать сколько угодно команд в одной строке, ограничение — лишь в максимальном количестве аргументов, которое можно передать программе. Определить это ограничение можно с помощью такой команды:

    getconf ARG_MAX

Командная строка — отличный инструмент, но команды в неё приходится вводить каждый раз, когда в них возникает необходимость. Что если записать набор команд в файл и просто вызывать этот файл для их выполнения? Собственно говоря, тот файл, о котором мы говорим, и называется сценарием командной строки.

Как устроены bash-скрипты

Создайте пустой файл с использованием команды touch. В его первой строке нужно указать, какую именно оболочку мы собираемся использовать. Нас интересует bash, поэтому первая строка файла будет такой:

    #!/bin/bash

В других строках этого файла символ решётки используется для обозначения комментариев, которые оболочка не обрабатывает. Однако, первая строка — это особый случай, здесь решётка, за которой следует восклицательный знак (эту последовательность называют шебанг) и путь к bash, указывают системе на то, что сценарий создан именно для bash.

Команды оболочки отделяются знаком перевода строки, комментарии выделяют знаком решётки. Вот как это выглядит:

    #!/bin/bash
    # This is a comment
    pwd
    whoami

Тут, так же, как и в командной строке, можно записывать команды в одной строке, разделяя точкой с запятой. Однако, если писать команды на разных строках, файл легче читать. В любом случае оболочка их обработает.


Установка разрешений для файла сценария

Сохраните файл, дав ему имя myscript, и работа по созданию bash-скрипта почти закончена. Сейчас осталось лишь сделать этот файл исполняемым, иначе, попытавшись его запустить, вы столкнётесь с ошибкой Permission denied.



Попытка запуска файла сценария с неправильно настроенными разрешениями

Сделаем файл исполняемым:

    chmod +x ./myscript

Теперь попытаемся его выполнить:

    ./myscript

После настройки разрешений всё работает как надо.



Успешный запуск bash-скрипта

Вывод сообщений

Для вывода текста в консоль Linux применяется команда echo. Воспользуемся знанием этого факта и отредактируем наш скрипт, добавив пояснения к данным, которые выводят уже имеющиеся в нём команды:

    #!/bin/bash
    # our comment is here
    echo "The current directory is:"
    pwd
    echo "The user logged in is:"
    whoami

Вот что получится после запуска обновлённого скрипта.



Вывод сообщений из скрипта

Теперь мы можем выводить поясняющие надписи, используя команду echo. Если вы не знаете, как отредактировать файл, пользуясь средствами Linux, или раньше не встречались с командой echo, взгляните на этот материал.
Использование переменных

Переменные позволяют хранить в файле сценария информацию, например — результаты работы команд для использования их другими командами.

Нет ничего плохого в исполнении отдельных команд без хранения результатов их работы, но возможности такого подхода весьма ограничены.

Существуют два типа переменных, которые можно использовать в bash-скриптах:

Переменные среды
Пользовательские переменные

Переменные среды

Иногда в командах оболочки нужно работать с некими системными данными. Вот, например, как вывести домашнюю директорию текущего пользователя:

    #!/bin/bash
    # display user home
    echo "Home for the current user is: $HOME"

Обратите внимание на то, что мы можем использовать системную переменную $HOME в двойных кавычках, это не помешает системе её распознать. Вот что получится, если выполнить вышеприведённый сценарий.



Использование переменной среды в сценарии

А что если надо вывести на экран значок доллара? Попробуем так:

    echo "I have $1 in my pocket"

Система обнаружит знак доллара в строке, ограниченной кавычками, и решит, что мы сослались на переменную. Скрипт попытается вывести на экран значение неопределённой переменной $1. Это не то, что нам нужно. Что делать?

В подобной ситуации поможет использование управляющего символа, обратной косой черты, перед знаком доллара:

    echo "I have \$1 in my pocket"

Теперь сценарий выведет именно то, что ожидается.



Использование управляющей последовательности для вывода знака доллара

Пользовательские переменные

В дополнение к переменным среды, bash-скрипты позволяют задавать и использовать в сценарии собственные переменные. Подобные переменные хранят значение до тех пор, пока не завершится выполнение сценария.

Как и в случае с системными переменными, к пользовательским переменным можно обращаться, используя знак доллара:

    #!/bin/bash
    # testing variables
    grade=5
    person="Adam"
    echo "$person is a good boy, he is in grade $grade"

Вот что получится после запуска такого сценария.



Пользовательские переменные в сценарии

Подстановка команд

Одна из самых полезных возможностей bash-скриптов — это возможность извлекать информацию из вывода команд и назначать её переменным, что позволяет использовать эту информацию где угодно в файле сценария.

Сделать это можно двумя способами.

С помощью значка обратного апострофа «`»
С помощью конструкции $()

Используя первый подход, проследите за тем, чтобы вместо обратного апострофа не ввести одиночную кавычку. Команду нужно заключить в два таких значка:

      mydir=`pwd`

При втором подходе то же самое записывают так:

      mydir=$(pwd)

А скрипт, в итоге, может выглядеть так:

      #!/bin/bash
      mydir=$(pwd)
      echo $mydir

В ходе его работы вывод команды pwdбудет сохранён в переменной mydir, содержимое которой, с помощью команды echo, попадёт в консоль.


Математические операции

Для выполнения математических операций в файле скрипта можно использовать конструкцию вида $((a+b)):

#!/bin/bash
var1=$(( 5 + 5 ))
echo $var1
var2=$(( $var1 * 2 ))
echo $var2



Математические операции в сценарии

Управляющая конструкция if-then

В некоторых сценариях требуется управлять потоком исполнения команд. Например, если некое значение больше пяти, нужно выполнить одно действие, в противном случае — другое. Подобное применимо в очень многих ситуациях, и здесь нам поможет управляющая конструкция if-then. В наиболее простом виде она выглядит так:

if команда
then
команды
fi

А вот рабочий пример:

#!/bin/bash
if pwd
then
echo "It works"
fi

В данном случае, если выполнение команды pwdзавершится успешно, в консоль будет выведен текст «it works».

Воспользуемся имеющимися у нас знаниями и напишем более сложный сценарий. Скажем, надо найти некоего пользователя в /etc/passwd, и если найти его удалось, сообщить о том, что он существует.

#!/bin/bash
user=likegeeks
if grep $user /etc/passwd
then
echo "The user $user Exists"
fi

Вот что получается после запуска этого скрипта.



Поиск пользователя

Здесь мы воспользовались командой grepдля поиска пользователя в файле /etc/passwd. Если команда grepвам незнакома, её описание можно найти здесь.

В этом примере, если пользователь найден, скрипт выведет соответствующее сообщение. А если найти пользователя не удалось? В данном случае скрипт просто завершит выполнение, ничего нам не сообщив. Хотелось бы, чтобы он сказал нам и об этом, поэтому усовершенствуем код.

Управляющая конструкция if-then-else

Для того, чтобы программа смогла сообщить и о результатах успешного поиска, и о неудаче, воспользуемся конструкцией if-then-else. Вот как она устроена:

if команда
then
команды
else
команды
fi

Если первая команда возвратит ноль, что означает её успешное выполнение, условие окажется истинным и выполнение не пойдёт по ветке else. В противном случае, если будет возвращено что-то, отличающееся от нуля, что будет означать неудачу, или ложный результат, будут выполнены команды, расположенные после else.

Напишем такой скрипт:

#!/bin/bash
user=anotherUser
if grep $user /etc/passwd
then
echo "The user $user Exists"
else
echo "The user $user doesn’t exist"
fi

Его исполнение пошло по ветке else.



Запуск скрипта с конструкцией if-then-else

Ну что же, продолжаем двигаться дальше и зададимся вопросом о более сложных условиях. Что если надо проверить не одно условие, а несколько? Например, если нужный пользователь найден, надо вывести одно сообщение, если выполняется ещё какое-то условие — ещё одно сообщение, и так далее. В подобной ситуации нам помогут вложенные условия. Выглядит это так:

if команда1
then
команды
elif команда2
then
команды
fi

Если первая команда вернёт ноль, что говорит о её успешном выполнении, выполнятся команды в первом блоке then, иначе, если первое условие окажется ложным, и если вторая команда вернёт ноль, выполнится второй блок кода.

#!/bin/bash
user=anotherUser
if grep $user /etc/passwd
then
echo "The user $user Exists"
elif ls /home
then
echo "The user doesn’t exist but anyway there is a directory under /home"
fi

В подобном скрипте можно, например, создавать нового пользователя с помощью команды useradd, если поиск не дал результатов, или делать ещё что-нибудь полезное.

Сравнение чисел

В скриптах можно сравнивать числовые значения. Ниже приведён список соответствующих команд.

n1 -eq n2Возвращает истинное значение, если n1 равно n2.
n1 -ge n2 Возвращает истинное значение, если n1больше или равно n2.
n1 -gt n2Возвращает истинное значение, если n1 больше n2.
n1 -le n2Возвращает истинное значение, если n1меньше или равно n2.
n1 -lt n2Возвращает истинное значение, если n1 меньше n2.
n1 -ne n2Возвращает истинное значение, если n1не равно n2.

В качестве примера опробуем один из операторов сравнения. Обратите внимание на то, что выражение заключено в квадратные скобки.

    #!/bin/bash
    val1=6
    if [ $val1 -gt 5 ]
    then
    echo "The test value $val1 is greater than 5"
    else
    echo "The test value $val1 is not greater than 5"
    fi

Вот что выведет эта команда.



Сравнение чисел в скриптах

Значение переменной val1больше чем 5, в итоге выполняется ветвь thenоператора сравнения и в консоль выводится соответствующее сообщение.


