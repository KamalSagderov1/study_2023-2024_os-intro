---
## Front matter
title: "Лабораторная работа No 6."
author: "Сагдеров Камал, НФИбд-04-22"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux.

# Выполнение лабораторной работы

## Подготовка лабораторного стенда

1. Установить `Apache2` при помощи `dnf`.

```
dnf install httpd
```

2. В конфигурационном файле httpd.conf прописать параметр ServerName (@fig:001).

![ServerName](./image/1.png){#fig:001}

![ServerName](./image/2.png){#fig:002}

3. Отключить пакетный фильтр при помощи `iptables` (@fig:002).

![iptables](./image/3.png){#fig:003} 

[ServerName](./image/4.png){#fig:004}

[ServerName](./image/5.png){#fig:005}

[ServerName](./image/6.png){#fig:006}

[ServerName](./image/15.png){#fig:015}

## Выполнение

1. Проверим правильность работы SELinux. Должен быть выставлен режим enforcing политики targeted (@fig:003).

![sestatus](./image/7.png){#fig:007} 

2. Запустим Apache веб-сервер (@fig:004).

![httpd](./image/8.png){#fig:008} 

3. В списке процессов найдем httpd (@fig:005). На этот процесс выставлен следующий контекст безопасности (первый столбец изображения @gentooSELinuxTutorialsLinuxServices).

![Контекст безопасности](./image/9.png){#fig:009} 

4. Посмотрим текущее состояние переключателей SELinux для Apache2 (@fig:006).

5. Также посмотрим текущую статистику по политике (@fig:007).

![Статистика по политике](./image/10.png){#fig:010} 

6. Посмотрим текущий контекст безопасности для файлов и поддиректорий в директории `/var/www` (@fig:008).

 - Установлен контекст `httpd_sys_script_exec_t` для cgi-скриптов, чтобы был разрешен им доступ ко всем sys-типам.

 - Установлен контекст `httpd_sys_content_t` для содержимого, которое должно быть доступно для всех скриптов httpd и для самого демона.
 
![Статистика по политике](./image/14.png){#fig:014} 

7. В директории `/var/www/html` пусто.

![/var/www/html](./image/10.png){#fig:010} 

8. В директории `/var/www/html` создавать папки может только root (право w есть только у него).

![Статистика по политике](./image/12.png){#fig:012} 

9. Создадим файл `/var/www/html/test.html` (@fig:010).

![test.html](./image/11.png){#fig:011} 

10. Проверим контекст созданного нами файла (@fig:011).

![test.html](./image/11.png){#fig:011} 

11. Перейдем в браузер и в нем проверим доступность данного файла (@fig:012).

![Проверка](./image/16.png){#fig:016} 

12. Изменим конекст файла, чтобы Apache не смог получить доступ (@fig:013).

![test.html](./image/17.png){#fig:017} 

13. Проверим, что доступ к файлу стал не доступен (@fig:014).

![Проверка](./image/18.png){#fig:018} 

14. Посмотрим логи от веб-сервера Apache (@fig:015).

![/var/log/messages](./image/19.png){#fig:019} 

Также проверим audit.log (@fig:0151).

![/var/log/audit/audit.log](./image/19.1.png){#fig:0191} 

15. Поменяем порт, на котором работает Apache.

![Порт 81](./image/20.png){#fig:020} 

16. Перезапустим веб-сервер (успешно).

![Перезапуск](./image/21.png){#fig:021} 

17. В логах наблюдаем запуск сервера на 81 порту.

![/var/log/messages](./image/22.png){#fig:022} 

18. Добавим порт в `semanage` для `http_port_t` и проверим его добавление

![Добавление](./image/23.png){#fig:023} 

![Проверка](./image/23.1.png){#fig:0231} 

19. Ввернем контекст файлу `test.html`.

20. Удалим привязку порта.

21. Удалим файл `test.html`.

# Выводы

В результате выполнения работы я выполнил цели работы.
