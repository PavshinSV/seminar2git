# Что такое Git

**система контроля версий(VCS)** - програмное обеспечение для облегчения работы с изменяющейся информацией.

# Подготовка репозитория

## git init

Команда `git init` создает в директории пустой репозиторий в виде директории .git, где и будет в дальнейшем храниться вся информация об истории коммитов, тегах — о ходе разработки проекта:

    mkdir project-dir
    cd project-dir
    git init

# Создание сохранений 

Следующее, что нужно знать — команда `git add`. Она позволяет внести в индекс — временное хранилище — изменения, которые затем войдут в коммит.
Индексирует измененный файл, либо оповещение о создании нового:
git add EDITEDFILE
Вносит в индекс все изменения, включая новые файлы:
git add .
    
    git add fileName.ext

# Перемещение между сохранениями 

Команда `git checkout` позволяет переключаться между последними коммитами (если упрощенно) веток:

    checkout some-other-branch

Создаёт ветку, в которую и произойдет переключение:

    git checkout -b some-other-new-branch
Если в текущей ветке были какие-то изменения по сравнению с последним коммитом в ветке(HEAD), то команда откажется производить переключение, дабы не потерять произведенную работу. Проигнорировать этот факт позволяет ключ -f:

    git checkout -f some-other-branch
В случае, когда изменения надо все же сохранить, следует использовать ключ -m. Тогда команда перед переключением попробует залить изменения в текущую ветку и, после разрешения возможных конфликтов переключиться в новую:

    git checkout -m some-other-branch
Вернуть файл (или просто вытащить из прошлого коммита) позволяет команда вида:

    git checkout somefile
Возвращает somefile к состоянию последнего коммита:

    git checkout somefile
Возвращает somefile к состоянию на два коммита назад по ветке:
    
    git checkout HEAD~2 somefile

# про возврат на конец ветки master

# Журнал изменений (git log)

Иногда требуется получить информацию об истории коммитов; коммитах, изменивших отдельный файл; коммитах за определенный отрезок времени и так далее. Для этих целей используется команда `git log`.

Простейший пример использования, в котором приводится короткая справка по всем коммитам, коснувшимся активной в настоящий момент ветки (о ветках и ветвлении подробно узнать можно ниже, в разделе «Ветвления и слияния»):

    git log
Получает подробную информацию о каждом в виде патчей по файлам из коммитов можно, добавив ключ `-p` (или `-u`):

    git log -p
Статистика изменения файлов, вроде числа измененных файлов, внесенных в них строк, удаленных файлов вызывается ключом `--stat`:

    git log --stat
За информацию по созданиям, переименованиям и правам доступа файлов отвечает ключ `--summary`:

    git log --summary
Чтобы просмотреть историю отдельного файла, достаточно указать в виде параметра его имя (кстати, в моей старой версии git этот способ не срабатывает, обязательно добавлять " — " перед «README»):

    git log README
или, если версия `git` не совсем свежая:

    git log — README
Далее приводится только более современный вариант синтаксиса. Возможно указывать время, начиная в определенного момента («`weeks`», «`days`», «`hours`», «`s`» и так далее):

    git log --since=«1 day 2 hours» README
    git log --since=«2 hours» README
изменения, касающиеся отдельной папки:

    git log --since=«2 hours» dir/
Можно отталкиваться от тегов.

Все коммиты, начиная с тега `v1`:

    git log v1...
Все коммиты, включающие изменения файла `README`, начиная с тега `v1`:

    git log v1... README
Все коммиты, включающие изменения файла `README`, начиная с тега `v1` и заканчивая тегом `v2`:

    git log v1..v2 README
Интересные возможности по формату вывода команды предоставляет ключ `--pretty`.

Выводит на каждый из коммитов по строчке, состоящей из хэша (здесь — уникального идентификатора каждого коммита, подробней — дальше):

    git log --pretty=oneline
Лаконичная информация о коммитах, приводятся только автор и комментарий:

    git log --pretty=short
Более полная информация о коммитах, с именем автора, комментарием, датой создания и внесения коммита:

    git log --pretty=full/fuller
В принципе, формат вывода можно определить самостоятельно:

    git log --pretty=format:'FORMAT'
Определение формата можно поискать в разделе по `git log` из `Git Community Book` или справке. Красивый ASCII-граф коммитов выводится с использованием ключа `--graph`.

`git diff` — отличия между деревьями проекта, коммитами и т.д.
Своего рода подмножеством команды `git log` можно считать команду `git diff`, определяющую изменения между объектами в проекте - деревьями (файлов и директорий).

Показывает изменения, не внесенные в индекс:

    git diff
Изменения, внесенные в индекс:

    git diff --cached
Изменения в проекте по сравнению с последним коммитом:

    git diff HEAD
Предпоследним коммитом:

    git diff HEAD^
Можно сравнивать «головы» веток:

    git diff master..experimental
или активную ветку с какой-либо:

    git diff experimental

`git show` — показать изменения, внесенные отдельным коммитом.

Посмотреть изменения, внесенные любым коммитом в истории, можно командой `git show`:

    git show COMMIT_TAG
`git blame` и `git annotate` — команды, помогающие отслеживать изменения файлов
При работе в команде часто требуется выяснить, кто именно написал конкретный код. Удобно использовать команду `git blame`, выводящую построчную информацию о последнем коммите, коснувшемся строки, имя автора и хэш коммита:

    git blame README
Можно указать и конкретные строки для отображения:

    git blame -L 2,+3 README — выведет информацию по трем строкам, начиная со второй.
Аналогично работает команда `git annotate`, выводящая и строки, и информацию о коммитах, их коснувшихся:

    git annotate README

`git grep` — поиск слов по проекту, состоянию проекта в прошлом
`git grep`, в целом, просто дублирует функционал знаменитой юниксовой команды. Однако он позволяет слова и их сочетания искать в прошлом проекта, что бывает очень полезно.

Ищет слова `tst` в проекте:

    git grep tst
Подсчитывает число упоминаний `tst` в проекте:

    git grep -с tst
Ищет в старой версии проекта:

    git grep tst v1
Команда позволяет использовать логическое И и ИЛИ.

Ищет строки, где упоминаются и первое слово, и второе:

    git grep -e 'first' --and -e 'another'
Ищет строки, где встречается хотя бы одно из слов:
    git grep --all-match -e 'first' -e 'second'


# Ветки в git

`git branch` — создание, перечисление и удаление веток.

Работа с ветками — очень легкая процедура в git, все необходимые механизмы сконцентрированы в одной команде.

Просто перечисляет существующие ветки, отметив активную:

    git branch
Создаёт новую ветку `new-branch`:

    git branch new-branch
Удаляет ветку, если та была залита (merged) с разрешением возможных конфликтов в текущую:

    git branch -d new-branch
Удаляет ветку в любом случае:

    git branch -D new-branch
Переименовывает ветку:

    git branch -m new-name-branch
Показывывает те ветки, среди предков которых есть определенный коммит:

    git branch --contains v1.2
Показывает коммит ответвления ветки `new-name-branch` от ветки `master`:

    git merge-base master new-name-branch

`git checkout` — переключение между ветками, извлечение файлов.

Команда `git checkout` позволяет переключаться между последними коммитами (если упрощенно) веток:

    checkout some-other-branch
Создаёт ветку, в которую и произойдет переключение:

    git checkout -b some-other-new-branch
Если в текущей ветке были какие-то изменения по сравнению с последним коммитом в ветке(`HEAD`), то команда откажется производить переключение, дабы не потерять произведенную работу. Проигнорировать этот факт позволяет ключ -f:

    git checkout -f some-other-branch
В случае, когда изменения надо все же сохранить, следует использовать ключ -m. Тогда команда перед переключением попробует залить изменения в текущую ветку и, после разрешения возможных конфликтов переключиться в новую:

    git checkout -m some-other-branch
Вернуть файл (или просто вытащить из прошлого коммита) позволяет команда вида:

    git checkout somefile
Возвращает `somefile` к состоянию последнего коммита:

    git checkout somefile
Возвращает `somefile` к состоянию на два коммита назад по ветке:

    git checkout HEAD~2 somefile


# Слияние веток и решение конфликтов

`git merge` — слияние веток, разрешение возможных конфликтов.

Слияние веток, в отличие от обычной практики централизованных систем, в `git` происходит практически каждый день. Естественно, что имеется удобный интерфейс к популярной операции.

Пытается объединить текующую ветку и ветку `new-feature`:

    git merge new-feature
В случае возникновения конфликтов коммита не происходит, а по проблемным файлам расставляются специальные метки а-ля svn; сами же файлы отмечаются в индексе как «не соединенные» (unmerged). До тех пор пока проблемы не будут решены, коммит совершить будет нельзя.

Например, конфликт возник в файле `TROUBLE`, что можно увидеть в `git status`.

Произошла неудачная попытка слияния:

    git merge experiment
Смотрим на проблемные места:

    git status
Разрешаем проблемы:

    edit TROUBLE
Индексируем наши изменения, тем самым снимая метки:

    git add .
Совершаем коммит слияния:

    git commit
Вот и все, ничего сложного. Если в процессе разрешения вы передумали разрешать конфликт, достаточно набрать (это вернёт обе ветки в исходные состояния):

    git reset --hard HEAD
Если же коммит слияния был совершен, используем команду:

    git reset --hard ORIG_HEAD


# Удаление веток
Для удаления ветки используют команду `git branch` с флагами -d или -D

Удаляет ветку, если та была залита (merged) с разрешением возможных конфликтов в текущую:

    git branch -d new-branch
Удаляет ветку в любом случае:

    git branch -D new-branch