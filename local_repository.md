# Маленькая начальная настройка git

**Git** — самая популярная система контроля версий. Работать с Git можно двумя способоами:

* Через командную строку (Терминал)
  
* Через IDE 
 
В каждой системе своя встроенная программа для работы с командной строкой. В Windows это PowerShell или cmd, а в Linux или macOS — Terminal.

Вместо встроенных программ можно использовать любую другую — например, Git Bash в Windows или iTerm2 для macOS. (также есть программы (IDE), в которых можно работать с git).

У гита есть настройка пользователя, от которого будет идти работа. Это разумная и необходимая вещь, так как когда создается коммит, гит берет именно эту информацию для поля *Author*.

Чтобы настроить имя пользователя и почту для всех проектов, нужно прописать следующие команды:

```
git config --global user.name ”Ivan Ivanov”
git config --global user.email ivan.ivanov@gmail.com 

```

Если есть необходимость для конкретного проекта поменять автора (для личного проекта, например), можно убрать `--global`, и так получится:

```
git config user.name ”Ivan Ivanov”
git config user.email ivan.ivanov@gmail.com
```


# Базовые команды для работы с локальным репозиторием

<br>

## Cоздание репозитория. Команда **git init**

<br>

Чтобы создать репозиторий, используется команда **git init**. Ее синтаксис выглядит вот так:

```
git init
```
![Ввод команды создания репозитория](https://cdn.javarush.ru/images/article/5dbf6234-0ea6-40a8-9d37-ce14609d53b4/800.webp)


Команда **git init** создает новый репозиторий Git где будет в дальнейшем храниться вся информация об истории *коммитов*, *тегах* — о ходе разработки проекта. 

С ее помощью можно преобразовать существующий проект без управления версиями в репозиторий Git или инициализировать новый пустой репозиторий. Большинство остальных команд Git невозможно использовать без инициализации репозитория, поэтому данная команда обычно выполняется первой в рамках нового проекта.

При выполнении команды **git init** в текущем рабочем каталоге создается подкаталог **.git** со всеми необходимыми метаданными Git для нового репозитория. Метаданные включают подкаталоги для объектов, ссылок и файлов шаблонов. Кроме того, создается файл *HEAD*, который указывает на текущий извлеченный коммит.

Кроме создания каталога **.git**, корневой каталог существующего проекта не изменяется каким-либо образом (в отличие от SVN, Git не требует наличия подкаталога **.git** в каждом подкаталоге).

<br>

## Отображение состояния проекта. Команда **git status**

Команду **git status**, пожалуй, можно считать самой часто используемой наряду с командами *коммита* и *индексации*. Она выводит информацию обо всех изменениях, внесенных в дерево директорий проекта по сравнению с последним коммитом рабочей ветки; отдельно выводятся внесенные в индекс и неиндексированные файлы. 

Эта команда отображает состояние рабочего каталога и раздела проиндексированных файлов. С ее помощью можно проверить индексацию изменений и увидеть файлы, которые не отслеживаются Git. Информация об истории коммитов проекта не отображается при выводе данных о состоянии. Для этого используется команда **git log** (будет рассмотрена ниже).

Использовать ее крайне просто:
```
git status
```
![Пример использования команды](https://cdn.javarush.ru/images/article/5159fcca-3b1c-4308-b1e4-854c1d480385/800.webp)


Кроме того, **git status** указывает на файлы с неразрешенными конфликтами слияния и файлы, игнорируемые git.



<br>

## Индексация изменений. Команды **git add, git rm**

<br>

Следующее, что нужно знать — команда **git add**. Она позволяет внести в *индекс* — временное хранилище — изменения, которые затем войдут в коммит. Она сообщает Git, что вы хотите включить изменения в конкретном файле в следующий коммит. Однако на самом деле команда **git add** не оказывает существенного влияния на репозиторий: изменения регистрируются в нем только после выполнения команды **git commit** (об этой команде будет сказано чуть ниже).

<br>

Ее синтаксис выглядит вот так:

```
git add
```

<br>

Однако, есть несколько вариантов использования данной команды:

<br>

1. Вносит в индекс все изменения, включая новые файлы (добавляет в индекс все файлы, находящиеся в этой папке и всех внутренних)

    ``` 
    git add . 
    ``` 
<br>

2. Добавляет только конкретный файл.


    ```
    git add <имя файла>
    ```
Здесь можно пользоваться регулярными выражениями, чтобы добавлять по какому-то шаблону. 

***Например***:
 
 ```
 git add *.java
 ```
 
Это значит, что нужно добавить только файлы с расширением **.java** (символ `*` используется для замены имени файлов, т.е. имеется в виду, что в данном примере важно не имя, а расширение файлов, поэтому будут добавлены все файлы с расширением **.java**)

<br>

3. Проиндексировать все изменения в каталоге `<directory>` для следующего коммита.

    ```
    git add <directory>
    ```
<br>

>*Есть и другие опции использования этой команды, здесь же были показаны основные из них.*

<br>

По итогу, команда **git add** — это первая команда в цепочке операций, предписывающей Git «сохранить» снимок текущего состояния проекта в истории коммитов. Когда **git add** используется как отдельная команда, она переносит ожидающие изменения файлы из рабочего каталога в раздел проиндексированных файлов для дальнейшей работы с ними.

<br>

*Команда git rm*

<br>

В начале использования Git часто возникает вопрос: «Как заставить Git больше не отслеживать какой-либо файл или несколько файлов?» Чтобы удалить файлы из репозитория Git, можно воспользоваться командой **git rm.** Ее действие противоположно действию команды **git add.**

Команда **git rm** позволяет удалять отдельные файлы или группы файлов.  Ее основное назначение — удаление отслеживаемых файлов из раздела проиндексированных файлов Git. Кроме того, с помощью **git rm** можно удалить файлы одновременно из раздела проиндексированных файлов и рабочего каталога. *Удалить с ее помощью файл только из рабочего каталога нельзя.* Файлы, в отношении которых выполняется команда, должны быть идентичны файлам в текущем указателе *HEAD*. В случае расхождений между версией файла из указателя *HEAD* и версией из раздела проиндексированных файлов или рабочего дерева Git заблокирует удаление. Такая блокировка является механизмом безопасности, который предотвращает удаление изменений в процессе их внесения. Также запомните, что **git rm** не удаляет *ветки*.
  
<br>

Ее синтаксис выглядит вот так:

Удаляет из индекса и дерева проекта отдельные файлы:

```
git rm <file>
```
Указывает файлы, подлежащие удалению. Можно указать один файл, несколько файлов через пробел (file1 file2 file3) или шаблон подстановки (~./directory/*).

***Например***:
```
git rm Documentation/\*.txt
```
или 

```
git rm FILE1 FILE2
```

<br>

***Также, есть несколько вариантов использования данной команды***:

1. Параметр `-f` (force) применяется для отключения проверки безопасности, с помощью которой Git обеспечивает соответствие файлов в указателе HEAD текущему содержимому раздела проиндексированных файлов и рабочего каталога.
   
    ```
    git rm -f git-*.sh
    ```

В этом примере команда выполняется с параметром force для всех файлов, соответствующих шаблону подстановки git-*.sh . Параметр force явным образом удаляет целевые файлы из рабочего каталога и раздела проиндексированных файлов.

<br>

2. Параметр `-r` — это сокращение от слова recursive. При выполнении команды git rm в рекурсивном режиме она удаляет не только каталог назначения, но и все содержимое его вложенных каталогов.
   
    ```
    git rm -r --cached .
    ```

>*Есть и другие опции использования этой команды. Здесь были показаны лишь некоторые из них.*

<br>

## Отмена изменений, возврат к определенному коммиту. Команда **git reset**

Изменения, вносимые при выполнении команды **git rm**, не являются окончательными. Эта команда обновляет раздел проиндексированных файлов и рабочий каталог. Изменения не сохранятся, пока не будет создан новый коммит и они не будут добавлены в историю коммитов. Так что изменения, внесенные командой **git rm**, можно «отменить» с помощью стандартных команд Git.

Сбросить весь индекс или удалить из него изменения определенного файла:

```
git reset
```
<br>
Удаляет из индекса конкретный файл:

<br>

```
git reset — EDITEDFILE
```
<br>

Команда **git reset** используется не только для сбрасывания индекса, поэтому дальше ей будет уделено больше внимания.

<br>

Команда **git reset** имеет три основные формы вызова, соответствующие аргументам командной строки `--soft`, `--mixed`, `--hard`. 

Каждый из этих трех аргументов соответствует трем внутренним механизмам управления состоянием Git:

* **дереву коммитов (HEAD)**
  
* **разделу проиндексированных файлов**
  
* **рабочему каталогу**.

<br>

* `-- hard`  
  ```
  git reset --hard
  ```
  Это самый прямой, ОПАСНЫЙ и часто используемый вариант. При использовании аргумента `--hard` указатели в истории коммитов обновляются на указанный коммит. Затем происходит сброс раздела проиндексированных файлов и рабочего каталога до указанного коммита. Все предыдущие ожидающие изменения в разделе проиндексированных файлов и рабочем каталоге сбрасываются в соответствии с состоянием дерева коммитов. Это значит, что любая работа, находившаяся в состоянии ожидания в разделе проиндексированных файлов и рабочем каталоге, будет потеряна.

<br>

* `--mixed`
  ```
  git reset --mixed
  ```
  
  Это режим работы по умолчанию. Указатели ссылок обновляются. Раздел проиндексированных файлов сбрасывается до состояния указанного коммита. Любые изменения, которые были отменены в разделе проиндексированных файлов, перемещаются в рабочий каталог.

<br>

* `--soft`
  ```
  git reset --soft
  ```
  
  При передаче аргумента `--soft` выполняется обновление указателей, и на этом операция сброса останавливается. Раздел проиндексированных файлов и рабочий каталог остаются неизменными. Четко продемонстрировать такое поведение довольно сложно.


<br>
<br>

## Cовершение коммита. Команда **git commit** 

Команда **git commit** делает для проекта снимок текущего состояния изменений, добавленных в раздел проиндексированных файлов. Такие подтвержденные снимки состояния можно рассматривать как «безопасные» версии проекта — Git не будет их менять, пока вы явным образом не попросите об этом. Перед выполнением команды **git commit** необходимо использовать команду **git add**, чтобы добавить в проект («проиндексировать») изменения, которые будут сохранены в коммите. 

**Коммиты** — основные конструктивные элементы временной шкалы проекта Git. Их можно рассматривать как снимки состояния или контрольные точки на временной шкале проекта Git. Коммиты создаются с помощью команды **git commit**, которая делает снимок состояния проекта на текущий момент времени. Коммиты снимков состояния Git всегда выполняются в локальном репозитории.

В простейшем случае достаточно после индексации (команды **git add**) набрать:

```
git commit
```

Коммит проиндексированного состояния кода. Эта команда откроет текстовый редактор с предложением ввести комментарий к коммиту. После ввода комментария сохраните файл и закройте текстовый редактор, чтобы выполнить коммит.

<br>

***Распространенные опции***:

1. Совершает коммит, автоматически индексируя изменения в файлах проекта. Эта команда включает только изменения отслеживаемых файлов (тех, которые были в какой-то момент добавлены в историю с помощью команды **git add**). <u> Новые файлы при этом индексироваться не будут! </u> Удаление же файлов будет учтено:

    ```
    git commit -a
    ```

<br>

2. Быстрая команда, которая создает коммит с указанным комментарием. По умолчанию команда git commit открывает локально настроенный текстовый редактор с предложением ввести комментарий к коммиту. При передаче параметра -m текстовый редактор не открывается, а используется подставленный комментарий:

    ```
    git commit -m "commit message"
    ```

<br>

***Есть и другие опции использования этой команды. Здесь были показаны лишь некоторые из них.***

<br>
<br>


## Получение разнообразной информация о коммитах. Команда **git log**
<br>

Простейший пример использования, в котором приводится короткая справка по всем коммитам, коснувшимся активной в настоящий момент ветки. Использовать ее крайне просто:

```
git log
```
<br>

![Пример вызова команды git log](https://cdn.javarush.ru/images/article/b869b551-e804-4ac1-b199-6f16684be943/800.webp)

<br>

*По умолчанию (без аргументов)* команда **git log** перечисляет коммиты, сделанные в репозитории в обратном к хронологическому порядке — последние коммиты находятся вверху. Из примера можно увидеть, что данная команда перечисляет коммиты с их *SHA-1 контрольными суммами, именем и электронной почтой автора, датой создания и сообщением коммита*.

<br>

Команда **git log** имеет очень большое количество опций для поиска коммитов по разным критериям. Рассмотрим наиболее популярные из них:
<br>

1. Одним из самых полезных аргументов является `-p` или `--patch`, который показывает разницу (выводит патч), внесенную в каждый коммит. 
   
    ```
    git log -p
    ```

<br>

2. Статистика изменения файлов, вроде числа измененных файлов, внесенных в них строк, удаленных файлов вызывается ключом `--stat`:
   
   ```
   git log --stat
   ```

<br>

3. Команда **grep** - мощный инструмент, который помогает работать в том числе и с git. Например, искать по коммитам:

    ```
    git log --oneline | grep revert # поиск упоминания revert

    git log --oneline | grep -i revert # независимо от регистра
    ```

<br>

Мы рассмотрели базовые примеры, но в документации по **git log** есть много различных опций. Все их рассматривать нет смысла, при необходимости изучайте документацию, вызвать которую можно так:

```
git log --help
```
<br>
<br>


## Отличия между объектами в проекте. Команда **git diff**

Своего рода подмножеством команды **git log** можно считать команду **git diff**, определяющую изменения между объектами в проекте - *деревьями* (файлов и директорий).

**Сравнение** — это функция, анализирующая два входных набора данных и отображающая различия между ними. **git diff** представляет собой многоцелевую команду Git, которая инициирует функцию сравнения источников данных Git — коммитов, веток, файлов и т. д.

Стандартный вызов команды. Показывает изменения, не внесенные в индекс:

```
git diff
```

<br>

![Пример использования команды git diff](https://cdn.javarush.ru/images/article/1f3506ae-1746-4ee9-b346-c992be73bb63/800.webp)

<br>

***Пояснение к картинке:***

1. 
    ```
    diff --git a/test_resource.txt b/test_resource.txt
    ```
В этой строке отображаются входные данные сравнения. Как видите, для сравнения переданы файлы `a/test_resource.txt и b/test_resource.txt`.

<br>
<br>

2.
    ```
    index e69de29..bc7774a 100644
    ```
В этой строке отображаются внутренние метаданные Git. Скорее всего, они вам не понадобятся. Номера в этих выходных данных соответствуют *хеш-идентификаторам* версий объектов Git.

<br>
<br>

3. 
    ```
    --- a/test_resource.txt
    +++ b/test_resource.txt
    ```

Эти строки представляют собой легенду обозначений для каждого источника входных данных сравнения. В данном случае изменения из файла `a/test_resource.txt `помечаются символом `---`, а из файла `b/test_resource.txt` — символом `+++`.

<br>
<br>

4. Остальные выходные данные сравнения — это список сравниваемых фрагментов. При сравнении отображаются только разделы файла, в которых есть изменения. В данном примере имеется только один такой фрагмент, поскольку обрабатывается простой сценарий. Фрагменты имеют собственную ограниченную семантику вывода.

```
@@ -0,0 +1 @@
+ hello world!
```

Первая строка — это **заголовок фрагмента**. К началу каждого фрагмента добавляется заголовок, ограниченный символами `@@`. Заголовок кратко описывает изменения в файле. В нашем простом примере заголовок `-0,0 +1` означает, что имеются изменения в первой строке. 

В реальных случаях заголовок может выглядеть так:

```
@@ -34,6 +34,8 @@
```
В данном примере заголовка было извлечено 6 строк начиная со строки 34. Кроме того, после строки 34 было добавлено 8 строк.

Остальное содержимое фрагмента сравнения — это недавние изменения. Каждой измененной строке предшествует символ `+` или `-`, указывающий на источник входных данных сравнения. Как уже упоминалось, символ `-` указывает на изменения в файле `a/test_resource.txt`, а `+` на изменения в файле `b/test_resource.txt.`

<br>

---

<div class = "buttonnavigation" align="center">
<p>

[К содержанию](readme.md/#содержание)

[Назад](architecture.md) 
[Далее](remote_repository.md)

</p>
</div>