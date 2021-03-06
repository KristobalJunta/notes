# Работа с push-to-deploy

## Что внутре

Push-to-deploy позволяет отправлять измененения на dev-сервер прямо из git. Для этого на сервере создается специальный репозиторий git - bare

```shell
git init --bare
```

В нем пишется (в данном случае нас интересует только он) хук на получение push'a. Этот хук - обычный шелл-скрипт, в базовом варианте он клонирует содержимое репозитория (того, bare, что на сервере, а не из битбакета) в каталог виртуалхоста.

## На практике

Для того, чтобы иметь себе возможность пушить в деплой, в локальную развертку git нужно добавить еще один remote.
У нас уже есть один, origin - он ссылается на репозиторий в bitbucket, и ветки настроены на отслеживание этого remote. Так и надо.
Для пуша задаем новый:

```
git remote add live git@gorlachov.com:/git/<repo>
```

Я назвал его live.
`<repo>` - местное название репозитория, например, `insoc.git`. От битбакетовского оно не зависит, но я стараюсь называть аналогично.

Теперь, для пуша на dev-сервер надо сделать так:

```
git push live master
```

Пушить будем **ТОЛЬКО** из ветки `master` и только после проверки всего на работоспособность.
