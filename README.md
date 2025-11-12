# Домашнее задание к занятию «Инструменты Git»

### Цель задания

В результате выполнения задания вы:

* научитесь работать с утилитами Git;
* потренируетесь решать типовые задачи, возникающие при работе в команде. 

### Инструкция к заданию

1. Склонируйте [репозиторий](https://github.com/hashicorp/terraform) с исходным кодом Terraform.
2. Создайте файл для ответов на задания в своём репозитории, после выполнения прикрепите ссылку на .md-файл с ответами в личном кабинете.
3. Любые вопросы по решению задач задавайте в чате учебной группы.

------

## Задание

В клонированном репозитории:

1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.
**Команда**
```bash
git show -s --format="%H %s" aefea
```

**Ответ**  
`aefead2207ef7e2aa5dc81a34aedf0cad4c32545  Update CHANGELOG.md`
2. Ответьте на вопросы.

* Какому тегу соответствует коммит `85024d3`?
**Как получено**: найдём все теги, **содержащие** указанный коммит (т.е. указывающие на тот же объект или его потомка в истории).

**Команда**
```bash
git tag --contains 85024d3
```

**Ответ**  
`v0.12.23`

(Также это видно по декораторам `--decorate`: `git show -s --decorate 85024d3`.)
```
* Сколько родителей у коммита `b8d720`? Напишите их хеши.
**Команда**
```bash
git show -s --pretty=%P b8d720
```

**Ответ**  
Два родителя:  
`56cd7859e05c36c06b56d013b55a252d0bb7e158`  
`9ea88f22fc6269854151c571162c5bcf958bee2b`


* Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами  v0.12.23 и v0.12.24.
**Команда**

```bash
git log --oneline v0.12.23..v0.12.24
```

**Ответ**
33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release
```
* Найдите коммит, в котором была создана функция `func providerSource`, её определение в коде выглядит так: `func providerSource(...)` (вместо троеточия перечислены аргументы).
**Как получено**: ищем **появление строки** с объявлением функции по индексации содержимого (`-S`), сортируем от старого к новому и берём первый результат (первое появление в истории).

**Команды**
```bash
git log -S"func providerSource(" --reverse --format="%H %s" | head -n1
# при необходимости посмотреть патч:

# git show <коммит_из_строки_выше>
```
**Ответ**  
`8c928e83589d90a031f811fae52a81be7153e82f  main: Consult local directories as potential mirrors of providers`
```
* Найдите все коммиты, в которых была изменена функция `globalPluginDirs`.
**Команды**
```bash
# Убедимся в имени и файле (необязательно, но полезно):
git grep -n "func globalPluginDirs("

# История изменений функции (кратко):
git log -L :globalPluginDirs:plugins.go --oneline
```

**Ответ** (краткий список из истории `-L`)
```
78b122055 Remove config.go and update things using its aliases
52dbf9483 keep .terraform.d/plugins for discovery
41ab0aef7 Add missing OS_ARCH dir to global plugin paths
66ebff90c move some more plugin search path logic to command
8364383c3 Push plugin discovery down into command package
```
* Кто автор функции `synchronizedWriters`? 
**Как получено**: ищем первое появление самой сигнатуры функции через `-S`, берём **самый ранний** коммит и выводим автора.

**Команды**
```bash
first=$(git log -S"func synchronizedWriters(" --reverse --format="%H" | head -n1)
git show -s --format="%an <%ae>" "$first"
```

**Ответ**  
`Martin Atkins <mart@degeneration.co.uk>`
*В качестве решения ответьте на вопросы и опишите, как были получены эти ответы.*

---

### Правила приёма домашнего задания

В личном кабинете отправлена ссылка на .md-файл в вашем репозитории.

### Критерии оценки

Зачёт:

* выполнены все задания;
* ответы даны в развёрнутой форме;
* приложены соответствующие скриншоты и файлы проекта;
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку:

* задание выполнено частично или не выполнено вообще;
* в логике выполнения заданий есть противоречия и существенные недостатки.
