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

Для возврата в конец ветки `Master` необходимо ввести команду:

    git checkout master

# Журнал изменений (git log)

# Ветки в git

# Слияние веток и решение конфликтов

Из индекса и дерева проекта одновременно файл можно удалить командой `git rm`.
Удаляет из индекса и дерева проекта отдельные файлы:

    git rm FILE1 FILE2
Хороший пример удаления из документации к git, удаляются сразу все файлы txt из папки:

    git rm Documentation/\*.txt
Вносит в индекс все удаленные файлы:

    git rm -r --cached .
Сбросить весь индекс или удалить из него изменения определенного файла можно командой `git reset`:
    
    git reset
Удаляет из индекса конкретный файл:
    
    git reset — EDITEDFILE
Команда `git reset` используется не только для сбрасывания индекса, поэтому дальше ей будет уделено гораздо больше внимания.


# Удаление веток
Для удаления ветки используют команду `git branch` с флагами -d или -D

Удаляет ветку, если та была залита (merged) с разрешением возможных конфликтов в текущую:

    git branch -d new-branch
Удаляет ветку в любом случае:

    git branch -D new-branch