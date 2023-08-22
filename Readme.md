# Simple Application project

+ **master** - это стабильный готовый к работе код (развернута производественная среда, разрешены только MR c веток **release** и **hotfix**).
+ **develop** - это место, где происходит вся разработка (развернута тестовая среда, по умолчанию, разрешены только MR c веток **feature** и **hotfix**, изначально создается из **master**).
+ **feature** -  используются для разработки новых функций или изменений (не разрешены MR, создается из **develop**). Именуются по маркеру Jira.
+ **release** - используются для подготовки к новому выпуску (не разрешены MR, создается из **develop**).
+ **hotfix** - используются для быстрого решения критических проблем в производственном коде (не разрешены MR, создается из **master**).

## Running application locally

Clone submodules first:  

```
./mvnw -N clean install
```

Build jar file with following command:

```shell script
./mvnw clean install 
```

Change properties in application-local.yaml if needed.  
Run application.

## Цикл производства продукта

Получаем послений образ проекта с ветки develop.
```shell script
./git pull
```
Создаем ветку проекта согласно темы работ Jira.
```shell script
./git branch <маркер задания JIRA>
./git checkout <маркер задания JIRA>
```

Изменения должны иметь маркер [маркер задания JIRA] для отображения достижения в ленте отчётов за неделю.  
Пример комментария:
```
feat(OTHER-564): Added kafka listener
```

По завершению работ проводим слияние ветки доработки с develop, чтобы изменение вступило в силу на тестовом сервере (История изменения полу-линейная, поэтому проводим rebase перед merge).  
Создаем MR в gitlab.  
Не нужную более ветку удаляем после merge.

## Выпуск новой версии продукта в прод

С develop ветки создаем ветку release.  
Проводим необходимые изменения, если требуются.  
Создаем MR в master ветку.  
После merge удаляем ветку release.

## Правила hotfix

Если на проде есть баг, который срочно надо исправить, создаем ветку hotfix с master ветки.  
После создаем MR на master и develop ветки.  
После merge удаляем ветку hotfix.

## Checkstyle

В проекте строгие правила для оформления кода.  
Имеется файл checkstyle.xml, который надо установить локально.  
Для этого в Intellij:
1. Устанавливаем плагин checkstyle.
2. В настройках Editor/Code style/Java/Scheme(Шестеренка)/Import Scheme/Checkstyle configuration и выбираем checkstyle.xml.
3. Чтобы Intellij подсказывал, выбираем в настройках Tools/Checkstyle, выбираем на конфигурационный файл checkstyle.xml и отмечаем галочкой Treat checkstyle errors as warnings.

Checkstyle внедрен в процесс build.  
Перед git push в терминале выполните команду "./mvnw clean install" и убедитесь, что формат кода удовлитворительный.

## Tests

В процесс build встроена проверка code coverage с 80% покрытия кода тестами.  
Тесты к написанному коду обязательны, иначе процесс build не завершится.  