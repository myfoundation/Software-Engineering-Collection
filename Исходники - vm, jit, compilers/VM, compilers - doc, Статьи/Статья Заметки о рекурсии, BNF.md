---
created: 2022-12-23T07:40:47 (UTC +03:00)
tags: [статья,]
source: https://club.shelek.ru/viewart.php?id=184
author: 
---

# Статья: "Заметки о рекурсии"

> ## Excerpt
> Клуб программистов "Весельчак У". Статья:  "Заметки о рекурсии"

---
Программисты вкладывают в это понятие следующий смысл: рекурсия - это прием программирования, при котором программа вызывает саму себя либо непосредственно, либо косвенно.

Обычно начинающий программист, впервые услышавший про рекурсию, впадает в легкое замешательство. Казалось бы, нет ничего более бессмысленного, чем рекурсия. Подобная цепочка вызовов никогда не завершится из-за зацикливания, или в лучшем случае завершится, но аварийно, когда задача исчерпает все наличные ресурсы компьютера.

На самом деле рекурсия - это тонкий и изящный инструмент, который при умелом использовании способен сослужить добрую службу программисту.

Действительно, почему именно рекурсии выпала честь удостоиться отдельной статьи? Разве это не рядовой прием программирования, один из многих?

Лично я выделяю рекурсию из общего ряда по многим причинам. Вот некоторые из них:

-   Прежде всего, рекурсия красива. Первоначальное знакомство с ней может вызвать непонимание и легкий шок, но, овладев этим механизмом, вы вряд ли в дальнейшем сможете избежать соблазна пользоваться им время от времени. Изящество рекурсии в программировании я могу сравнить только с изяществом метода индукции в математике.
-   Многие структуры, как искусственного, так и естественного происхождения, рекурсивны по самой своей сути. Рассмотрите ветви и корни деревьев, структуру сложной организации, иерархию классов в сложной программе, фракталы - в любой из этих структур вы найдете многочисленные повторения целого в части. Таким образом, многие сущности естественным образом могут быть смоделированы рекурсивными структурами. А чем же еще обрабатывать рекурсивную структуру, как не рекурсивной процедурой?
-   Немало задач имеет также рекурсивную природу. Даже, пожалуй, большинство нетривиальных задач. По крайней мере, в программировании далеко не нов метод декомпозиции задачи на несколько более простых, те, в свою очередь, разделяются на еще более простые, и так до тех пор, пока не дойдем до элементарных частей, решить которые достаточно просто. Естественно, программисты не являются монополистами в этой области: подобными методами пользуются архитекторы, машиностроители, математики

Несмотря на все эти достоинства, рекурсия отнюдь не спешит прописаться в арсенале средств многих программистов, даже профессиональных (под профессионализмом в данный момент я подразумеваю отнюдь не степень мастерства, а факт, что именно программирование дает данному человеку средства к существованию), имеющих о ней самое смутное представление.

Любое незнание обычно порождает массу предрассудков. Например, шаман племени людоедов, не имея представления об атмосферном электричестве, объясняет благодарным соплеменникам, что гроза - это проявление немилости богов. И его слова принимаются за истину в первой инстанции, ибо природа не терпит пустоты, - там, где нет знания, поселяется суеверие.

Рекурсия также обросла слухами и суевериями. Вы можете услышать от компьютерных шаманов, что она:

-   безмерно расточительно относится к памяти (забавно, но я слышал эти доводы много лет назад, программируя на мини-ЭВМ PDP-11/40, которая "щедро" отводила на пользовательскую задачу около 50Кбайт; периодически меня пытаются запугать этим и сейчас, когда объем ОЗУ в 1 Гбайт не является чем-то из ряда вон выходящим);
-   весьма накладна по ресурсам процессора; при этом факт, что производительность микропроцессоров Intel за минувшие 10 лет возросла как минимум в 100 раз (если судить только по тактовой частоте, не учитывая прогресс архитектуры), как-то совершенно не принимается во внимание;
-   наконец, сложна в реализации и отладке, что влечет за собой массу ошибок в программе; то, что ошибки в задании условий для циклов с пред- и постусловиями, а также начальных и конечных значений в циклах с перебором значений обычно изобилуют в типичной нерекурсивной программе, конфузливо замалчивается.

Итак, попробуем разобраться, что же представляет собой рекурсия на самом деле.

По сложившейся традиции для первого знакомства с рекурсией почти все авторы, не сговариваясь, приводят два примера: вычисление факториала и поиск чисел Фибоначчи. Должен заметить, что с точки зрения практикующего программиста рекурсивное вычисление факториала весьма уступает обычному итерационному способу. Лично убедившись в этом, новичок обычно на этом и заканчивает знакомство с рекурсией, предпочитая потратить время на изучение более практичных вещей.

Не будем отступать от традиции и мы. При этом, разумеется, примем вышесказанное к сведению и постараемся воспринять следующий пример просто как иллюстрацию к простейшему применению метода, а вовсе не как призыв весь остаток жизни вычислять факториалы именно таким образом.

Итак, вспомним, что такое факториал. Обозначается факториал восклицательным знаком "!" и вычисляется следующим образом:

N! = 1 \* 2 \* 3 \*  \* (N-1) \* N,   _**(1)**_

то есть представляет собой произведение натуральных чисел от 1 до N включительно.

На эту функцию можно посмотреть и под другим углом зрения. Внимательнее изучив формулу _**(1)**_, мы можем прийти к выводу, что

N! = N \* (N-1)!   _**(2)**_

Таким образом, мы определили факториал через сам факториал! Казалось бы, такое определение совершенно бесполезно, так как неизвестное понятие определяется через опять же неизвестное понятие, и мы приходим к бесконечному циклу. Однако это впечатление обманчиво, потому что на самом деле факториал "N" определяется через факториал (N-1), то есть нахождение неизвестной функции сводится к вычислению той же неизвестной функции от другого аргумента, на единицу меньшего, чем исходный. Таким образом, есть надежда, что каждая следующая задача будет решаться чуть легче предыдущей.

Окончательно разорвать замкнутый круг мы сможем, если дополним рекурсивное определение _**(2)**_ еще одним, которое служит чем-то вроде тупика, предназначенного для остановки процесса:

1! = 1   _**(3)**_

(На самом деле следовало бы оговорить поведение факториала в нуле и при отрицательных значениях аргумента, но, поскольку в данный момент мы знакомимся с рекурсией, а не создаем коммерческую библиотеку математических функций, опустим несущественные детали).

Правила _**(2)**_ и _**(3)**_ совместно позволяют вычислить значение факториала от любого аргумента. Попробуем для примера вычислить значение 5!, несколько раз применив правило _**(2)**_ и однократно  правило _**(3)**_:

5! = 5 \* 4! = 5 \* 4 \* 3! = 5 \* 4 \* 3 \* 2! = 5 \* 4 \* 3 \* 2 \* 1! = 5 \* 4 \* 3 \* 2 \* 1

Мы получили результат, который совпадает с определением _**(1)**_ с точностью до перестановки сомножителей. (И самой перестановки в принципе можно было бы избежать, если перейти в _**(1)**_ от правосторонней рекурсии к левосторонней, но эти разновидности мы обсудим позже).

Как же выглядит рекурсия на практике? Попробуем реализовать рекурсивное вычисление факториала в виде функции на C:

long int Fact(long int N)  
{  
  if (N < 1) return 0;  
  else if (N == 1) return 1;  
  else return N \* Fact(N-1);  
}  

Как видим, реализация функции ничем принципиально не отличается от рекурсивного определения факториала.

На этом рекомендую сделать некоторую паузу, скопировать текст функции **Fact** в вашу любимую систему разработки на языке C и несколько раз прогнать ее под отладчиком в пошаговом режиме; если вы владеете отладчиком на уровне работы с регистрами процессора и областями оперативной памяти, рекомендую понаблюдать также за метаморфозами, происходящими в стеке.

Большинство из тех, кто прочитал все вышеописанное, наверняка уже задались вопросом: а зачем все это нужно? Ведь первоначальное определение факториала ничуть не хуже, выглядит гораздо привычнее, да и реализовать его в общем-то не сложнее:

long int Fact2(long int N)  
{  
long int F = 1;  
for (long int i=2; i<=N; i++)  
F \*= i;  
return F;  
}  

Такая реализация наверняка покажется гораздо более естественной почти всем программистам. Исключение, пожалуй, составит немногочисленная братия, программирующая на языках Lisp и Prolog, для которых рекурсивное мышление стало привычным ввиду особенностей применяемых инструментов.

На самом деле, разбираясь в столь детских по уровню примерах, мы затронули гораздо более глубинное и серьезное явление, а именно - эквивалентность рекурсии и итерации.

В теоретическом программировании существует теорема, которая гласит, что рекурсия и итерация эквивалентны. Это значит, что любой алгоритм, который можно реализовать рекурсивно, с таким же успехом может быть реализован и итеративно, и наоборот. Тем, кто знаком с теорией формальных языков, теорией конечных автоматов и подобными дисциплинами, она наверняка хорошо известна. Остальным придется поверить мне на слово, тем более что мы только что убедились в ее истинности на простейшем примере.

Следует ли из этого, что мы попусту потратили время, я - на написание данной статьи, вы - на ее прочтение? Не означает ли вышесказанное, что программисту достаточно владеть только приемами итерации для решения любых задач, поскольку она эквивалентна рекурсии?

Тут нам придется признать как факт, что теоретики программирования привыкли манипулировать довольно абстрактными понятиями: вместо компьютера они используют машины Тьюринга, машины Поста и подобные сооружения, которые вы вряд ли приобретете в ближайшем компьютерном салоне. Соответственно, вместо программы у них - алгоритм (строго говоря, они не эквивалентны, ибо далеко не каждая программа является алгоритмом). При этом в центре внимания в первую очередь находится корректность алгоритма, об эффективности особенно не пекутся, ибо все, что требуется от алгоритма, - это завершиться за конечное время, а уж насколько оно велико, теоретиков мало волнует, - вычисления на машине Тьюринга все равно обходятся им даром.

Мы же с вами живем в реальном мире, который далеко не столь идеален: наши компьютеры обладают конечными ресурсами, за ограничения которых нам не выйти, а потребители наших программ - конечным терпением, границы которого переходить тоже чревато. Поэтому мы вынуждены обращать внимание на такие низменные материи, как эффективность вычислений. А обратив на нее более пристальное внимание, мы будем неприятно поражены фактом: несмотря на якобы обещанную эквивалентность, рекурсивный вариант вычисляет факториал куда медленнее, чем его итеративный собрат! Более того, и памяти при работе он потребляет куда больше.

Итак, оставим рекурсию теоретикам, а сами прекрасно обойдемся и без нее? Именно к такому выводу приходят многие после изучения соответствующего раздела учебника программирования, в котором красуются все те же рекурсивные факториалы и числа Фибоначчи. Но не будем спешить с выводами. На самом деле есть очень много примеров, подобных вышеприведенному, в которых применение рекурсии просто расточительно и не выдерживает никакой конкуренции с итерацией. Однако существует немало и противоположных примеров, для которых рекурсивное решение просто и элегантно, а итеративное  громоздко и неестественно. Задача программиста  не только овладеть обоими подходами, но и научиться выбирать, какой из них применить в данном случае. Лично я, к сожалению, не могу предложить вам на этот счет готового рецепта; интуиция и чувство меры  вот на что я обычно полагаюсь в подобных ситуациях.

Итак, до сих пор мы рассматривали рекурсию на примере, рекурсивная реализация которого на практике малополезна. Теперь, осознав основные идеи, лежащие в основе рекурсии, приступим к рассмотрению задач, для решения которых она действительно полезна.

В общем случае наилучшее применение рекурсии - это решение задач, для которых свойственна одна черта: решение задачи в целом сводится к решению подобной же задачи, но меньшей размерности и, следовательно, легче решаемой.

Поскольку эта фраза звучит довольно загадочно и абстрактно, постараюсь проиллюстрировать ее примером подобной задачи. Хрестоматийный вариант - игра Ханойские башни.

Легенда гласит, что где-то в Ханое находится храм, в котором размещена следующая конструкция: на основании укреплены 3 алмазных стержня, на которые при сотворении мира Брахма нанизал 64 золотых диска с отверстием посередине,  причем внизу оказался самый большой диск, на нем  чуть меньший и так далее, пока на верхушке пирамиды не оказался самый маленький диск. Жрецы храма обязаны перекладывать диски по следующим правилам:

-   За один ход можно перенести только один диск.
-   Нельзя класть больший диск на меньший.

Руководствуясь этими нехитрыми правилами, жрецы должны перенести исходную пирамиду с 1-го стержня на 3-й. Как только они справятся с этим заданием, наступит конец света.

Попытаемся и мы внести свою лепту в этот процесс. Сначала попытаемся построить абстрактную модель процесса переноса части пирамиды (когда мы с ней справимся, станет яснее, как перенести пирамиду целиком).

Итак, решаем обобщенную задачу: как перенести пирамиду из **n** колец со стержня **i** на стержень **j**, пользуясь стержнем **k** как вспомогательным? Эта задача решается следующим образом:

-   Перенести (n-1) кольцо с i на k.
-   Перенести 1 кольцо с i на j.
-   Перенести (n-1) кольцо с k на j.

Выглядит уже немного знакомо, не правда ли? Задача переноса **n** колец решается через перенос **(n-1)** кольца. Осталось только добавить тривиальное граничное условие для вырожденного случая переноса пустой пирамиды из 0 колец, чтобы наш алгоритм завершился, а не отправился в минус бесконечность.

Реализуем алгоритм на любезном многим языке C:

void TowerMove(// перенос пирамиды  
   int n, // из n дисков  
   short i, // со стержня i  
   short j, // на стержень j,  
   short k  // используя стержнень k как вспомогательный  
   )  
{  
// проверка граничного условия  пустую пирамиду не перемещаем  
if (!n)  
return;  
TowerMove(n-1, i, k, j);  
// переносим одно кольцо  фактически это TowerMove(1, i, j, k);  
printf("%2d - %2d\\n", i, j);  
TowerMove(n-1, k, j, i);  
}  

Функция **TowerMove** фактически способна переместить пирамиду из любого количества колец, в том числе и всю исходную. Для решения задачи в целом нам остается всего лишь дописать главную функцию:

int main(int argc, char\* argv\[\])  
{  
TowerMove(5, 1, 3, 2);  
return 0;  
}  

Как нетрудно догадаться, в данном случае решается задача переноса пирамиды из 5 колец с 1-го стержня на 3-й.

Собственно, предпосылкой для написания данной статьи явился наш диалог с Dimyan'ом (полностью увидеть его можно [здесь](https://forum.shelek.ru/index.php/topic,2871.0.html%22)). Для тех, кто не намерен перечитывать его целиком и вникать в детали, краткий пересказ проблемы.

Имеется некая иерархическая структура, хранимая в реляционной базе данных. (Вообще хранение иерархических структур в реляционной базе данных  это отдельная, достаточно интересная сама по себе тема, но она не имеет непосредственного отношения к данной статье; возможно, мы к ней вернемся в одной из статей по базам данных, а пока примем на веру, что это вполне возможно).

Необходимо визуально представить ту структуру в виде классического отображения дерева подобно тому, как это сделано в Проводнике MS Windows.

Древовидная структура - яркий пример рекурсивной структуры. Каждый узел дерева может быть представлен как корень дерева меньшего размера. Значит, отображение каждого узла подобно отображению дерева в целом. А из этого следует, что рекурсивная процедура в данном случае вполне уместна.

Заранее прошу прощения у приверженцев C, но очередной пример будет представлен на языке C#. Причина проста  программа должна работать с базой данных, а реализация подобных программ как на C, так и на C++  занятие не для слабонервных. Разумеется, детали работы с базой данных я здесь приводить не буду. Исходный текст процедуры приведен полностью на форуме (см. ссылку выше), а весь тестовый проект, если у кого-то возникнет желание воспроизвести его на своем компьютере, я могу выслать почтой по запросу.

    private void FillBranch(TreeNode tn)  
    {  
      // получить список дочерних узлов для tn  
      // выбираем из базы данных все вершины, у которых значение поля ParentID  
      //  совпадает со значением тега текущего узла tn  
.  
.  
.  
      // строим список узлов  
      ArrayList nodeList = new ArrayList();  
.  
.  
.  
     // все дочерниу узлы для tn занесены в nodeList  
     // добавить все дочерние узлы к текущему  
      foreach (TreeNode nn in nodeList)  
      {  
        tn.Nodes.Add(nn);  
        FillBranch(nn);  
      } // foreach  
    }  

Как видите, основная идея остается неизменной. Для того, чтобы построить ветку дерева с корнем в узле **tn**, нужно:

-   найти все дочерние узлы для tn;
-   добавить их к дереву;
-   построить дерево с корнем в каждом из дочерних узлов.

Как я уже говорил ранее, для рекурсивных процедур весьма важно, чтобы на каком-то уровне вложенности дальнейшие вызовы прекращались, в противном случае произойдет зацикливание до тех пор, пока пространство стека не будет исчерпано (в стеке хранятся формальные параметры процедуры, локальные переменные с автоматическим размещением, а также некоторая служебная информация, необходимая для корректного возврата из процедуры). Чаще всего для этого используется условный оператор (как в случае с факториалом или Ханойскими башнями). Но в нашем примере условный оператор отсутствует. Однако зацикливания не происходит, т.к. его роль неявно выполняет оператор цикла **foreach**: когда мы добираемся до листовых вершин дерева, которые не имеют дочерних, список **nodeList** станет пустым и цикл не выполнится, предотвратив таким образом очередные рекурсивные вызовы процедуры. С этого момента начнется обратная раскрутка цепочки рекурсивных вызовов, пока не дойдет до первоначального вызова для корня всего дерева.

Результат работы процедуры показан на рисунке:

![](data:image/gif;base64,R0lGODlhGAJXAfcAAP//////9/f3//f39/f37+/3/+/39//v5u/v//fv5u/v9+bv/+/v5u/v3v/g2N7m/+bm5tbm////ANvj99ri69be/87e/8Xe//jOwNzX2c7W/8XW/8XW9/fFvb3W/8XW5vfFtLXW/73O//e9tb3O9/e9rb3O77XO/7/O3rXO963O/63O97XF98XFxa3F963F5rLF1q291qW91qW11rW1pZaz/wD//5y13q2t1u+ahZy11oy1/5S13oyt/62tnISq93ut/++Mc4+n3n2j/++Ea4Sl1u+EY3ul1u97Y3Oc/+97Wnuc1u9zWmOc94yQwWuU/+ZzWuZzUmOU/2OU92OU7+ZrUmOM/+ZrSu9oO2OM9+ZrQlqM9+ZjSuZjQuZjOjqU/1qE9zGU/1KE9+ZaQuZaOimU/+9YJOZaMUqE9+ZaKTGM/0KE91J79+ZSMUp79+ZSKeZSISGM/+ZSGWtztUJ7995SKdtSMe9NE4SEAOZKKTp79+ZKISGE/+ZKGUJz995KKfw5MjF79xmE/yl7/zpz99VKKSl799ZKISF7/zFz93NrY+ZBCDpr9ylz99lBGRB7/9FCITFr9yFz99ZCEP8A/xlz/ylr9xlz9xBz/943DrVGLSFr9zFrxdU6CjFj5ilj9xlr9ylj5gBz/yFj90pjhBBr9wxr/xBr7yFj5ghr9wCEhCpd3hlj5gBr/9MvACla5gBr9yFa5ghj9xla8603GQBj/y1WxRla5gBj97UxEABj77UxCBBa5q0xEABa/w5V8Qha5gBa9wBa7xBS5gBa5rUnAgBa3gBS9whS3gBS7wBS5gBS3qsjAhlKvQBK9wBK7xBKvQBK5ghG3gBK3gBK1gBKzgBC7xBCvQBC5gBC3ghCxQBC1ghCvQBCzgBCxQA67wA65gA63gA61gg6vQA6zgA6xQgx3gA6vQAx3gAx1ggzsgAxzgAxxQAp2QApzgAptwAhzgAd1oQAAAAhrQAhpQAZzgAbvQAYrQAZpQgZhAAA/wAQpQARlgAQjAAIewAAACwAAAAAGAJXAUAI/gAbCBRYrSC1gweXLSPGkJiuhhCFSZxIsaLFixgzatzIsaPHjyBDihxJsqTJkyhBQlzJsqXLlzBjypxJs6bNmzE1JkumLFk0ZcqiJXsWrajRaES3DRR4jx25bgqNMdRFtapVl0Czat3KtavXr2DDih1LtqzZs2jTql3Ltq3bswrjyp1Lt67du3jz6t3LVy6AJD1q/Ejy5MmUKVkSb1kMRowYN34ixbK1QOHfwIMLH06cZfGWxo8jT16gbBmAIDlSByHCGolrKLBhV6lypYsdSJoWkGPH7h09f/bIiZu2TFkLeZSSy2uBB4CE58/xtICoDID1fF5TLLC+gIRW6wBS/hQA8KD6AxICABQgUWO7AA5brVMAj/2t/fv48+vfz7+///8AZgUAIWAw5hgbbtBBByGJJNKIJZtEuIkloTQDAFADFvgZAGIgqCCDDkIIQIQUWhgUAGTEBkUUUXQA3gHglQAHHHvsUUchtAAgjjjswPNbPOEIRVULRBZp5JEt4KKkkvQt6eR24FmnQD7I4NLkkk1meZ2TTG7J5ZdghinmmGSWaeaZaKap5ppstunmm3DGKeecdFo5CyN+uBHlnoEEYogkklxSSinAcAMAk3fmqeeejAIgKKGGBhMMAI6Q0cUVDljXgRaceuHpGSVYB8Idi3RSDABEYQMOO/zUE044/9M81c2sCE1j6zI/AVWcVkf16uuvwAYr7LDEFmvsscgmq+yyzDbr7LPQPmvrtNRWa+212Gar7bbcduvtt+CGe20012Zj7qvhnGvuutnsuGM64qQTzqr91GPOKcgspe++/Pbr778AByzwwAQXbPDBCCes8MIMN+zwwxBHLPHEFFe87zPhzFPvvfla7PHHIIcs8sgkl2zyySinnDDG9GyMb8E+KOLDzDTT3CgA/1iX88089+zzz0AHLfTQRBdt9NFID80AABxUYIEFG0S9gQhUU00CCQDAsDTMiugc5c5Jgwd22DyPfXPOZpOt9tpsF701wSy73PHAMddsN9JpGz123v55t+3334AHHvTSLEhtuAcnJH5CCiwAoMPbdHct+OSUV2555ZALHLe9LxMcs9F213z215eXbvrppy+9gAUctH716ykw7sIEAECQecCfo6777rxbfjvAm3PMNeihzzx036MHjnzvzOu+dAobaGC41FSfIALWjwMwfPPcd+890L//G3znkYftQ9DFG5/03t+37z6jz09vePUnYB++v7m/r//+qN/f7zPZaBnn5iaw/B3tfEBLHwL5x8AGCo1w8pOaBxDHOMf5j18GdKAGN5i0C14sHAIUnuckh7QFCs2EPVNgo8y2vJ6xT2wcjCEA4hfBqYkg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)

Еще один класс задач, которые целесообразно решать рекурсивными методами, - это задачи, для решения которых не существует определенного алгоритма. В этом случае приходится искать решение методом проб и ошибок, подобно поиску выхода из лабиринта. Кстати, аналогия тут прямая, поскольку, блуждая в лабиринте, мы регулярно забредаем в тупик и вынуждены возвращаться обратно и искать другой путь. При этом, если мы не хотим кружить в нем вечно, нам следует помечать уже обследованные ветви, чтобы не возвращаться к ним вновь.

Итак, уже накатанная последовательность действий: обследовать лабиринт - значит обследовать каждую из его ветвей. Дойдя до развилки, повторять алгоритм рекурсивно, возвращаясь из тупиковых ветвей. Если лабиринт не содержит циклов, рано или поздно вы найдете путь на волю. Если содержит, алгоритм немного усложняется, но все рано приводит к цели.

Если желаете поупражняться в рекурсии, попробуйте самостоятельно составить программу, находящую выход из ациклического лабиринта.

Реальные задачи, конечно, обычно посложнее, чем просто хождение по лабиринту. Однако в целом поиск решения практически такой же: мы делаем очередной шаг в поиске решения, фиксируем его и пытаемся на его основе сделать следующий. Не найдя решение, мы должны попробовать другой допустимый шаг. Если все возможные шаги уже перебраны, а решение так и не найдено, следует отступить на шаг назад и повторить процедуру поиска. Подобная стратегия применима, например, в логических играх, где игровое пространство и множество допустимых ходов ограничено правилами игры.

Попробуем и мы решить простенькую задачу. Условие ее таково. Дана квадратная доска, разделенная на клетки, размером n\*n клеток наподобие шахматной. В одном из ее углов расположен шахматный конь. Требуется ходом коня пройтись по всей доске, побывав на каждом поле ровно по одному разу.

Прежде всего нам потребуется доска. Сделаем ее классом и поместим его интерфейс в файл **Board.h**:

/\*  
описание шахматной доски, по которой будет перемещаться конь  
\*/

const int MaxLen = 10; // максимальный размер доски

class CBoard   
{  
private:  
  unsigned short m\_Size;                         // актуальный размер доски  
  unsigned short m\_Cell\[MaxLen\]\[MaxLen\];         // собственно ячейки доски - каждая клетка содержит номер хода  
public:  
  CBoard(unsigned short n);  
  virtual ~CBoard();  
  bool CheckFree(short i, short j);              // проверка клетки i, j на предмет возможности хода  
  void Move(short x, short y, unsigned short n); // пометить клетку i, j как занятую ходом n  
  void MoveBack(short x, short y);               // пометить клетку i, j как свободную  
  void Dump(void);                               // вывод доски на печать  
  unsigned short CellsCount(void);               // кол-во клеток на доске  
};

Функциональность класса доски довольно нехитрая. Кроме создания и уничтожения объекта, доска может сообщить о состоянии отдельной клетки (свободно/занято), пометить клетку как занятую,  узнать количество клеток на доске, а также вывести на консоль состояние всех клеток доски.

Реализацию класса поместим в файл **Board.cpp**:

#include "Board.h"  
#include <stdio.h>  
#include <stdlib.h>

CBoard::CBoard(unsigned short n): m\_Size(n)  
{  
  if (n <= MaxLen)  
  {  
    for (unsigned short i=0; i<n; i++)  
      for (unsigned short j=0; j<n; j++)  
        m\_Cell\[i\]\[j\] = 0;  
  }  
  else  
  {  
    printf("Board size is too big!\\n");  
    exit(1);  
  }  
}

CBoard::~CBoard()  
{  
}

bool CBoard::CheckFree(short i, short j)  
{  
  return  
    (  (i >= 0)  
    && (j >= 0)  
    && (i < m\_Size)  
    && (j < m\_Size)  
    && (0 == m\_Cell\[i\]\[j\])  
    );  
}

void CBoard::Move(short x, short y, unsigned short n)  
{  
    m\_Cell\[x\]\[y\] = n;  
}

void CBoard::MoveBack(short x, short y)  
{  
  m\_Cell\[x\]\[y\] = 0;  
}

void CBoard::Dump(void)  
{  
  for (unsigned short i=0; i<m\_Size; i++)  
  {  
    for (unsigned short j=0; j<m\_Size; j++)  
      printf("  %3d", m\_Cell\[i\]\[j\]);  
    printf("\\n\\n");  
  }  
  printf ("\\n");  
}

unsigned short CBoard::CellsCount(void)  
{  
  return m\_Size \* m\_Size;  
}

Думаю, что реализация доски не должна вызвать никаких вопросов. Конечно же, функцию подсчета клеток на доске можно сделать и поэффективнее, вычислив произведение один раз в момент создания объекта, а не перемножая одно и то же при каждом обращении к функции. Но все же в фокусе нашего внимания сегодня рекурсия, а не оптимизация шахматных досок, поэтому оставим ее в первозданном виде.

Теперь наступил черед реализации коня. Интерфейс класса тоже не отличается сложностью (файл **Knight.h**):

#include "Board.h"

// шахматный конь  
class CKnight   
{  
private:  
  CBoard \*m\_pBoard;  
  short MoveX(unsigned short i);  
  short MoveY(unsigned short i);  
public:  
    CKnight(CBoard \*b);  
  virtual ~CKnight();  
  // сделать ход n на клетку x, y  
  void Move(short x, short y, unsigned short n);  
  // отменить ход  
  void MoveBack(short x, short y);  
  // попытка сделать ход n на клетку x, y  
  bool Try(short x, short y, unsigned short n);  
};

Реализация класса располагается в файле **Knight.cpp**:

#include "Knight.h"  
#include <stdio.h>

CKnight::CKnight(CBoard \*b): m\_pBoard(b)  
{  
}

CKnight::~CKnight()  
{  
  m\_pBoard = 0;  
}

void CKnight::Move(short x, short y, unsigned short n)  
{  
  m\_pBoard->Move(x, y, n);  
}

void CKnight::MoveBack(short x, short y)  
{  
  m\_pBoard->MoveBack(x, y);  
}

// попытка сделать ход n на клетку (x,y)  
bool CKnight::Try(short x, short y, unsigned short n)  
{  
    Move(x, y, n);  
// все возможные ходы сделаны - успех  
  if (n == m\_pBoard->CellsCount())  
    return true;  
  // попытаться сделать следующий ход  
  bool res = false;  
  for (unsigned short i=0; i<8; i++)  
  {  
    // сгенерировать след. ход  
    short x1 = x + MoveX(i);  
    short y1 = y + MoveY(i);  
    // Проверить на допустимость  
    if (m\_pBoard->CheckFree(x1, y1))  
    {  
      // попытаться его сделать  
      res = Try(x1, y1, n+1);  
      // если ход успешный, прекратить дальнейший перебор  
      if (res)  
        break;  
      // неудачный ход - стереть его  
      MoveBack(x1, y1);  
    }  
  }  
  return res;  
} // Try

short CKnight::MoveX(unsigned short i)  
{  
  static short x\[\] = {-1, 1, 2, 2, 1, -1, -2, -2};  
  return x\[i\];  
}

short CKnight::MoveY(unsigned short i)  
{  
  static short y\[\] = {2, 2, 1, -1, -2, -2, -1, 1};  
  return y\[i\];  
}

Методы **Move** и **Moveback** тривиальны и просто делегируют соответствующие методы класса **CBoard**.

Методы **MoveX** и **MoveY**, возможно, нуждаются в некоторых комментариях. Они реализуют правила хода коня, точнее, выдают смещения относительно текущей позиции по горизонтали и вертикали. Представим фрагмент доски в виде таблицы, при этом символ "\*" символизирует текущую позицию коня, а цифры - номер допустимого хода:

<table><tbody><tr><td></td><td>1</td><td></td><td>2</td><td></td></tr><tr><td>8</td><td></td><td></td><td></td><td>3</td></tr><tr><td></td><td></td><td>*</td><td></td><td></td></tr><tr><td>7</td><td></td><td></td><td></td><td>4</td></tr><tr><td></td><td>6</td><td></td><td>5</td><td></td></tr></tbody></table>

Думаю, этой иллюстрации вполне достаточно для понимания их реализации.

Самый интересный из методов - это, конечно же, **Try**. Собственно, ради его рассмотрения все и затевалось. Построение этого метода довольно типично для данного класса задач:

-   зафиксировать ход;
-   проверить, не найдено ли решение, и вернуть значение истина, если да;
-   генерировать допустимые правилами следующие ходы и пробовать их;
-   если следующий ход увенчался успехом, прекратить перебор, иначе стереть его и продолжить перебор;
-   если все ходы перебраны и решение не найдено, вернуть значение ложь.

Теперь у нас есть все необходимое для решения поставленной задачи. Дело за малым - написать основную программу. Она весьма проста (файл **Main.cpp**):

// Knight.cpp : Defines the entry point for the console application.  
//  
//  
#include <stdio.h>  
#include <string.h>  
#include "Knight.h"

int main(int argc, char\* argv\[\])  
{  
  printf("Knight Program\\n\\n");  
  CBoard brd(5);  
  CKnight kn(&brd);

  bool b = kn.Try(0, 0, 1);  
  if (b) // решение найдено  
    brd.Dump(); // вывести его  
  else  
    printf("No solution found...\\n");  
    return 0;  
}

ЗаключениеТема рекурсии, пожалуй, столь же неисчерпаема, сколь и число приложений, где ее применение делает программы ясными, компактными и изящными (а что эти свойства значат для программ, вы можете узнать в разделе, посвященном переводам статей Дейкстры). Если вы попробовали выполнить приведенные примеры, то наверняка имели возможность лично убедиться в том, что слухи о прожорливости к ресурсам и низкой эффективности рекурсии несостоятельны.

Весьма показательно было бы также попытаться решить те же задачи посредством итерации. Это наглядно продемонстрирует, что теоретическая эквивалентность итерации и рекурсии может быть не так уж легко достижима на практике.

К сожалению, объем статьи получился уже значительным, да и времени на ее подготовку потребовалось немало. Поэтому пора ставить точку, хотя тема, на мой взгляд, не исчерпана полностью.

Если вы находите тему интересной и полезной для себя и хотели бы продолжить разговор о рекурсии, оставьте свои пожелания на форуме. Материал как минимум еще на одну статью найдется без труда.

Alf, 21 апреля 2004
---
created: 2022-12-23T07:40:36 (UTC +03:00)
tags: [статья,]
source: https://club.shelek.ru/viewart.php?id=205
author: 
---

# Статья: "Заметки о рекурсии - 2. Взгляд на рекурсию изнутри."

> ## Excerpt
> Клуб программистов "Весельчак У". Статья:  "Заметки о рекурсии - 2. Взгляд на рекурсию изнутри."

---
В [прошлой статье](https://club.shelek.ru/viewart.php?id=184) мы познакомились с замечательным приемом программирования  рекурсией, рассмотрели его достоинства и недостатки, рассмотрели примеры, где она приводит к простым и элегантным решениям и где она, напротив, неуместна. Теперь я предлагаю вам заглянуть внутрь этого механизма и понаблюдать в деталях, что же происходит внутри программы при использовании рекурсивных вызовов.

Конечно же, рекурсию вполне можно использовать как черный ящик, не заглядывая внутрь, и некоторые именно так и делают (хотя многие не делают даже так). Однако понимание некоторых тонкостей позволит вам избежать некоторых неприятных сюрпризов, которые рекурсия может преподнести и на которых базируется предубеждение против ее использования.

Итак, открываем крышку черного ящика и смотрим внутрь, знакомясь с нехитрым механизмом внутри.

Прежде чем приступить непосредственно к изложению материала, хотелось бы упомянуть добрым словом людей, сопричастных к этой теме.

Прежде всего, конечно же,  **Dimyan**, который в процессе своих неутомимых изысканий в дебрях .NET добрался наконец и до задачи, которую просто грех было не решить рекурсивными методами  обработки древовидной структуры  и тем самым сделал тему рекурсии не предметом абстрактного разговора, а реальным приемом, позволившим построить компактный и красивый алгоритм решения конкретной проблемы. Без этого статья, безусловно, потеряла бы изрядную часть интереса.

Добрые слова **Never** после первой публикации явились доказательством того, что время на работу над статьей было потрачено не зря и тему следует продолжить. Если справедливость восторжествует и программирование наконец-то займет достойное место среди прочих искусств, то я уже знаю имя десятой музы, которая будет ему покровительствовать :).

Нельзя также не высказать благодарности людям, которые приняли самое деятельное участие в обсуждении статьи.

**ysv\_** обнаружил ошибку, которая вкралась в одну из моих программ, и предложил ряд своих вариантов.

**npak** вопреки сложившейся у большинства программистов печальной традиции сразу же бросаться кодировать предварительно подверг задачу анализу с точки зрения теории автоматов, что в результате позволило нам рассмотреть два корректных решения одной задачи  рекурсивное и нерекурсивное  и воочию убедиться в допустимости и эквивалентности обоих подходов. Ему также принадлежат решения дополнительных задач, которые я предложил в форуме для закрепления навыков рекурсивного подхода.

Надеюсь, что и эта статья вызовет интерес у читателей.

В качестве иллюстративного материала для статьи я выбрал, как ни странно, подпрограмму рекурсивного вычисления факториала. Хотя мы и пришли к очевидному выводу, что на практике рекурсивный подход для данной задачи существенно уступает итеративному, тем не менее для данного применения эта подпрограмма имеет ряд вполне подходящих свойств:

-   она предельно компактна и содержит минимум операторов, которые не будут заслонять своими деталями картины в целом;
-   она не содержит никаких обращений к каким-либо объектам вне самой подпрограммы (в отличие от ханойских башен и многих подобных программ);
-   при всем этом она решает вполне реальную, а не надуманную задачу, ее результат осмыслен, и его корректность может быть без труда проверена.

В качестве рабочей среды я предпочел использование **Microsoft Visual C++** версии 6 в составе интегрированной среды **Visual Studio 6**. Полагаю, большинство читателей располагают этим средством и не встретят затруднений при попытке воспроизвести примеры на своем рабочем компьютере. Не думаю также, что предпочитающих платформу .NET или компиляторы от Borland (включая старый добрый 16-разрядный C++ 3.1 для реального режима) ждут какие-то серьезные трудности; вам придется только повнимательнее изучить руководство к своему отладчику.

Для подготовки к выполнению приведенных в статье примеров вам придется выполнить следующие действия:

-   создать пустой проект консольного приложения;
-   создать в этом проекте файл с исходным текстом программы;
-   скопировать в него следующий исходный код:

long int Fact(long int N)  
{  
if (N < 1) return 0;  
else if (N == 1) return 1;  
else return N \* Fact(N-1);  
}  
  
int main(void)  
{  
long int F3;  
F3 = Fact(3);  
return 0;  
}

  

-   открыть окно свойств проекта;
-   выбрать конфигурацию Win32 Release;
-   на вкладке C++ в категории General установить: Optimization = Minimize Size, Debug info = Program Database;
-   установить Win32 Release в качестве активной конфигурации;
-   откомпилировать программу.

Все, теперь во всеоружии мы можем приступать к погружению во внутренний мир рекурсивной программы.

Начинаем сеанс отладки нажатием на клавишу **F11**. Желтая стрелка, указывающая текущий оператор, устанавливается на первой строке нашей главной программы.

Теперь самое время открыть дополнительные окна, которые предоставят нам информацию о деталях процесса, происходящего в нашей нехитрой программе. В меню **View -> Debug Windows** выбираем:

-   Registers  для просмотра содержимого регистров процессора;
-   Memory  для исследования содержимого оперативной памяти;
-   Disassembly  для просмотра выполняемых инструкций процессора;
-   Call Stack  для доступа к содержимому стека вызовов функций.

Вот что находится в этих окнах в текущий момент (некоторые несущественные для рассмотрения детали, вроде содержимого не интересующих нас регистров, я буду опускать:

**Registers:**

EAX = 00321138  
EBX = 7FFDF000  
ECX = 00320768  
EDX = 00320000  
ESI = 00000000  
EDI = 00000000  
EIP = 00401021  
ESP = 0012FF84  
EBP = 0012FFC0  
EFL = 00000206  
CS = 001B DS = 0023  
ES = 0023 SS = 0023  
FS = 0038 GS = 0000 OV=0  
UP=0 EI=1 PL=0 ZR=0 AC=0  
PE=1 CY=0

**Disassembly:**

1:    long int Fact(long int N)  
2:    {  
00401000 56                   push        esi  
00401001 8B 74 24 08          mov         esi,dword ptr \[esp+8\]  
00401005 6A 01                push        1  
00401007 58                   pop         eax  
00401008 3B F0                cmp         esi,eax  
0040100A 7D 04                jge         Fact+10h (00401010)  
0040100C 33 C0                xor         eax,eax  
0040100E 5E                   pop         esi  
6:    }  
0040100F C3                   ret  
3:        if (N < 1) return 0;  
4:        else if (N == 1) return 1;  
00401010 74 0D                je          Fact+1Fh (0040101f)  
5:        else return N \* Fact(N-1);  
00401012 8D 46 FF             lea         eax,\[esi-1\]  
00401015 50                   push        eax  
00401016 E8 E5 FF FF FF       call        Fact (00401000)  
0040101B 0F AF C6             imul        eax,esi  
0040101E 59                   pop         ecx  
0040101F 5E                   pop         esi  
6:    }  
00401020 C3                   ret  
7:  
8:    int main(void)  
9:    {  
00401021 6A 03                push        3  
00401023 E8 D8 FF FF FF       call        Fact (00401000)  
00401028 59                   pop         ecx  
10:       long int F3;  
11:       F3 = Fact(3);  
12:       return 0;  
00401029 33 C0                xor         eax,eax  
13:   }  
0040102B C3                   ret

**Call Stack:**

main() line 9  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

Как видно из окна **Disassembly**, будут выполнены следующие действия: сначала в стек будет отложена константа 3 (фактический аргумент вызываемой функции Fact), а затем вызвана сама функция.

Почему именно так? Для этого нам придется прерваться на некоторое время и ознакомиться с механизмом передачи параметров в функции C++.

Вообще говоря, способов передачи фактических параметров функции при ее вызове не так уж мало. Различия между ними  одна из причин барьера между языками, когда функция, разработанная на одном языке программирования, не может быть вызвана на другом, и приходится переписывать ее заново, хотя функциональность нас вроде бы полностью устраивает.

Более-менее благополучно дело с межъязыковой совместимостью обстоит только в управляемой среде .NET, но в данном случае мы имеем дело с традиционным кодом, который в данный момент преобладает. Поэтому уделим немного времени рассмотрению механизмов, используемых для передачи параметров в программах на VC++ V6.

Подробно этот материал изложен в MSDN, в разделе **MSDN Library Visual Studio 6.0 => Visual C++ Documentation => Using Visual C++ => Visual C++ Programmers Guide => Adding Program Functionality => Overviews => Calling conventions: Overview**.

В прежних системах программирования порой применялись экзотические методы передачи параметров подпрограммам. Например, в **FORTRAN IV** аргументы (передаваемые только по ссылке) образовывали непрерывный блок в памяти, указатель на который передавался в одном из регистров общего назначения. Со временем разработчики реализаций пришли к выводу, что наиболее рационально передавать аргументы через стек. Впрочем, и тут нашелся повод для разногласий, даже целых два: 1) в каком порядке передавать параметры  слева направо или справа налево, и 2) кто будет чистить стек после выполнения функции  сама вызванная функция или тот, кто ее вызвал. К сожалению, там, где есть возможность выбора, найдется источник несовместимости. Итак, краткая сводка используемых методов передачи параметров (или, более корректно, _соглашений о вызовах_):

<table><tbody><tr><td>Метод</td><td>Передача параметров</td><td>Кто чистит стек</td><td>Где применяется</td></tr><tr><td>__cdecl</td><td>В стек, справа налево</td><td>Вызывающий</td><td>По умолчанию в C и C++</td></tr><tr><td>__stdcall</td><td>В стек, справа налево</td><td>Сама функция</td><td>Вызовы почти всех системных функций, а также по умолчанию внутренние функции Visual Basic</td></tr><tr><td>__fastcall</td><td>Два первых параметра&nbsp; в регистрах ECX и EDX, остальные&nbsp; в стек, справа налево</td><td>Сама функция</td><td>По умолчанию в Borland Delphi</td></tr><tr><td>thiscall</td><td>Указатель this передается в регистре ECX, параметры&nbsp; в стек, справа налево.</td><td>Сама функция</td><td>Функциями-методами классов без списка аргументов переменной длины. Не может быть задан явно.</td></tr></tbody></table>

Как видно из этого краткого обзора, только **\_\_cdecl** не чистит за собой стек после завершения функции, поэтому вызывающая функция должна каждый раз делать это явно. Естественно, это приводит к некоторому количеству избыточного кода по сравнению с другими соглашениями, для которых единственный фрагмент очистки стека встроен в само тело функции. Казалось бы, это нерационально, и выбор **\_\_cdecl** в качестве соглашения по умолчанию неоправдан.

На самом деле это  единственное соглашение, которое может поддерживать вызовы функций с переменным числом параметров вроде **printf()**. Поскольку функция не знает заранее, сколько аргументов получит при вызове, сгенерировать универсальный код для очистки стека в данном случае проблематично. Зато непосредственно в точке вызова функции об аргументах известно все, поэтому очистка стека не представляет проблем.

Таким образом, разработчики среды программирования идут на компромисс: ценой увеличения объема кода добиваются большей гибкости при передаче параметров. К счастью, как мы вскоре убедимся сами, эта цена не столь уж велика, чтобы существенно повлиять на общий размер программы.

Итак, подведем итог сказанному:

-   по умолчанию C++ использует соглашение о вызовах \_\_cdecl (и, поскольку мы явно не меняли это умолчание, код в нашей программе порожден с использованием именно этого соглашения);
-   параметры функции заносятся в стек в обратном порядке, справа налево; таким образом, первый параметр окажется на верхушке стека, второй  под ним и так далее; последний параметр зароется в стек глубже всех, поскольку начинается занесение именно с него;
-   по завершении функция ничего не делает с параметрами; очистка стека  обязанность вызывающего кода.

Теперь нам проще будет разобраться в коде нашего примера.

Посмотрим теперь, куда указывает желтая стрелка в окне **Disassembly**. Это и есть текущие инструкции нашей программы. Поскольку мы еще не успели продвинуться по нашему примеру, мы видим самое начало функции **main**:

00401021 6A 03                push        3  
00401023 E8 D8 FF FF FF       call        Fact (00401000)

На всякий случай повторю, что первая из этих инструкций заносит константу 3 на верхушку стека, а вторая  вызывает функцию Fact. Совместно они реализуют вызов **Fact(3)**.

Инструкция **call**  это команда процессора, посредством которой реализуются вызовы подпрограмм. По этой команде выполнение потока инструкций временно прекращается и переходит к инструкциям, составляющим код подпрограммы (по адресу, который является аргументом инструкции). В конце подпрограммы находится инструкция ret, по которой поток инструкций возвращается обратно в код, вызвавший подпрограмму, и продолжается со следующей за call инструкции.

Разумеется, для возвращения необходимо сохранить адрес возврата, причем по возможности таким образом, чтобы вызываемая функция в свою очередь тоже могла вызывать другие функции, т.е. поддерживалась _вложенность_ вызовов. Наилучшим образом для хранения адреса возврата, как и аргументов функций, подходит _стек_.

Поскольку следующие наши шаги непосредственно будут связаны со стеком (да и вообще при реализации рекурсии стек играет весьма важную роль), имеет смысл познакомиться с ним поближе.

На логическом уровне стек проще всего представить себе как стопку однородных предметов (например, книг, тарелок или любых других предметов, которые можно сложить друг на друга стопкой). Правило тут простое: класть очередной предмет можно только на верхушку стопки, и брать тоже можно только верхний предмет.

Такую структуру называют также очередью LIFO (Last In  First Out, т.е. последним пришел  первым ушел). Она незаменима, например, при хранении адресов возврата при вызовах функций.

Например, предположим, что функция F1 в некоторой точке вызывает F2, а та, в свою очередь, при выполнении вызывает F3. Для простоты примем, что изначально стек пуст.

void F1(void)  
{  
  
F2();  
  
}  
  
void F2(void)  
{  
  
F3();  
  
}  
  
void F3(void)  
{  
  
}

Исходное состояние стека:

После вызова F2:

<table><tbody><tr><td>???</td></tr><tr><td>???</td></tr><tr><td>Адрес возврата в F1</td></tr></tbody></table>

После вызова F3:

<table><tbody><tr><td>???</td></tr><tr><td>Адрес возврата в F2</td></tr><tr><td>Адрес возврата в F1</td></tr></tbody></table>

Выход из F3, возврат в F2:

<table><tbody><tr><td>???</td></tr><tr><td>???</td></tr><tr><td>Адрес возврата в F1</td></tr></tbody></table>

Выход из F2, возврат в F1:

Итак, вывод: корректная работа со стеком возможна только через его вершину. Несоблюдение этого правила ведет к нарушению целостности структуры. Вспомните фильм Операция Ы и катастрофу, которая постигла Вицина при попытке извлечь девайс со дна заполненного стека

Стек: физическая реализацияРазумеется, никакого специального аппаратного блока для реализации стека внутри компьютера вы не найдете; по крайней мере, это на 100% верно для IBM PC-совместимых компьютеров, на одном из которых вы сейчас наверняка и работаете с этой статьей. Все, что найдется подходящего в его архитектуре, - это однородный массив ячеек оперативной памяти и набор регистров внутри центрального процессора. К счастью, этого вполне достаточно для организации стека.

Физически стек  это специально выделенная область оперативной памяти. Стек растет в обратном направлении, т.е. от старших адресов  к младшим. Таким образом, дно стека  это ячейка с максимальным адресом в области стека.

На вершину стека указывает специальный регистр процессора, который так и называется  указатель стека. Его содержимое можно увидеть среди прочих регистров в окне **Registers**, он фигурирует там под именем **ESP**.

Стек работает со словами размером 32 бита, или 4 байта. В случае, если реальные данные требуют для хранения меньше места, свободный остаток не используется. Это может показаться расточительным, но, во-первых, обмен словами между процессором и оперативной памятью наиболее эффективен, во-вторых, меньше вероятность нарушить структуру стека, скажем, положив туда длинное целое, а потом забрав один байт.

При занесении слова в стек командой **push** сначала содержимое регистра **ESP** уменьшается на 4, а затем слово заносится в оперативную память по адресу, который хранится в **ESP**.

При извлечении слова из стека командой **pop** с верхушки стека, то есть из ячейки оперативной памяти с адресом, хранящемся в **ESP**, считывается значение, а потом содержимое **ESP** увеличивается на 4.

Обратите внимание, что при выборке из стека не происходит физического уничтожения прежних значений хранящихся там данных. Они всего лишь перестают быть доступны через операции со стеком, но по-прежнему остаются в тех же ячейках оперативной памяти. Разумеется, прежние значения этих данных будут уничтожены при последующем занесении новых данных в стек.

Разумеется, это отступление было неспроста. Теперь мы знаем, что такое стек и где его искать.

У нас по-прежнему открыто окно **Memory**, которое мы до сих пор никак не использовали. Разумеется, открывали мы его не для того, чтобы чем-то заполнить экран. Самое время воспользоваться им для того, чтобы следить за состоянием стека, вооружившись только что полученными познаниями.

Для этого:

-   находим ESP среди регистров процессора в окне Registers;
-   двойным щелчком левой кнопки мыши выделяем его содержимое (у меня это - 0012FF84);
-   копируем его в буфер (комбинацией <Ctrl>+C или кому как больше нравится);
-   в окне Memory выделяем поле ввода Address и вставляем туда содержимое указателя стека (комбинацией <Ctrl>+V);
-   для большего удобства переключаем окно Memory на показ содержимого памяти в виде 32-разрядных слов (раз уж стек все равно работает именно с ними); сделать это можно, щелкнув правой кнопкой мыши в окне и выбрав Long Hex Format в контекстном меню.

Теперь мы обзавелись инструментом для наблюдения за метаморфозами, происходящими в стеке. Это было необходимо, поскольку все самое интересное в ближайшее время будет происходить именно там.

Наберемся храбрости и нажмем 2 раза клавишу **F11** (каждое нажатие  это команда отладчику выполнить очередную инструкцию процессора в пошаговом режиме). Сразу же заглянем в стек:

0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Мы видим, что значение формального параметра  3  оказалось в стеке. А на вершине стека теперь лежит 00401028. Обратившись к окну **Disassembly**, мы видим, что это  адрес инструкции, следующей за вызовом функции **Fact**.

Если вы аккуратно проделали предыдущие действия, в данный момент программа должна в первый раз войти в функцию **Fact**, и указатель текущей инструкции (желтая стрелка) находится перед первой ее инструкцией.

Прежде всего посмотрим, что делается в стеке вызовов функций (окно **CallStack**):

Fact(long 3) line 2  
main() line 9 + 7 bytes  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

Судя по тому, что мы уже знаем о стеках, события в нем должны идти в порядке, обратном хронологическому, т.е последнее находиться на вершине стека. Так и есть: в верхней строке мы видим вызов **Fact(3)**, затем вызвавшую ее функцию  **main()**. (Ниже расположены еще 2 строки, имеющие отношение к работе исполняющей системы. Для того, чтобы наша программа могла выполняться, перед ее запуском необходимо проделать некоторую вспомогательную работу по созданию среды, в которой она будет выполняться. Однако этот вопрос находится за пределами нашей темы).

Первая инструкция функции:

00401000 56                   push        esi

Как мы уже знаем, команда **push** заносит в стек свой операнд (в данном случае  содержимое регистра **ESI**). Очевидно, компилятор планировал использовать этот регистр для своих нужд, поэтому позаботился о предварительном сохранении его содержимого в стеке. Такой подход является хорошим стилем при программировании на ассемблере  перед тем, как менять содержимое регистра в подпрограмме, сохранить его содержимое в стеке, а перед выходом из подпрограммы восстановить его содержимое, если только регистр не используется для возврата результата функции,  и позволяет избежать многих неприятных ошибок.

Выполняем очередную команду (напоминаю, делается это нажатием на клавишу **F11**) и проверяем состояние стека. Обратите внимание, что значение **ESP** изменилось с 0012FF7C на 0012FF78. Новое содержимое стека:

0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Следующая инструкция подтверждает нашу догадку о том, что сохранение **ESI** было сделано неспроста:

00401001 8B 74 24 08          mov         esi,dword ptr \[esp+8\]

Эта инструкция загружает в регистр **ESI** содержимое двойного слова (**dword**), которое смещено на 8 байт (или 2 слова) относительно указателя стека **ESP**.

Прокрутим обратно в памяти историю работы программы со стеком. На вершине стека (**\[esp\]**) лежит прежнее содержимое регистра **ESI**, под ним (**\[esp+4\]**)  адрес возврата в вызывающую функцию **main()**, а еще ниже (**\[esp+8\]**)  константа 3, которая была загружена в стек перед вызовом **Fact** (в полном соответствии с соглашением о вызовах **\_\_cdecl**, как мы уже знаем).

Выполняем эту инструкцию. Обратите внимание, что в окне **Registers** содержимое **ESI** отмечено красным цветом. Отладчик использует это выделение, чтобы программисту легче было заметить, что содержимое регистра изменилось. Новое значение, как и следовало ожидать,

ESI = 00000003

Итак, наша функция получила доступ к значению своего аргумента через регистр **ESI**. Как видно из исходного текста функции

if (N < 1) return 0;

сначала функция должна сравнить аргумент с 1 и возвратить в случае равенства значение 0. Столь нехитрая конструкция, тем не менее, не может быть выполнена в коде процессора за один шаг.

Прежде всего нужно получить константу 1. Это достигается парой инструкций:

00401005 6A 01                push        1  
00401007 58                   pop         eax

Константа сначала помещается в стек, а потом загружается из него в регистр **EAX**. (Казалось бы, нерациональный подход, проще было бы прямо загрузить 1 в **EAX**. Однако попробуйте  и убедитесь, что в этом случае код получается длиннее. Впрочем, не будем заострять на этом внимание, ибо статья не об оптимизации).

Следующая инструкция производит сравнение:

00401008 3B F0                cmp         esi,eax

Эта инструкция выполняется аналогично вычитанию, однако при этом ни один из операндов не меняется. Зато флаги в регистре состояния устанавливаются в соответствии с результатом  и могут быть использованы для условного перехода.

Обычно инструкция **cmp** используется в паре с последующим условным переходом. Именно так обстоит дело и в нашем случае:

0040100A 7D 04                jge         Fact+10h (00401010)

В случае, если значение аргумента (**ESI**) больше или равно 1 (**EAX**), выполнение продолжается с ветки **else**. В нашем случае так и есть (3 ;gt= 1).

Следом должно выполниться сравнение аргумента с 1 на равенство:

else if (N == 1) return 1;

Это делается при помощи инструкции условного перехода по равенству:

00401010 74 0D                je          Fact+1Fh (0040101f)

В этом случае инструкции условного перехода не предшествует инструкция сравнения, т.к. предыдущая инструкция условного перехода оставляет состояние флагов неизменным, и им можно воспользоваться.

В нашем случае условие (3 == 1) не выполняется, поэтому выполнение функции продолжается.

А теперь  самое интересное: рекурсивный вызов:

5:        else return N \* Fact(N-1);

Должен быть произведен вызов функции **Fast(N-1)**. Для этого, как мы уже знаем, сначала следует положить на вершину стека значение (N-1). Это делается парой инструкций

00401012 8D 46 FF             lea         eax,\[esi-1\]  
00401015 50                   push        eax

Как всегда, не забываем заглянуть в стек:

ESP = 0012FF74  
  
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Значение (N-1), то есть 2, теперь находится на вершине стека. Остается произвести рекурсивный вызов:

00401016 E8 E5 FF FF FF       call        Fact (00401000)

Выполняем его (**F11**).

На первый взгляд, мы снова оказались в том же месте, что и в предыдущем разделе. Указатель текущей инструкции находится все на той же первой инструкции функции **Fact**. Но это только на первый взгляд.

Во-первых, на вершине стека у нас находится 2, а не 3, как в прошлый раз.

Во-вторых, посмотрим в окно Call Stack:

Fact(long 2) line 2  
Fact(long 3) line 5 + 9 bytes  
main() line 9 + 7 bytes  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

На вершине стека вызовов появился новый вызов **Fact(2)**.

Продолжаем пошагово выполнять функцию до инструкции **call** по адресу 00401016. Все происходит почти так же, как и в прошлый раз, за исключением того, что в этот раз на вершине стека находится значение 1:

ESP = 0012FF68  
  
0012FF68  00000001   
0012FF6C  00000003   
0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

В первую очередь, как обычно, проверяем стек вызовов:

Fact(long 1) line 2  
Fact(long 2) line 5 + 9 bytes  
Fact(long 3) line 5 + 9 bytes  
main() line 9 + 7 bytes  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

Пошагово выполняем функцию до инструкции **cmp** по адресу 00401008. В этот раз значения сравниваемых регистров у нас равны:

EAX = 00000001  
ESI = 00000001

Следовательно, можно ожидать, что результат сравнения будет иным. Проверим это предположение, выполнив инструкцию **cmp**. В этот раз в окне **Registers** установился флаг **ZR=1**, что указывает на равенство операндов инструкции. По этому условию инструкция

00401010 74 0D                je          Fact+1Fh (0040101f)

должна осуществить переход.

Так и есть: по очередному нажатию **F11** попадаем на инструкцию

0040101F 5E                   pop         esi

Состояние стека на этот момент

ESP = 0012FF60

0012FF60  00000002   
0012FF64  0040101B   
0012FF68  00000001   
0012FF6C  00000003   
0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Инструкция **pop** восстанавливает значение регистра **ESI**, сохраненное в момент входа в функцию (если вы внимательно отслеживали операции со стеком, то заметили, что это  не что иное, как значение формального аргумента **N** из вызывающей функции). Выполняем ее:

ESI = 00000002

ESP = 0012FF64

0012FF64  0040101B   
0012FF68  00000001   
0012FF6C  00000003   
0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Наконец-то мы достигли инструкции **ret**, которая осуществляет возврат из подпрограммы (путем загрузки счетчика команд **EIP** содержимым вершины стека):

00401020 C3                   ret

С этого момента начинается обратная раскрутка стека вызовов.

Выполнив инструкцию **ret**, мы возвращаемся обратно в первый рекурсивный вызов функции:

Call stack:  
Fact(long 2) line 5 + 9 bytes  
Fact(long 3) line 5 + 9 bytes  
main() line 9 + 7 bytes  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

Остаток кода функции, который будет выполняться далее, имеет вид:

0040101B 0F AF C6             imul        eax,esi  
0040101E 59                   pop         ecx  
&nbsз;0040101F 5E                   pop         esi  
6:    }  
00401020 C3                   ret

Инструкция **imul** выполняет целочисленное умножение. В данном случае будет вычислено произведение **EAX\*ESI**, и результат будет занесен в регистр **EAX**.

Как мы уже выяснили, в регистре **ESI** в нашей функции хранится значение **N**, то есть в данном случае 2. Но вот в регистр **EAX** мы вроде бы ничего подходящего не заносили. Зачем мы используем его в инструкции умножения?

Чтобы понять это, нам нужно узнать еще небольшую деталь о вызове функций. Мы уже знаем, как передать значения фактических параметров в функцию, но ни слова до сих пор не сказали о том, как же функция возвращает результат своей работы. На самом деле это происходит очень просто, во всяком случае, когда результат функции типа **int**: при выходе из функции возвращаемое значение находится в регистре **EAX**.

Теперь все становится понятно: вызов **Fact(1)** завершился, вернув результат 1 в регистре **EAX**, который теперь и используется в инструкции умножения. В результате выполнения инструкции

EAX = 00000002

Состояние стека в данный момент:

ESP = 0012FF68

0012FF68  00000001   
0012FF6C  00000003   
0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Следующая инструкция

0040101E 59                   pop         ecx

на первый взгляд несколько туманна. Мы ведь не использовали регистр **ECX** до сих пор, зачем же нам нужно загружать его из стека?

Все станет ясно, если вспомнить подробнее детали соглашения **\_\_cdecl**. Как уже было сказано, при этом соглашении убирать параметры функции из стека должен _вызывающий_ код, а не сама функция. Данная инструкция  это самый компактный способ продвинуть указатель стека вперед на одно слово. В качестве побочного эффекта при этом изменяется значение регистра **ECX**, но его содержимое не представляет для нас интереса.

Итак, данная инструкция всего лишь чистит стек после вызова **Fact(1)**:

ESP = 0012FF6C

0012FF6C  00000003   
0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Очередная инструкция

0040101F 5E                   pop         esi

восстанавливает значение регистра **ESI**, которое мы испортили в начале функции:

ESI = 00000003

ESP = 0012FF70

0012FF70  0040101B   
0012FF74  00000002   
0012FF78  00000000   
0012FF7C  00401028   
0012FF80  00000003   
0012FF84  004010E0

Далее инструкция **ret** завершает очередной вызов **Fact(2)**:

Call Stack:  
Fact(long 3) line 5 + 9 bytes  
main() line 9 + 7 bytes  
MECH! mainCRTStartup + 180 bytes  
KERNEL32! 77e814c7()

Теперь мы вернулись в первый, нерекурсивный вызов **Fact(3)**. Указатель текущей инструкции вновь находится в строке

0040101B 0F AF C6             imul        eax,esi

но в этот раз уже

EAX = 00000002  
ESI = 00000003

(соответственно значения **Fact(2)** и **N**).

Пошагово выполняем остаток инструкций функции и возврат в вызывающую функцию **main()**. Теперь у нас:

EAX = 00000006  
ESI = 00000000  
  
ESP = 0012FF80  
  
0012FF80  00000003   
0012FF84  004010E0

Осталось рассмотреть теперь совсем немного. Инструкция

00401028 59                   pop         ecx

как мы уже знаем, очищает верхушку стека от аргумента предыдущего вызова (константы 3). Следующая инструкция

00401029 33 C0                xor         eax,eax

заносит 0 в регистр **EAX**, который снова будет использован в качестве возвращаемого функцией **main()** значения (c точки зрения исполняющей системы **main()**  такая же функция, как и любая другая, за исключением того, что с нее начинается выполнение программы, и она так же точно может возвращать значение, которое используется системой в качестве кода завершения; обычно 0  нормальное завершение, отличное от нуля значение  код ошибки, хотя это соглашение применяется не всегда).

Наконец, инструкция

0040102B C3                   ret

осуществляет возврат в исполняющую систему, которая выполнит код, необходимый для корректного завершения программы.

Разумеется, в качестве примера я выбрал наиболее примитивный вариант рекурсивной программы, который выполняет минимум действий и лишен каких бы то ни было средств ввода/вывода. Тем не менее на этом примере мы смогли увидеть метаморфозы, проистекающие в стеке при работе рекурсивной программы, а также ознакомиться с низкоуровневыми деталями передачи параметров в вызываемые функции (разумеется, не только рекурсивные).

Кроме того, мы могли убедиться, что при работе функции стек использовался не столь уж расточительно: на каждый рекурсивный вызов расходовались 3 слова в стеке. Если учесть, что результат вычисления 50! уже превосходит 1 с 64 нулями и вряд ли может потребоваться на практике, вряд ли при реальных вычислениях будет использовано более 150 двойных слов стека, что можно назвать непомерной ценой лишь с большой натяжкой. Конечно, параметров у рекурсивной функции вполне может быть и более, чем 1, да и могут потребоваться вспомогательные локальные переменные, которые также размещаются в стеке. Однако при рациональном построении алгоритма и умеренной глубине рекурсии использование стека обычно остается в пределах разумного.

Конечно, убежденные противники рекурсии всегда смогут привести пример рекурсивного приложения, которому и гигабайта стека окажется недостаточно. Но тут уж мне остается только вспомнить поговорку, что существует категория людей, которые даже когда их заставляют молиться богу, умудряются получить травмы головы

(Если вам все еще интересно, продолжение следует).

**Alf, 1 августа 2004**

_Автор: Alf_
---
created: 2022-12-23T07:36:43 (UTC +03:00)
tags: [версия для печати,статья,клуб программистов,programers club,весельчак у,veselchak u,статьи,articles,книги,books,файлы,files,фото,foto,photo,программирование,programing,driver,драйвер,ddk,веб,web,скрипты,php,perl,html,xml,js,javascript,базы данных,dbf,databases,мускул,mysql,оракле,oracle,interbase,си,c++,ansi c,vc++,visual,начинающим,for beginners,технологии разработки,uml,патерны,patterns,idef,ооп,ява,жава,java,си шарп,c#,дот нет,.net,дельфи,delphi,бейсик,vb,visual basic,vba,1c,1с,авторское по,встроенное по,embedded,сети,net,network,программы,software,игры,games,графика,graphics,graphix,grafix]
source: https://club.shelek.ru/print.php?id=220
author: Громозека, MOPO3, RXL
---

# Клуб программистов "Весельчак У". Версия для печати. Статья: "Заметки о рекурсии - 3. Косвенная рекурсия"

> ## Excerpt
> Клуб программистов "Весельчак У". Версия для печати. Статья:  "Заметки о рекурсии - 3. Косвенная рекурсия"

---
Возникает замкнутый круг: одна из процедур неизбежно должна вызвать другую, еще не описанную; перестановка процедур местами ничего не дает, поскольку проблема симметрична. К счастью, разорвать его весьма просто; в частности, для C++ достаточно лишь объявить процедуру **B** перед вызовом, не описывая ее тело:

В других языках также предусмотрены аналогичные средства решения подобных проблем. Например, в языке Pascal имеется ключевое слово **forward**, которое указывает, что тело процедуры будет описано позже.

Впрочем, не все языки нуждаются в подобных ухищрениях. Так, Visual Basic превосходно обходится без предварительного описания подпрограмм, а сами подпрограммы могут располагаться в произвольном порядке.

Итак, технические сложности для реализации косвенной рекурсии нами успешно преодолены. Теперь осталось определиться с другим вопросом, не менее важным: о целесообразности ее применения.

Конечно же, мы не будем довольствоваться голословным утверждением, что косвенная рекурсия важна, а попросту приведем пример задачи, которая легко и изящно решается с ее использованием.

Чтобы наши усилия не пропали впустую, постараемся выбрать такую задачу, которая достаточно проста, чтобы на ее примере можно было учиться, не утопая в премудростях задачи, и в то же время достаточно полезна, чтобы найти практическое применение.

Нередко в программы, например, имеющие дело с финансами, встраивается простейший калькулятор, позволяющий на лету произвести несложные расчеты. Еще чаще, к сожалению, такого калькулятора в программе не оказывается. Помнится, не так давно на форуме задавали вопрос о том, где можно взять подобную подпрограмму. Мы попытаемся сделать подобный калькулятор своими руками.

Разумеется, написать простейший калькулятор  задача весьма расплывчатая. Поскольку нечетко поставленные задачи  настоящий бич программистского племени, мы не будем поддаваться искушению сразу же приступить к кодированию, а сначала более основательно поработаем над заданием.

Итак, мы хотим написать подпрограмму, которая получает на входе арифметическое выражение, вычисляет его значение, если это выражение корректно, и возвращает его значение в вызывающую программу.

Такая постановка уже выглядит конкретнее, но тем не менее еще далека от совершенства. Какое выражение мы считаем арифметическим и корректным? Уточняем дальше.

Прежде всего, очевидно, что наш калькулятор должен работать с числами. Для простоты мы ограничимся действительными числами, состоящими из целой и (необязательной) дробной частей. Если дробная часть присутствует, она отделяется от целой части десятичной точкой. Мы намеренно откажемся от экспоненциального представления чисел в виде мантиссы и порядка, поскольку для простейших расчетов, как правило, такое представление избыточно.

Для того, чтобы приносить реальную пользу, наш калькулятор должен уметь производить операции над числами. Мы реализуем четыре действия арифметики: сложение, вычитание, умножение и деление.

На первый взгляд поставленная задача кажется весьма тривиальной. Арифметическое выражение представляется в виде последовательности чисел, отделенных друг от друга знаками арифметических операций. Достаточно считать первое число, затем знак операции, второе число  и можно выполнять операцию над считанными числами. Если за вторым числом следует новая операция, она выполняется над предыдущим результатом и следующим числом, и так далее, пока все выражение не будет подсчитано. Обычный цикл, и без рекурсии можно прекрасно обойтись.

Все было бы просто, если бы не одно но: арифметические операции не равнозначны по приоритету. Принято, что умножение и деление имеют приоритет выше, чем сложение и вычитание, и выполняются в первую очередь. Казалось бы, мелочь, но отравляет жизнь писателей программ-калькуляторов изрядно.

Получается, что, наткнувшись на операцию сложения, мы не можем выполнять ее немедленно, а должны заглянуть вперед на одну операцию. Если следующая операция  умножение, мы должны отложить текущую операцию, выполнить следующую за ней, а потом вернуться к текущей. И это еще не все, ведь за умножением может следовать еще одно умножение, которое тоже должно быть выполнено прежде. Конечно, задача не из неразрешимых, бывают и посложнее. Но тем не менее изяществом подобный алгоритм не блещет, и удовольствие от разработки такой программы вряд ли получишь. А ведь получившегося монстрика нужно потом еще и протестировать, ибо подобные запутанные программы нередко таят в себе ошибки.

Некоторые разработчики калькуляторов в таких случаях используют тактику страуса: можно спрятать голову в песок и делать вид, что ее не существует. В этом случае пользователи через некоторое время привыкнут к этому недостатку и будут подстраиваться под него, меняя порядок операндов таким образом, чтобы более приоритетные операции оказались впереди низкоприоритетных. Такой прием проходит не всегда, и тогда в дело идет клочок бумаги в качестве стека для записи промежуточных результатов в случае длинных выражений.

Собственно, именно так и работают большинство из виденных мной бухгалтерских калькуляторов (что не удивляет) и изрядная часть инженерных (что уже может быть неприятным сюрпризом). Помнится, много лет назад купленный мной на первом курсе для обсчета экспериментальных результатов калькулятор Texas Instruments изрядно удивил однокурсников, выдав, что 2+2\*2=6. Его коллеги советского производства семейства Электроника-ХХ единодушно сходились во мнении, что истинное значение данного выражения  8. Лишь несколько лет спустя их младшие товарищи усвоили-таки приоритет арифметических операций.

Мы не пойдем по этому пути и будем разрабатывать простую, но тем не менее корректную программу, которая не потребует от пользователя каких-либо ухищрений и вывертов. Это значит, что порядок выполнения операций будет определяться общепринятым приоритетом: сначала умножение/деление, затем сложение/вычитание.

Правда, при этом возникает другая проблема: как быть, если низкоприоритетную операцию потребуется выполнить в первую очередь? Традиционно для изменения порядка операций используются скобки. Так, например, запись:

означает, что сложение следует выполнить до умножения, и правильный результат такого выражения 25, а не 17, как предписывает приоритет операций.

Итак, очередное уточнение технического задания: программа-калькулятор принимает выражение, составленное из чисел, арифметических операций и скобок, и вычисляет его значение, руководствуясь следующими правилами:

Теперь требования к программе полностью прояснены. Однако возникает странное ощущение: сначала была поставлена довольно простая задача, затем она была уточнена до деталей  и вся простота куда-то улетучилась. Обычно бывает наоборот: по мере уточнения задачи программисту становится все яснее, как ее реализовать. Но у нас, похоже, не тот случай. Конечно, велик соблазн последовать примеру разработчиков бухгалтерских калькуляторов и не напрягаться самим, переложив проблему на потребителя. Мы ему, впрочем, не поддадимся, а попробуем взглянуть на проблему с другой стороны.

Зачастую взгляд на, казалось бы, сложную проблему с другой стороны может привести к простому и элегантному решению. Не является исключением и наш случай.

Рассмотрим выражение вроде

**2 \* 4 + 9 / 3 - 4 \* 5 / 2**

Согласно правилам, приведенным нами выше, вычисления должны производиться в следующем порядке:

-   2 \* 4 = 8
-   9 / 3 = 3
-   8 (п.1) + 3 (п.2) = 11
-   4 \* 5 = 20
-   20 (п.4) / 2 = 10
-   11 (п.3)  10 (п.5) = 1

Нагляднее порядок вычисления данного выражения можно представить в виде графа:

![](data:image/gif;base64,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)

На этом графе в виде бинарного дерева числа, над которыми выполняются арифметические операции, представлены листовыми вершинами. Они соединяются ребрами с узловыми вершинами, которые помечены знаками арифметических операций. Справа от каждого узла находится число, которое представляет собой порядковый номер выполнения операции при вычислении выражения.

Каждый узел фактически представляет собой подвыражение вида **левый-операнд операция правый-операнд**. Если обе дочерние вершины графа листовые, то есть операнды  числа, значение подвыражения определяется в результате выполнения над ними данной операции. Если же хотя бы одна из дочерних вершин узловая, соответствующий операнд сам является подвыражением, которое должно быть вычислено перед вычислением текущего подвыражения. Значение всего выражения можно вычислить, выполнив операцию, которой помечен корневой узел, над его операндами-подвыражениями.

Не правда ли, уже знакомая картина? Чтобы вычислить выражение, нужно вычислить значения левого и правого поддеревьев, а затем выполнить указанную в корне операцию. В свою очередь, каждое поддерево представляет собой уменьшенное подобие исходного (конкретно  меньше исходного на 1 уровень). Идеальная задача для применения рекурсии.

Итак, у нас две новости. Первая  хорошая, даже очень: чтобы вычислить выражение, достаточно пройтись по дереву, от корня до листьев, и просчитать каждый узел. Задача весьма тривиальная для тех, кто владеет рекурсией (а мы в ней изрядно поднаторели за два предыдущих урока).

А теперь плохая новость. Дерево это за нас никто строить не собирается, придется заняться этим самостоятельно. Все, что пользователь передает на вход нашей программы,  это текстовая строка, представляющая выражение в форме, дружественной пользователю (а именно  в точности как в примерах из школьного учебника арифметики). Значит, проблема с правильной расстановкой приоритетов операций никуда не делась, мы ее просто до поры отложили в сторону.

Выходит, наша возня с построением дерева вычисления выражений была впустую? Не совсем. Конечно, непосредственного и немедленного решения поставленной задачи мы не получили. Однако, если посмотреть на разобранное выражение повнимательнее, можно заметить, что в отсутствие скобок умножение и деление тяготеют к нижним уровням дерева, в то время как сложение с вычитанием  к верхним, ближе к корню (не забываем, что в математике многое поставлено с ног на голову, и даже деревья в ней растут корнями вверх). Это и понятно, ведь при рекурсивной обработке выражения верхние уровни обсчитываются в последнюю очередь. Отсюда важный вывод, который нам очень пригодится впоследствии. Точнее, этих выводов три, но они очень тесно связаны:

-   Выражение представляет собой сумму слагаемых.
-   Слагаемое представляет собой произведение сомножителей.
-   Сомножитель представляет собой число.

Данное представление выражения представляется мне куда более простым и логичным, чем 4 правила вычисления выражений, выведенные нами в предыдущей главе. (Разумеется, при этом не делается различие между сложением и вычитанием; точно так же единой операцией считаются умножение и деление). Остаются невыясненными лишь 2 вопроса:

-   Если мы сначала перемножаем сомножители, а потом складываем слагаемые, причем тут рекурсия?
-   Что делать, если в выражении встретилось подвыражение в скобках?

Отвечать, пожалуй, лучше начну со второго вопроса. Как мы уже выяснили на примере с построением дерева разбора выражения, подвыражение ничем в сущности не отличается от выражения (разве что проще). Поэтому немного подправим третий вывод о структуре выражения:

3'. **Сомножитель** представляет собой **число** либо **выражение, заключенное в скобки**.

Это небольшое с виду дополнение позволяет нам охватить все возможные варианты построения арифметического выражения из чисел, четырех операций арифметики и круглых скобок, позволяющих группировать подвыражения.

Теперь и ответ на первый вопрос становится очевиден. Мы определи **выражение** через **слагаемое**, **слагаемое**  через **сомножитель**, а **сомножитель**, в свою очередь,  через **выражение**. Круг замкнулся, и теперь мы знаем, что имя ему  взаимная рекурсия.

К сожалению, задача все еще не прояснилась настолько, чтобы стал очевиден путь ее решения, хоть мы и полагаем уже, что лежит он через применение взаимной рекурсии. Перед тем, как двигаться дальше, мы должны вновь изменить точку зрения на задачу и посмотреть на нее под другим углом, на этот раз  с точки зрения _теории формальных языков_.

  

Поставим задачу немного иначе. У нас есть некое выражение, записанное в виде цепочки символов. Мы должны разобрать это выражение и, если оно правильное (смысл этого понятия уточним чуть позже), вычислить его значение.

В такой постановке задача начинает напоминать разработку интерпретатора некоего языка программирования, пусть и несложного. Этот интерпретатор считывает программу, интерпретирует инструкции и, если их синтаксис корректен, выполняет одну за другой, пока программа не окажется выполненной. Тогда правильное выражение  такое, которое соответствует синтаксису этого самого языка.

Если мы решаем выбрать такое направление, наша задача разделяется на две, как и при реализации любого другого языка программирования:

-   Определить сам язык.
-   Разработать его транслятор (в нашем случае  интерпретатор).

Если разработка транслятора языка в наше время является уже довольно-таки рутинной задачей, хорошо формализованной и проработанной теоретически, то разработка нетривиального языка  задача намного более сложная и творческая. Впрочем, в нашем случае язык очень прост, поэтому муки творчества мы вряд ли успеем испытать. Однако для определения языка по всем правилам все же придется немного потрудиться. Но вначале не обойтись без азов теории формальных языков.

  

Рассмотрим множество каких-либо символов, например, кириллический алфавит. Выстраивая их в цепочку один за другим, можно составить слова, а из слов  осмысленные предложения русского языка. Любой знающий этот язык способен понять смысл фразы.

Однако далеко не каждое сочетание букв дает русское слово. Если комбинировать буквы хаотически, вероятность его получения не столь велика. Точно так же далеко не каждое сочетание слов дает синтаксически правильное предложение. (В данный момент мы не вдаемся в смысл, который несет это предложение, нас интересует лишь то, насколько формально правильным оно является; семантика  отдельный вопрос, которого мы касаться не будем).

Разумеется, в данный момент нас не интересует морфология живых языков, используемых людьми для общения; мы будем рассматривать исключительно формальные языки, в которых предложения строятся по относительно немногочисленным, четко определенным правилам.

Итак, для того, чтобы изъясняться на каком-либо языке, мы должны в первую очередь освоить его _алфавит_, то есть множество символов, из которых строятся фразы. Эти символы атомарны, то есть не могут быть разделены на какие-либо составные части; например, черточки, составляющие букву А, по отдельности никакого смысла не несут.

Но знание одного алфавита явно недостаточно, поскольку большинство цепочек символов бессмысленно с точки зрения языка. Правила, по которым строятся предложения языка из символов, называются _грамматикой_.

Итак, _язык_  это подмножество цепочек символов, принадлежащих его _алфавиту_, или предложений. Предложение принадлежит _языку_, если оно удовлетворяет правилам _грамматики_.

Таким образом, чтобы определить свой собственный язык, мы должны определить его алфавит и грамматику. Определить алфавит несложно: достаточно всего лишь перечислить все допустимые символы языка. С грамматикой сложнее: словесное описание нетривиальных грамматических правил может получиться громоздким и невразумительным, поскольку наш человеческий язык плохо приспособлен для подобной задачи.

К счастью, в теории формальных языков имеется строгий и изящный формализм для описания грамматики.

  

Определение формальной грамматики включает в себя 4 основные части:

**G(Vn, Vt, P, S)**

Здесь:

**G** - грамматика;

**Vn** - множество _нетерминальных_ символов;

**Vt** - множество _терминальных_ символов;

**P** - множество _правил продукции_;

**S** - _начальный_ символ.

Проясним смысл каждого из этих понятий.

_Терминальный_ символ - это по сути синоним атомарного, то есть символ, который входит в алфавит языка и не подлежит дальнейшему разложению на составные части. Не следует путать символ в понимании формальной грамматики с тем смыслом, который обычно вкладывается в него программистами: это не обязательно литера. Например, ключевое слово **begin** языка программирования Pascal - это тоже символ, причем терминальный, неделимый на отдельные литеры.

_Нетерминальный_ символ - это вспомогательное понятие, он вводится исключительно для описания неких категорий языка, которые в результате преобразований превращаются в цепочки терминальных. В предложениях языка терминальные символы не встречаются.

_Правило продукции_ - это правило вида **N->X**, где **N** - нетерминальный символ, **X** - последовательность терминальных и нетерминальных символов. (Вообще говоря, есть более общее представление правил продукции, но в данном случае мы будем иметь дело исключительно с контекстно-свободными грамматиками, поэтому такое упрощение вполне допустимо; вдаваться в тонкости классификаций грамматик мы в этой статье не будем, просто примите это примечание к сведению).

_Начальный_ символ - один из нетерминальных символов, который особым образом выделен из всего множества. Именно с него начинается построение предложения данного языка.

  

Для описания синтаксиса формальных языков (а фактически - правил продукции) великим ученым, внесшим огромный вклад в развитие информатики, Джоном Бэкусом был разработан специальный метаязык, который в его честь был назван Нормальной Формой Бэкуса (БНФ, Backus Normal Form, BNF). Этот метаязык довольно прост. Он содержит небольшое число метасимволов - "<", ">", "|", "::=". (К сожалению, в те времена про WWW еще и слыхом не слыхивали, поэтому Бэкус и не предполагал, какие проблемы вызовет публикация описаний формальных языков посредством HTML, который считает угловые скобки своей собственностью).

Метасимвол "::=" означает "есть по определению". Он отделяет нетерминальный символ (или короче - нетерминал) от его определения.

Угловые скобки заключают в себя нетерминал. Если вы видите конструкцию вида <nonterm>, это значит, что символ nonterm является нетерминалом, и вы наверняка найдете его где-то в левой части одного из правил продукции.

Метасимвол "|" разделяет альтернативные варианты определения нетерминала. Например, определение вида:

<знак-числа> ::= + | -

означает, что в любом месте, где встречается нетерминал "знак-числа", его можно заменить либо плюсом, либо минусом.

Попробуем воспользоваться БНФ и определить с ее помощью какую-нибудь несложную синтаксическую категорию. Например, нам для нашего интерпретатора потребуется работа с действительными числами. Итак:

<number> ::= <unsigned-number> | <sign> <unsigned-number>

Эта конструкция означает, что число может либо не иметь знака вовсе (быть беззнаковым), либо состоять из знака, за которым следует беззнаковое число.

Определить категорию знака числа совсем несложно:

<sign> ::= + | -

Категория "беззнаковое число" посложнее:

<unsigned-number> ::= <int-part> | <int-part> . <fract-part>

Мы определили, что беззнаковое число состоит либо из одной только целой части, либо из целой части, за которой следуют точка и дробная часть. Разумеется, от такого определения немного толку, если не определены нетерминалы, которые мы использовали в его правой части. Продолжаем:

<int-part> ::= <digits>

<fract-part> ::= <digits>

И целую, и дробную часть мы определили как последовательности цифр:

<digits> ::= <digit> | <digit> <digits>

На последнее определение стоит обратить внимание. Если сформулировать его неформальным языком, оно звучит следующим образом: "Последовательность цифр - это либо цифра, либо цифра, за которой следует последовательность цифр". Как видим, и здесь без рекурсии не обошлось...

И напоследок определим, что есть цифра:

<digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

Очевидно, что под определение <number> попадают целые и действительные числа, со знаком и без такового, у которых целая часть и дробная (если она есть) содержат как минимум по одной цифре.

Аналогичным образом можно описать практически любую конструкцию формального языка. Правда, описание это лаконичным не назовешь. Если уж такая несложная категория, как действительное число, потребовала 7 определений, то достаточно сложная конструкция может оказаться куда более громоздкой. Однако ради строгости и непротиворечивости БНФ с этим недостатком можно смириться.

  

Когда БНФ была применена в знаменитом (можно сказать, судьбоносном для информатики) проекте разработки языка Algol-60 (который для программистов является тем же, что и латынь для медиков), руководитель проекта Питер Наур решил избежать громоздкости, дополнив и расширив нотацию новыми элементами. Новая нотация получила название Расширенной Формы Бэкуса-Наура (РБНФ).

Строго говоря, в литературе встречается несколько сходных нотаций, и каждая из них именуется РБНФ. Видимо, нотация окончательно сформировалась не сразу, и промежуточные формы продолжают сосуществовать.

Изменение нотации коснулось следующих аспектов:

-   Исчезли метасимволы в виде угловых скобок, теперь нетерминалы выделяются курсивом (за что Науру огромное человеческое спасибо от всех, кто публикует статьи в HTML).
-   Добавлены метасимволы в виде квадратных скобок "\[" и "\]". Элементы, заключенные в квадратные скобки, являются необязатель;ными, т.е. могут быть пропущены.
-   Добавлены метасимволы в виде фигурных скобок "{" и "}". Элементы, заключенные в фигурные скобки, могут повторяться произвольное число раз (в том числе ни разу).

Есть варианты РБНФ, в которых метасимвол "::=" заменен простым знаком равенства, но я не считаю эту замену хорошей идеей, поскольку экономия двух литер может привести к путанице, и мы такой вариант использовать не будем.

Разумеется, сами метасимволы также могут встретиться в конструкциях описываемого языка. В этих случаях они берутся в кавычки. Надеюсь, путаницы это не вызовет, тем более что в нашем простейшем примере грамматики они не встретятся.

В заключение следует отметить, что пробелы, разделяющие описания элементов языка, не являются частью конструкций самого языка. Так, в предыдущем примере <digit> <digits> не означает, что цифры в последовательности разделены пробелами.

Итак, посмотрим, что нам дает подобное расширение. Повторим описание той же категории "число" новыми средствами:

_number_ ::= \[sign\] _int-part_ \[._fract-part_\]  
_sign_ ::= + | -  
_int-part_ ::= _digits_  
_fract-part_ ::= _digits_  
_digits_ ::= _digit_ {_digit_}  
_digit_ ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

Описание стало явно компактнее и явно проще для чтения.

Конечно, наша обзорная экскурсия в теорию формальных языков была весьма поверхностной. Тем не менее, полученных сведений должно хватить для решения нашей нехитрой задачи.

  

Сделаем еще одно (на этот раз последнее) уточнение поставленной задачи:

Необходимо разработать интерпретатор арифметических выражений, которые записываются по следующим правилам:

-   Выражение представляет собой сумму слагаемых.
-   Слагаемое представляет собой произведение сомножителей.
-   Сомножитель представляет собой число либо выражение, заключенное в скобки.

Определим входной язык интерпретатора.

  

Алфавит нашего языка незатейлив. Он включает в себя:

-   Десятичные цифры "0", "1", "2", "3", "4", "5", "6", "7", "8", "9".
-   Разделитель дробной части - десятичную точку ".".
-   Знаки арифметических операций - "+", "-", "\*", "/".
-   Круглые скобки для группировки подвыражений - "(" и ")".

Никакие другие символы в нашем языке употребляться не должны.

  

Синтаксис можно выразить посредством РБНФ следующим образом:

-   _expr_ ::= _item_ { _op-add item_ }
-   _op-add_ ::= + | -
-   _item_ ::= _factor_ { _op-mul factor_ }
-   _op-mul_ ::= \* | /
-   _factor_ ::= _number_ | ( _expr_)
-   _number_ ::= _int-part_ \[ ._fract-part_ \]
-   _int-part_ ::= _digits_
-   _fract-part_ ::= _digits_
-   _digits_ ::= _digit_ { _digit_ }
-   _digit_ ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

Комментарии:

-   Выражение представляет собой слагаемое, после которого может быть произвольное количество операций-типа-сложения, за каждой из которых следует слагаемое.
-   Операция-типа-сложения может быть сложением либо вычитанием.
-   Слагаемое представляет собой сомножитель, после которого может быть произвольное количество операций-типа-умножения, за каждой из которых следует сомножитель.
-   Операция-типа-умножения может быть умножением либо делением.
-   Сомножитель представляет собой либо число, либо выражение, заключенное в круглые скобки.
-   В оставшихся правилах продукции содержится уже хорошо знакомое нам описание категории "число".

Теперь в нашем распоряжении есть все 4 части, составляющие полное определение грамматики:

-   Терминальные символы перечислены в разделе "Алфавит калькулятора".
-   Нетерминальные символы перечислены слева от метасимвола "::=" в разделе "Синтаксис калькулятора".
-   Правила продукции перечислены в разделе "Синтаксис калькулятора".
-   Начальным символом нашей грамматики является метасимвол "выражение" ("expr").

Мы основательно потрудились, потратив немало сил и времени на детализацию задачи и на ее формальную спецификацию. Собственно, то, чем мы занимались до сих пор, нельзя назвать программированием в общепринятом смысле этого слова. Скорее, это был анализ задачи (пожалуй, чересчур тщательный и детальный для столь скромной задачи, но наша цель сейчас - обучение, а не конечный результат, поэтому подробности здесь не будут лишними).

Однако теперь, приступив к этапу написания самой программы, мы увидим, насколько легко и быстро будет осуществляться кодирование, каким ясным и компактным получится код и, главное, как легко будет при необходимости его модифицировать (например, если возникнет потребность добавить дополнительные функции к нашему калькулятору).

К слову сказать, во многих фирмах, занимающихся промышленным производством софта, само слово программист фактически является синонимом слова кодер и не является ни престижной, ни высокооплачиваемой, поскольку написать код по подробной спецификации - задача немногим сложнее той, что выполняют компиляторы. Зачастую эту работу выполняют люди со средним техническим образованием, и творчества в их труде немногим больше, чем в труде бухгалтера, кропотливо сводящего дебет с кредитом.

Собственно, именно к такой работе мы сейчас и приступим.

  

Хотя это и не было упомянуто в нашем кратком обзоре теории формальных языков, здесь уместно было бы упомянуть, что разработанный нами язык относится к 3-му типу по классификации Хомского. Это довольно важно, потому что для синтаксического разбора подобных языков достаточно иметь сканер с предпросмотром на 1 символ вперед. При условии наличия такого сканера разбор предложения языка можно осуществить за один проход без возврата. (Забегая вперед, скажу, что сначала я заготовил впрок именно такой сканер; однако грамматика оказалась столь проста, что даже просмотр на 1 символ вперед оказался избыточным, и я впоследствии удалил эту функцию из интерфейса класса сканера).

Итак, начнем работу с реализации сканера.

  

Итак, наш сканер должен давать нам возможность посимвольно просматривать выражение только вперед, при этом сигнализируя о попытке чтения после конца входной строки. Для выполнения этой задачи создадим заголовочный файл **Scanner.h**:

class CScanner  
{  
private:  
  const char \*m\_pCurrent; // буфер для хранения выражения  
public:  
  CScanner(const char \*pc);  
  bool IsEmpty(void) const; // проверка буфера на пустоту  
  char Current(void) const; // текущий символ буфера  
  void MoveNext(void);  // переход к следующему символу буфера  
}; // CScanner  

Данный класс включает в себя конструктор, позволяющий при создании сканера привязать его к интерпретируемому выражению, пару функций, позволяющих проверить остаток интерпретируемой строки на пустоту и извлечь текущий символ, и метод, позволяющий продвинуться на один символ вперед.

Реализация класса находится в файле **Scanner.cpp**. Она столь незатейлива, что вряд ли нуждается в комментариях даже для начинающего программиста:

#include "Buffer.h"

CScanner::CScanner(const char \*pc)  
{  
  m\_pCurrent = pc;  
}

bool CScanner::IsEmpty(void) const  
{  
  return !(\*m\_pCurrent);  
}

char CScanner::Current(void) const  
{  
  return \*m\_pCurrent;  
}

void CScanner::MoveNext(void)  
{  
  m\_pCurrent++;  
}

Теперь мы почти готовы к написанию самого интерпретатора. Почти - потому, что нам нужно разобраться еще с одним назойливым, но немаловажным вопросом - обработкой ошибок.

  

Поскольку практически наверняка пользоваться нашим калькулятором будет конечный пользователь программы, в которую он будет встроен, и поскольку людям свойственно ошибаться, необходимо встроить в наш калькулятор средства для обработки ошибок.

Самое простое решение - это завершать выполнение программы с соответствующей диагностикой. Но такой подход годится лишь для учебной программы, да и то с большой натяжкой. Если наш калькулятор окажется встроен в большой пакет программ для финансовых расчетов и после часа работы программа из-за опечатки при вводе вылетит, выдав на прощание что-то невразумительное вроде неожиданно достигнут конец строки, вряд ли это будет хорошей рекламой нашему софту.

В современных языках программирования есть хорошая альтернатива данному подходу. Если при работе приложения какая-либо подпрограмма обнаруживает ошибку (например, делимое равно нулю), она вызывает исключение. Если вызывающая программа знает, как обрабатывать такую исключительную ситуацию, она перехватывает это исключение и обрабатывает ошибку, например, предлагая пользователю исправить вводимую строку. Если вызывающая программа не собирается обрабатывать исключение, оно передается по стеку вызовов на один уровень выше до тех пор, пока либо на каком-то уровне исключение не будет перехвачено и обработано, либо стек вызовов не будет исчерпан. В последнем случае исполняющая система завершит выполнение программы с соответствующей диагностикой, ибо дальнейшее продолжение работы в такой ситуации бессмысленно.

Такой подход намного конструктивнее. Если наш интерпретатор найдет в выражении символ, недопустимый в данном контексте, будет сгенерировано исключение, которое может быть перехвачено пользовательской программой и обработано.

Для того, чтобы диагностика в случае ошибки была более вразумительной, имеет смысл генерировать не одно исключение на все случаи жизни, а пакет родственных исключений, каждое из которых соответствует определенной проблеме ввода. В этом случае автор программы имеет выбор: ограничиться скупой констатацией факта ошибки или сделать детальную диагностику.

Все исключения калькулятора перечислены в файле **Error.h:**

// общая ошибка  
class CCalcError  
{  
public:  
  CCalcError() {}  
  ~CCalcError() {}  
  virtual const char \* Reason(void) const { return "Calc - Expression calculation error"; }  
};

// ожидается конец строки  
class CCalcEndOfLineExpErr: public CCalcError  
{  
private:  
  char cBadChar;  
public:  
  CCalcEndOfLineExpErr(char c) { cBadChar = c; }  
  ~CCalcEndOfLineExpErr() {}  
  virtual const char \* Reason(void) const { return "Calc - End of line expected"; }  
  const char BadChar(void) const { return cBadChar; }  
};

// ожидается цифра или открывающая круглая скобка  
class CCalcDigitOrOpenExpErr: public CCalcError  
{  
private:  
  char cBadChar;  
public:  
  CCalcDigitOrOpenExpErr(char c) { cBadChar = c; }  
  ~CCalcDigitOrOpenExpErr() {}  
  virtual const char \* Reason(void) const { return "Calc - Digit or '(' expected"; }  
  const char BadChar(void) const { return cBadChar; }  
};

// ожидается цифра  
class CCalcDigitExpErr: public CCalcError  
{private:  
  char cBadChar;  
public:  
  CCalcDigitExpErr(char c) { cBadChar = c; }  
  ~CCalcDigitExpErr() {}  
  virtual const char \* Reason(void) const { return "Calc - Digit expected"; }  
  const char BadChar(void) const { return cBadChar; }  
};

// ожидается открывающая круглая скобка  
class CCalcCloseExpErr: public CCalcError  
{  
private:  
  char cBadChar;  
public:  
  CCalcCloseExpErr(char c) { cBadChar = c; }  
  ~CCalcCloseExpErr() {}  
  virtual const char \* Reason(void) const { return "Calc - ')' expected"; }  
  const char BadChar(void) const { return cBadChar; }  
};

// буфер строки пуст  
class CCalcBufEmptyErr: public CCalcError  
{public:  
  CCalcBufEmptyErr() {}  
  ~CCalcBufEmptyErr() {}  
  virtual const char \* Reason(void) const {return "Calc - Buffer is empty"; }  
};

// деление на нуль  
class CCalcDivByZeroErr: public CCalcError  
{public:  
  CCalcDivByZeroErr() {}  
  ~CCalcDivByZeroErr() {}  
  virtual const char \* Reason(void) const {return "Calc - Division by zero"; }  
};

Обратите внимание, что все классы исключения, кроме **CCalcError**, являются производными от **CCalcError**. Это позволяет программе, которая не желает вдаваться в тонкости ошибки, обрабатывать это исключение, общее для всех ошибок. Кроме того, можно поставить перехват этого исключения после перехватчиков всех дочерних ошибок; это может оказаться полезным в случае, если вы забудете перехватить какую-либо из них. Хоть в этом случае вызывающая программа не сможет получить детальную информацию об ошибке, тем не менее аварийного завершения не произойдет.

Все классы исключений имеют виртуальную функцию **Reason**, которая возвращает текстовое описание причины ошибки. Кроме того, в тех случаях, когда это уместно, исключение возвращает также символ **BadChar**, который представляет тот самый символ, который оказался причиной данной ошибки.

Теперь, закончив со вспомогательными классами, можно приступать к написанию самого интерпретатора.

  

Интерфейс интерпретатора незатейлив - он представляет собой единственную функцию, которая получает при вызове сканер, в который заряжена строка с исходным выражением. Эта функция возвращает результат типа **double**, который является результатом вычисления выражения (разумеется, при условии, что выражение корректно, точнее - соответствует описанной нами грамматике; в противном случае возникает одно из исключений, описанных в **Error.h**).

Содержимое **Calc.h**:

/\*Программа-калькулятор\*/  
#include "Scanner.h"

extern double GetValue(CScanner &b); // возвращает значение выражения

Реализация интерпретатора (**Calc.cpp**):

#include <ctype.h>  
#include "Calc.h"  
#include "Error.h"

// вспомогательная функция - преобразование литеры в число  
int CharToInt(const char c)  
{  
  // проверяем, является ли литера цифрой  
  if (!isdigit(c)) // если нет, вызываем исключение  
    throw CCalcDigitExpErr(c);  
  // цифра - вычисляем значение  
  return (c - '0');  
} // CharToInt

// обработка числа  
double GetNumber(CScanner &b)  
{  
  // проверяем, не пуст ли буфер  
  if (b.IsEmpty())  
    throw CCalcBufEmptyErr();  
  // проверяем, является ли текущая литера цифрой  
  if (!isdigit(b.Current()))  
    throw CCalcDigitExpErr(b.Current());  
  // все в порядке - как и ожидалось, в буфере число  
  double resultValue = 0.0;  
  // считываем целую часть числа  
  while (isdigit(b.Current()))  
  {  
    resultValue \*= 10.0;  
    resultValue += CharToInt(b.Current());  
    b.MoveNext();  
  } // while  
  // проверяем наличие необязательной дробной части  
  if (b.Current() == '.')  
  {  
    // пропускаем десятичную точку  
    b.MoveNext();  
    // считываем дробную часть  
    // сначала убедимся, что после точки следует цифра  
    if (!isdigit(b.Current()))  
      throw CCalcDigitExpErr(b.Current());  
    double step = 1.0;  
    while (isdigit(b.Current()))  
    {  
      step /= 10.0;  
      resultValue += step \* CharToInt(b.Current());  
      b.MoveNext();  
    } // while  
  } // if  
  // возвращаем значение считанного числа  
  return resultValue;  
} // GetNumber

/\*  
  А теперь приступаем к первой функции, задействованной  
в цепочке взаимной рекурсии.  
  Поскольку сомножитель может быть выражением, а функция разбора  
выражения будет описана позже, в этом месте необходимо включить  
предварительное описание функции GetExpr (без реализации), чтобы избежать  
ошибки обращения к неопределенной функции при компиляции программы.  
\*/  
double GetExpr(CScanner &b);  
{  
// обработка сомножителя  
double GetFactor(CScanner &b)  
{  
  // сомножитель может быть числом,  
  if (isdigit(b.Current())) // в этом случае его значение = значению этого числа,  
    return GetNumber(b);  
  // либо выражением в скобках, в этом случае его следует вычислить  
  else if (b.Current() == '(')  
  {  
    // пропускаем открывающую скобку  
    b.MoveNext();  
    // получаем значение выражения  
    double resultValue = GetExpr(b);  
    // проверяем наличие закрывающей скобки  
    if (b.Current() != ')')  
      throw CCalcCloseExpErr(b.Current());  
    // пропускаем закрывающую скобку  
    b.MoveNext();  
    // возвращаем значение сомножителя  
    return resultValue;  
  } // else if  
  else // текущий символ - не число и не открывающая скобка  
    throw CCalcDigitOrOpenExpErr(b.Current());  
} // GetFactor

// обработка слагаемого  
double GetItem(CScanner &b)  
{  
  // получаем первый сомножитель  
  double resultValue = GetFactor(b);

    // проверяем, следует ли за ним операция типа умножения  
  while ((b.Current() == '\*') || (b.Current() == '/'))  
  {  
    // сохраняем литеру операции  
    char mulOp = b.Current();  
    b.MoveNext();  
    // получаем следующий сомножитель  
    double secondOp = GetFactor(b);  
    // выполняем операцию типа умножения  
    if (mulOp == '\*')  
      resultValue \*= secondOp;  
    else if (mulOp == '/')  
    {  
      // если операция - деление, следует проверить,  
      //   не равен ли нулю второй операнд  
      if (secondOp == 0.0)  
        throw CCalcDivByZeroErr();  
      // все в порядке - можно делить  
      resultValue /= secondOp;  
    } // else if  
  } // while  
  return resultValue;  
} // GetItem

// обработка выражения  
double GetExpr(CScanner &b)  
{  
  // получить первое слагаемое  
  double resultValue = GetItem(b);  
  // проверяем, следует ли за ним операция типа сложения  
  while ((b.Current() == '+') || (b.Current() == '-'))  
  {  
    // сохраняем литеру операции  
    char addOp = b.Current();  
    b.MoveNext();  
    // получаем следующее слагаемое  
    double secondOp = GetItem(b);  
    // выполняем операцию типа сложения  
    if (addOp == '+')  
      resultValue += secondOp;  
    else if (addOp == '-')  
      resultValue -= secondOp;  
  } // while  
  return resultValue;  
} // GetExpr

// единственная функция интерпретатора, видимая вызывающей программе  
double GetValue(CScanner &b)  
{  
  // вычисляем выражение, находящееся в сканере  
  double resultValue = GetExpr(b);  
  // если по завершении работы сканер не пуст,  
  //   значит, в конце выражения что-то неладно  
  if (!b.IsEmpty())  
    throw CCalcEndOfLineExpErr(b.Current());  
  // все в порядке - нормальное завершение, возвращаем значение выражения  
  return resultValue;  
} // GetValue

Полагаю, что при таком обилии комментариев вопросов по алгоритму работы интерпретаторов возникнуть не должно, поэтому на этом раздел заканчиваю.

  

Ну и в завершение всего, чтобы испытать новоиспеченный калькулятор в работе, дополним наш проект основной программой, которая позволит вводить строки из стандартного ввода, правильные и не очень, и посмотреть реакцию нашего интерпретатора на них:

// Cross.cpp : Defines the entry point for the console application.  
//

#include <stdio.h>  
#include "Calc.h"  
#include "Error.h"

int main(int argc, char\* argv\[\])  
{  
  char buff\[80\]; // буфер для хранения ввода пользователя  
  for (;;)  
  {  
    printf("Input string:\\n");  
    scanf("%s", buff);  
    CScanner Buffer(buff);  
    try  
    {  
      // вычислить значение введенного выражения  
      double resultValue = GetValue(Buffer);  
      printf("Result is: %10.5f\\n", resultValue);  
    } // try  
    // обработка ошибок  
    catch (CCalcEndOfLineExpErr E)  
    {  
      printf("ERROR! %s; found: %c\\n", E.Reason(), E.BadChar());  
    } // catch  
    catch (CCalcDigitOrOpenExpErr E)  
    {  
      printf("ERROR! %s; found: %c\\n", E.Reason(), E.BadChar());  
    } // catch  
    catch (CCalcDigitExpErr E)  
    {  
      printf("ERROR! %s; found: %c\\n", E.Reason(), E.BadChar());  
    } // catch  
    catch (CCalcCloseExpErr E)  
    {  
      printf("ERROR! %s; found: %c\\n", E.Reason(), E.BadChar());  
    } // catch  
    catch (CCalcBufEmptyErr E)  
    {  
      printf("ERROR! %s\\n", E.Reason());  
    } // catch  
    catch (CCalcDivByZeroErr E)  
    {  
      printf("ERROR! %s\\n", E.Reason());  
    } // catch  
    catch (CCalcError E)  
    {  
      printf("ERROR! %s\\n", E.Reason());  
    } // catch  
  } // for  
return 0;  
} // main

  

Попытка привести сколько-нибудь применимый на практике пример заставила меня помимо собственно косвенной рекурсии упомянуть вскользь еще целый ряд не менее любопытных тем: теорию формальных языков, нотации БНФ и РБНФ, построение транслятора языка, заданного посредством грамматики... Разумеется, в небольшой статье, к тому же посвященной другой теме, обо всем этом можно лишь упомянуть, не более того. Если у вас после прочтения данной статьи возникло желание более серьезно познакомиться с этими вопросами, пишите, ваше мнение я обязательно учту при выборе тематики будущих статей.

Все примеры, приведенные в статье, были разработаны и оттестированы в среде Visual C++ V6.0. Однако в них нет никакой привязки именно к этой версии, поэтому ожидается, что они будут работоспособны в любой среде программирования на C++. Кроме того, очевидно, что приведенный код без особых усилий может быть преобразован в программу на языках Pascal, Visual Basic, C# и др. Главное здесь - сама идея построения программы на базе взаимной рекурсии, позволяющая с легкостью построить гибкую программу для интерпретации входных данных, описанных средствами формальной грамматики.

Разумеется, программа не была доведена до совершенства, поскольку это всего лишь иллюстрация метода, а не самоцель. Например, в представлении числа не предусмотрен знак. Сканер можно было бы снабдить счетчиком символов, чтобы в сообщении об ошибке можно было точно указать место ее возникновения. Да и саму функцию интерпретатора можно было бы сделать членом класса сканера, поскольку объектные языки не позволяют вводить "свободные" функции, не принадлежащие классу.

То же самое относится и к терминологии, которая не была особенно строгой. Так, например, порой термин "символ" употреблялся как в своем значении из теории формальных языков, так и в более привычном для программистов значении в качестве синонима слова "литера". Надеюсь, эти вольности не вызовут у читателя путаницы, поскольку из контекста всегда понятно, какое значение термина имелось в виду.

Все усовершенствования программы вы можете выполнить в качестве самостоятельного упражнения, а я на теме косвенной рекурсии ставлю точку. Как всегда, жду ваших вопросов, замечаний, предложений и т.д. в своем разделе форума.

В очередной раз возьму на себя грех и не приведу список литературы, поскольку никакой литературой при подготовке статьи не пользовался. Если кто-то из читателей все же изъявит желание самостоятельно поглубже ознакомиться с какой-либо из затронутых тем, обращайтесь, попробую помочь, чем смогу.

___

**Alf, 22 сентября 2004**
---
created: 2022-12-23T07:23:56 (UTC +03:00)
tags: [статья,]
source: https://club.shelek.ru/viewart.php?id=226
author: 
---

# Статья: "Заметки о рекурсии. Приложение 1."

> ## Excerpt
> Клуб программистов "Весельчак У". Статья:  "Заметки о рекурсии. Приложение 1."

---
В статье [Заметки о рекурсии  3](https://club.shelek.ru/viewart.php?id=220) я упомянул о БНФ и РБНФ, а также об их значении для описания синтаксиса формальных языков. К сожалению, ограниченный объем статьи не позволил углубиться в эту тему, поэтому она была освещена весьма поверхностно.

После выхода статьи я начал получать письма с просьбами ответить на ряд вопросов, касающихся некоторых деталей данных нотаций. По возможности я стараюсь на них отвечать, но, во-первых, не всегда на подготовку продуманного ответа хватает времени, во-вторых, ответ на поставленный вопрос видит лишь автор вопроса и никто более, тогда как он мог бы быть интересен более широкой аудитории.

Учитывая чрезвычайную важность владения БНФ и РБНФ для профессионального программиста, я решил вернуться к этому вопросу снова.

В упомянутой выше статье я использовал диалект РБНФ, широко используемый Н. Виртом в его работах (и по этой причине наиболее мне знакомый, поскольку мой научный руководитель придерживался школы Вирта). В данной статье описан другой диалект, на мой взгляд, не менее интересный и выразительный.

Это - краткая статья, в которой я пытаюсь пояснить, что же такое БНФ, на основе сообщения **[wkwwagbizn.fsf@ifi.uio.no](mailto:wkwwagbizn.fsf@ifi.uio.no),** отправленного в конференцию **comp.text.sgml** 16 июня 1998. Она представляет собой черновой набросок, поэтому, если вы получили ответы не на все интересующие вопросы, напишите мне, и я постараюсь объяснить, как смогу.

С тех пор она была существенно дополнена и изрядно прибавила в объеме. Впрочем, не тревожьтесь. По мере чтения статья становится все более детальной, поэтому, если вы не намереваетесь закапываться в эту тему глубоко, просто прервите чтение в момент, когда на все интересующие вас вопросы получен ответ и уже начинает становиться скучно.

Нотация Бэкуса-Наура (больше известная как БНФ или Форма Бэкуса-Наура) - формальный математический прием для описания языка, разработанный Джоном Бэкусом (а также, возможно, Питером Науром) для описания синтаксиса языка программирования Algol 60.

(Легенда гласит, что она была вначале разработана Джоном Бэкусом на основе более ранних работ математика Эмиля Поста, а затем заимствована и несколько улучшена Питером Науром для Algol 60, что принесло ей широкую известность. Поэтому Наур называет БНФ Нормальной Формой Бэкуса, тогда как все остальные называют ее Формой Бэкуса-Наура).

Она используется для формального определения грамматики языка таким образом, что не остается разногласий или неоднозначности касательно того, что является допустимым, а что нет. На самом деле БНФ настолько однозначна, что вокруг подобных грамматик было построено немало математических теорий, и в действительности можно механически построить парсер (модуль синтаксического разбора - **прим. пер.**) для языка, задав для него грамматику в виде БНФ. (Существуют некоторые виды грамматик, для которых это невозможно, однако обычно их можно вручную преобразовать к пригодному для этого виду).

Программы, которые делают это, обычно называются "компиляторами компиляторов". Наиболее известен среди них YACC, однако есть и немало других.

БНФ несколько напоминает математическую игру: вы начинаете с символа (называемого _начальным символом_ и по соглашению обычно обозначаемого в примерах **S**), а затем вы получаете набор правил, указывающих, чем вы можете заменить этот символ. Язык, определяемый грамматикой БНФ,  это всего лишь набор всех строк, которые вы можете получить, следуя этим правилам.

Правила называются _правилами продукции_ и выглядят следующим образом:

  символ := альтернатива1 | альтернатива2 ...  

Правило продукции просто устанавливает, что символ слева от ":=" должен быть заменен одной из альтернатив в правой части. Альтернативы разделяются знаком "|". (В одном из вариантов используется знак "::=" вместо ":=", но смысл его тот же). Альтернативы обычно состоят из символов и так называемых терминалов. Терминалы  это просто фрагменты конечной строки, не являющиеся символами. Они называются терминалами, потому что для них нет правил продукции: они завершают процесс построения строк. (Символы часто называют нетерминалами).

(**Примечание переводчика.** В отечественной литературе по теории формальных языков обычно используется подобная, но несколько другая терминология. Терминалы также называют _терминальными_ символами, а нетерминалы  _нетерминальными символами_. Таким образом, и те, и другие являются _символами_ данной грамматики).

Еще одна вариация грамматики БНФ заключает терминалы в кавычки, чтобы отличать их от символов. Некоторые грамматики БНФ явно указывают, где допустимы пробелы, зарезервировав для этого специальный символ, тогда как другие грамматики предоставляют догадываться об этом самому читателю.

В БНФ имеется специальный символ "@", который означает, что символ может быть удален. Если вы заменяете символ на "@", вы просто удаляете символ. Это полезно, поскольку иногда трудно завершить процесс замещения, не используя этот трюк.

Таким образом, язык, описываемый грамматикой, представляет собой набор всех строк, которые вы можете вывести при помощи правил продукции. Если строка никоим образом не может быть выведена с использованием правил, строка не является допустимой в языке.

  

Ниже представлена простая грамматика БНФ:

    S  := ';-' FN | FN

    FN := DL | DL '.' DL

    DL := D | D DL

    D  := '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'

Все символы здесь  сокращения: **S** - начальный символ (start symbol), **FN** - число с дробной частью (fractional number), **DL** - список цифр (digit list), **D** - цифра (digit).

Допустимые предложения языка, описываемого данной грамматикой, - это все числа, возможно, дробные, возможно, отрицательные. Чтобы построить число, начинаем с начального символа:

Затем заменяем символ **S** одной из его продукций. Мы решили не ставить знак "-" перед числом, так что используем простую продукцию **FN** и заменяем **S** на **FN**:

Следующий шаг - заменить символ **FN** на одну из его продукций. Мы хотим получить число с дробной частью, поэтому выбираем продукцию, которая создает две цепочки десятичных цифр с точкой между ними, а затем продолжаем выбирать замену символа на одну из его продукций по одной в строке (см. пример):

    DL . DL

    D . DL

    3 . DL

    3 . D DL

    3 . D D

    3 . 1 D

    3 . 1 4

Здесь мы вывели дробное число 3.14. Как вывести число -5, оставим в качестве упражнения читателю. Чтобы убедиться, что вы поняли изложенное, изучите грамматику настолько, чтобы понять, что строка 3..14 не может быть выведена с использованием этих правил продукции.

  

Для описания **DL** мне пришлось использовать рекурсию (**DL** может произвести новые **DL**), чтобы выразить факт, что можно использовать любое количество **D**. Это не совсем удобно и делает БНФ не слишком удобной для чтения. Расширенная БНФ (разумеется, РБНФ) решает эту проблему добавлением трех операторов:

-   ? означает, что символ (или группа символов в скобках) слева от оператора является необязательной (может встретиться 0 либо 1 раз);
-   \* означает, что нечто может повторяться произвольное число раз (а также может быть опущено);
-   \+ означает, что нечто может встретиться 1 или более раз.

С использованием РБНФ грамматика из предыдущего примера может быть записана следующим образом:

    S := '-'? D+ ('.' D+)?

    D := '0' | '1'| '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'

что выглядит куда симпатичнее.

На заметку: РБНФ не мощнее, чем БНФ, в смысле языков, которые она может определить, а просто удобнее. Любая продукция РБНФ может быть преобразована в эквивалентный набор продукций БНФ.

  
  

Большинство стандартов языков программирования используют какую-либо разновидность РБНФ для определения грамматики языка. Такой подход дает два преимущества: не может быть разногласий относительно того, что представляет собой синтаксис языка, и гораздо проще построить компилятор, поскольку парсер может быть сгенерирован автоматически компилятором компиляторов вроде YACC.

РБНФ используется также во многих других стандартах, таких как определение форматов протокола, форматов данных и языков разметки вроде XML и SGML. (HTML не определяется грамматикой, вместо этого он определен через SGML DTD, что представляет собой разновидность грамматики более высокого уровня).

Вы можете ознакомиться с собранием грамматик БНФ в [BNF web club](http://cuiwww.unige.ch/db-research/Enseignement/analyseinfo/BNFweb.html).

  

Теперь вы знаете, что такое БНФ и РБНФ, для чего они используются, но, возможно не знаете, почему они столь полезны или как извлечь из них пользу.

Наиболее очевидный способ использования формальной грамматики уже был упомянут мимоходом: задав формальную грамматику для языка, вы полностью определяете его. Не может быть разногласий, что дозволено в языке, а что нет. Это весьма полезно, поскольку обычное словесное описание языка значительно многословнее и подвержено неоднозначной интерпретации.

Другое достоинство: формальные грамматики - математические конструкции и могут быть поняты компьютером. Имеется масса программ, которым можно задать не входе (Р)БНФ и автоматически получить код парсера для данной грамматики. На самом деле это самый обычный способ изготовления компилятора: использование так называемого компилятора компиляторов, который принимает грамматику на входе и производит код парсера на каком-то языке программирования.

Разумеется, компиляторы делают гораздо больше проверок, чем только проверка грамматики (например, проверку типов), и они также генерируют код. Эти аспекты не описываются грамматикой (Р)БНФ, поэтому компиляторы компиляторов обычно включают специальный синтаксис для связывания фрагментов кода (называемых действиями) с различными грамматическими конструкциями.

Наиболее известный компилятор компиляторов  это YACC (Yet Another Compiler Compiler), который производит код на языке C, но существуют также и другие для C++, Java, Python и многих других языков.

  
  
  

Самый простой способ синтаксического разбора в соответствии с грамматикой, используемый в настоящее время, называется LL-парсингом (или разбором сверху-вниз). Он работает следующим образом: для каждой конструкции определяем, с каких нетерминалов может начинаться данная конструкция (они называются стартовым набором).

Затем при разборе мы начинаем с начального символа и сравниваем стартовые наборы различных продукций с первым элементом ввода, определяя, какая из продукций может быть использована. Разумеется, это не может быть сделано в том случае, если два или более стартовых набора для одного символа содержат один и тот же терминал. В этом случае нет возможности определить, какую продукцию выбрать, располагая первым терминалом на вводе.

LL-грамматики часто классифицируются по номерам: LL(1), LL(0) и т.д. Число в скобках указывает максимальное количество терминалов, которое вам требуется видеть одновременно, чтобы выбрать правильную продукцию в любом месте грамматики. Так, для LL(0) вам вообще не нужно просматривать терминалы, вы всегда можете выбрать правильную продукцию. Это возможно лишь в случае, если все символы имеют лишь одну продукцию, и если они имеют лишь одну продукцию, язык может включать лишь одну строку. Другими словами, грамматики LL(0) не представляют интереса.

Наиболее типичный (и полезный) класс LL-грамматик  это LL(1), в которой вы всегда можете выбрать правильную продукцию, видя лишь первый терминал ввода в каждый момент времени. Для LL(2) вам необходимо видеть два символа, и т.д. Существуют грамматики, не являющиеся LL(k) для любого заданного значения k, и к сожалению, они довольно часто встречаются.

  

В качестве примера давайте проанализируем стартовый набор для грамматики, приведенной выше. Для символа D это просто: все продукции имеют одну цифру в качестве стартового набора, и стартовый набор символа D представляет все 10 цифр. Это значит, что наша грамматика в лучшем случае LL(1), т.к. в этом случае нам нужно просмотреть один терминал для выбора правильной продукции.

С DL натыкаемся на неприятность. Обе продукции начинаются с D, поэтому обе имеют один и тот же стартовый набор. Это значит, что, глядя лишь на первый терминал ввода, нельзя выбрать правильную продукцию. Впрочем, мы легко можем обойти эту проблему, применив небольшую хитрость: если второй терминал ввода  не цифра, мы должны выбрать первую продукцию, а если оба они  цифры, мы должны выбрать вторую. Другими словами, в лучшем случае это  грамматика LL(2).

Фактически здесь я несколько упростил ситуацию. Продукция для DL сама по себе не говорит нам, какие терминалы допустимы после первого терминала в продукции D @, поскольку нам нужно знать, какие терминалы допустимы после символа DL. Этот набор терминалов называется последующим набором символа, и в нашем случае это "." и конец ввода.

Для символа FN все оказывается даже еще хуже, поскольку обе продукции имеют все цифры в качестве стартового набора. Просмотр второго терминала не помогает, поскольку нам нужно видеть первый терминал после последней цифры в списке цифр (DL), а мы не знаем, сколько в нем цифр, пока не считаем их все. И поскольку не задан предел возможного количества цифр, это не грамматика LL(k) для любого заданного k (какое бы k мы ни выбрали, все равно цифр может быть больше k).

Как это ни странно, символ S прост. Первая продукция имеет "-" в качестве стартового набора, вторая  все цифры. Другими словами, когда вы начинаете разбор, вы начинаете с символа S и смотрите на ввод, чтобы решить, какой продукцией воспользоваться. Если первый терминал "-", вы знаете, что используется первая продукция. Если нет, используется вторая. (Здесь в оригинале ошибка - разумеется, вторая продукция может быть применена, лишь если на входе - одна из цифр - **прим. перев.**) Только продукции FN и DL вызывают проблемы.

  

Впрочем, не стоит терять надежду. Большинство грамматик, не являющихся LL(k), могут быть довольно легко преобразованы к грамматикам LL(1). В нашем случае нам потребуется изменить два символа: FN и DL.

Проблема с FN заключается в том, что обе продукции начинаются с DL, однако вторую продолжает "." и еще один DL после первого DL. Это легко решается: заменим FN таким образом, чтобы он состоял из одной продукции, которая начинается с DL, за которой следует FP (fractional part), где FP  либо пустая строка, либо ".", за которой следует DL:

    FN := DL FP

    FP := @ | '.' DL

Теперь у нас больше нет проблем с FN, поскольку имеется лишь одна продукция, и FP также не представляет проблем, поскольку две продукции имеют различные стартовые наборы, соответственно конец ввода и ".".

DL - более крепкий орешек, поскольку проблема состоит в рекурсии и обусловлена фактом, что нам нужен как минимум один D для получения DL. Решение - задать для DL единственную продукцию, D, за которым следует DR (digit rest). DR имеет две продукции: D DR или @. Стартовый набор первой продукции - набор всех цифр, второй - "." и конец ввода, так что проблема решена.

Вот полная LL(1)-грамматика, полученная в результате преобразования:

    S  := '-' FN | FN

    FN := DL FP

    FP := @ | '.' DL

    DL := D DR

    DR := D DR | @

    D  := '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'

  
  

Более трудный способ синтаксического разбора известен как "редукция со сдвигом", или "разбор снизу-вверх". При этом подходе читается ввод до тех пор, пока не выяснится, что входную последовательность можно свести к символу. Возможно, это звучит сложновато, поэтому приведу пример для пояснения. Мы разберем строку 3.14 и увидим, как она может быть выведена из грамматики. Начинаем с чтения 3 из ввода:

а затем попробуем свести ее к символу, из которого она выведена. Это нетрудно, она выведена из символа D, которым мы заменяем 3. Затем мы замечаем, что можем вывести D из DL, и заменяем D на DL. (Грамматика неоднозначна, это значит, что мы можем свести сначала к FN, что будет неправильно. Для простоты мы опускаем здесь неверные шаги, но однозначная грамматика не позволила бы сделать неверный выбор). Затем мы считываем "." из ввода и пытаемся свести ее, но тщетно:

Эта последовательность ни к чему не сводится, поэтому читаем очередной символ из ввода: 1. Затем сводим его к D и читаем следующую литеру 4. Она может быть сведена к D, затем к DL, затем последовательность D DL можно свести к DL:

    DL .

    DL . 1

    DL . D

    DL . D 4

    DL . D D

    DL . D DL

    DL . DL

Взглянув на это грамматику, мы тут же замечаем, что из FN выводится эта самая последовательность "DL . DL", и выводим ее. Затем замечаем, что FN выводится из S, и сводим FN к S, завершая тем самым синтаксический разбор.

Как вы уже, наверное, заметили, мы часто стоим перед выбором, произвести ли редукцию к символу немедленно или подождать, пока у нас появится больше символов, и тогда произвести редукцию. Есть более сложные разновидности этого алгоритма разбора со сдвигом-редукцией, в порядке возрастания сложности и мощности: LR(0), SLR, LALR и LR(1). LR(1) обычно требует непрактично больших таблиц разбора, поэтому алгоритм LALR используется наиболее часто, поскольку мощности SLR и LR(0) оказывается недостаточно для большинства языков программирования.

LALR и LR(1) слишком сложны, чтобы рассматривать их в этой статье, но основную идею вы поняли.

  

На этот вопрос гораздо лучше меня уже ответил кто-то другой, поэтому я лишь полностью процитирую его сообщение в конференции:

  

Надеюсь, это не станет началом войны

Прежде всего  Фрэнк, если ты читаешь это, не бей меня сильно. (Мой начальник  Фрэнк ДеРемер, создатель LALR)

(Я позаимствовал эту сводку из Fischer&LeBlanc's "Crafting a Compiler")

Простота - LL

Общность - LALR

Действия - LL

Исправление ошибок - LL

Размер таблиц - LL

Скорость разбора  сравнима (от меня: и зависит от инструмента)

  

Работа парсера LL гораздо проще. И если вам приходится отлаживать парсер, просмотр парсера с рекурсивным спуском (обычный способ программирования LL-парсера) гораздо проще, чем таблицы парсера LALR.

  

Из-за простоты спецификации LALR легко побеждает. Большая разница между LL и LA(LR) состоит в том, что в LL-грамматике вы должны провести левую факторизацию правил и избавиться от левой рекурсии.

Левая факторизация необходима, поскольку LL-парсинг требует произвести выбор на основе ограниченного количества входных токенов.

Левая рекурсия проблематична, поскольку предпросмотр токена правила - это всегда предпросмотр токена того же самого правила. (Все, что содержится в множестве A, содержится в множестве A...) Это вынуждает правило рекурсивно обращаться к себе снова и снова...

Для многих языков уже доступны LALR-грамматики, поэтому вам придется преобразовывать. Если для языка нет доступной грамматики, на мой взгляд, не столь сложно самостоятельно написать LL-грамматику. (Нужно только быть в правильном "LL" настроении, обычно для этого нужно часов 8 посмотреть Dr. Who перед тем, как усесться за написание грамматики... Если вы еще не знаете, я действительно предпочитаю LL...)

  

Для LL-парсера вы можете поместить действия где угодно, не вызывая конфликта.

  

LL-парсеры имеют намного больше информации о контексте (поскольку они производят разбор сверху-вниз), и поэтому они могут быть намного полезнее для исправления ошибок, не говоря уже о сообщениях об ошибках.

  

Если вы пишете таблично-управляемый LL-парсер, его таблицы будут приблизительно вдвое меньше. (Откровенно говоря, есть способы оптимизации таблиц LALR, чтобы сделать их меньше, и я считаю их убдительными...)

  

От меня: и зависит от инструмента.

  Scott Stanchfield, статья [33C1BDB9.FC6D86D3@scruz.net](http://groups.google.com/groups?hl=en&selm=33C1BDB9.FC6D86D3%40scruz.net) в конференции comp.lang.java.softwaretools Mon, 07 Jul 1997.

John Aycock разработал замечательный и простой в использовании инструмент на языке Python под названием [SPARK](http://pages.cpsc.ucalgary.ca/~aycock/spark/), описанный в этой [очень легко читаемой статье](http://www.foretec.com/python/workshops/1998-11/proceedings/papers/aycock-little/aycock-little.html).

Бесплатная онлайн альтернатива, с виду неплохая,  вот [эта книга](http://www.cs.vu.nl/~dick/PTAPG.html), однако не могу комментировать ее качество, поскольку сам еще не прочитал.

Henry Baker написал [статью о парсинге на Common Lisp](http://home.pipeline.com/~hbaker1/Prag-Parse.html), который представляет простую, высокопроизводительную и весьма удобную среду для парсинга. Подход аналогичный применяемому в компиляторах компиляторов, но основывается на очень мощной системе макросов Common Lisp.

  
  

Приношу свои благодарности:

-   [Jelks Cabaniss](http://cpcug.org/user/jelks/) за то, что подал мысль превратить заметку в новостях в статью, а также за очень полезную критику.
-   C. M. Sperberg-McQueen за дополнительную историческую информацию относительно названия БНФ.
-   [Scott Stanchfield](http://www.jguru.com/thetick/) за то, что предоставил мне великолепное сравнение LALR и LL. Я запрашивал разрешения процитировать его, но, к сожалению, не получил ответа.
-   James Huddleston за то, что поправил меня относительно имени Джона Бэкуса.

Last update 2003-07-21, by Lars M. Garshol.

   

___

  Перевод: ©**Alf, 17/10/2004**
