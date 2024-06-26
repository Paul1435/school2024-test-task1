# Тестовое задание для отбора на Летнюю ИТ-школу КРОК по разработке

## Условие задания
Один развивающийся и перспективный маркетплейс активно растет в настоящее время. Текущая команда разработки вовсю занята тем, что развивает ядро системы. Помимо этого, перед CTO маркетплейса стоит задача — разработать подсистему аналитики, которая на основе накопленных данных формировала бы разнообразные отчеты и статистику.

Вы — компания подрядчик, с которой маркетплейс заключил рамочный договор на выполнение работ по разработке этой подсистемы. В рамках первого этапа вы условились провести работы по прототипированию и определению целевого технологического стека и общих подходов к разработке.

На одном из совещаний с Заказчиком вы определили задачу, на которой будете выполнять работы по прототипированию. В качестве такой задачи была выбрана разработка отчета о периодах наибольших трат со стороны пользователей.

Аналитики со стороны маркетплейса предоставили небольшой срез массива данных (файл format.json) о покупках пользователей, на примере которого вы смогли бы ознакомиться с форматом входных данных. Каждая запись данного среза содержит следующую информацию:
- Идентификатор пользователя;
- Дата и время оформления заказа;
- Статус заказа;
- Сумма заказа.

В пояснительной записке к массиву данных была уточняющая информация относительно статусов заказов:
- COMPLETED (Завершенный заказ);
- CANCELED (Отмененный заказ);
- CREATED (Созданный заказ, еще не оплаченный);
- DELIVERY (Созданный и оплаченный заказ, который доставляется).

Необходимо разработать отчет, вычисляющий по полученному массиву данных месяц, когда пользователи тратили больше всего. Если максимальная сумма пользовательских трат была в более, чем одном месяце, отчет должен показывать все такие месяцы. В отчете должны учитываться только завершенные заказы.

Требования к реализации:
1. Реализация должна содержать, как минимум, одну процедуру (функцию/метод), отвечающую за формирование отчета, и должна быть описана в readme.md в соответствии с чек-листом;
2. В качестве входных данных программа использует json-файл (input.json), соответствующий структуре, описанной в условиях задания;
3. Процедура (функция/метод) формирования отчета должна возвращать строку в формате json следующего формата:
   - {«months»: [«march»]} 
   - {«months»: [«march», «december»]}
4. Найденный в соответствии с условием задачи месяц должен выводиться на английском языке в нижнем регистре. Если месяцев несколько, то на вывод они все подаются на английском языке в нижнем регистре в порядке их следования в течение года.

## Автор решения
   Шестаков Павел Евгеньевич
## Описание реализации
   Данная реализация полностью соответствует требованиям к реализации. Функция, которая ответственна за формирование отчета является String object2Json(List<List<String>> object).
   Данная функция принимает массив массивов строк(каждый массив это год с своими месяцами, где total  = MaxTotal для этого года). Массив массивов предварительно отсортирован в порядке следования месяцев в году.
   Пример:  json, когда у нас два разных года, в одном из которых Total max в двух месяцах, а в другом - один:
   {"months":["march"]}
   {"months":["march", "december"]}
   Также для конкретного создания этого отчета были сделаны два метода findMonthsWithMaxTotal(Map<YearAndMonth, BigDecimal> sumForAllMonths) и getMonthsWithMaxTotal(List<Order> orders).
   Второй метод вызывает первый.  Получив список заказов, второй метод находит сумму total для каждого месяца из предоставленных лет(используется HashMap, которая хранит в качестве ключа год и месяц), а в качестве значения - сумму за этот период. После формирования данного HashMap он передается в первый метод, где для каждого года сначала находим максимальный total(среди месяцев данного года), а потом находим месяца среди этого года, которые соответствуют данным максимумам и сохраняем их в List. Получается, что в первом методе используется для реализации два Hashmap:
   - Map<Integer, BigDecimal> maxTotalForEveryYear = new HashMap<>(); - хранит максимумы для каждого года
   -  Map<Integer, List<String>> maxMonthsForYear = new HashMap<>(); - хранит месяца, которые сответсвуют максимуму каждого года
   Данная реализация производит проверку корректности состояний каждого заказа.
Не валидными являются те заказы, где:
   1) упущены какие-нибудь поля или они null
   2) если дата заказа указана в будущем
   3) total отрицателен

В данной реализации использовалась библиотека  Gson в основном, а также для хранения времени LocalDataTime.   
Валидация просто не пропускает конкретный заказ, если у него не валидное состояние. В консоль укажется ошибка, покажет в каком заказе ошибка и в чем ошибка. 
Более детальное описании реализации классов описано внутри кода с  помощью  javaDoc. 
## Инструкция по сборке и запуску решения
   Важно: должен быть установлен maven.
   1) перейти в папку проекта(где находится pom.xml)
   2) ввести команду "mvn clean compile assembly:single" 
   3) В папке target  появится файл .jar его нужно запустить с аргументом
       (сюда передать путь и название с расширением файла).
       ![img.png](img.png)
   Это можно выполнить командой(Пример):
   java -jar target/crockJsonConverter-1.0-SNAPSHOT-jar-with-dependencies.jar path
