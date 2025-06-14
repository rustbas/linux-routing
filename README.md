# linux-routing

Стенд с маршрутизацией на Linux. 

> [!WARNING] 
> В данный момент наблюдается баг, что чтобы пакеты передавались из одной сети
> в другую, необходимо предварительно пропинговать хосты с
> машины-маршрутизатора.

## Использование

Предварительно необходимо:
1. Установить `vagrant` и `ansible`. Также нужно установить роль для 
   ansible `ansible-galaxy install geerlingguy.nginx`.
2. Скачать **box** виртуальной машины с сайта HashiCorp (необходима версия с
   *libvirt*).

После этого, находясь в папке с репозиторием запустить стенд:
```console
foo@bar:~/linux-router$ vagrant up
```

Подключиться к машине-маршрутизатору и пропинговать хосты:
```console
foo@bar:~/linux-router$ vagrant ssh router
foo@bar:~/linux-router$ ping 10.0.1.10 -c 10
foo@bar:~/linux-router$ ping 10.0.2.10 -c 10
foo@bar:~/linux-router$ logout
```

Теперь можно подключаться к любому хосту и проверять (*для этого на машинах установлен* ***nginx***):
```console
foo@bar:~/linux-router$ vagrant ssh host-1
foo@bar:~/linux-router$ curl 10.0.1.10 # Должен вывести дефолтную страницу nginx
foo@bar:~/linux-router$ curl 10.0.2.10 # Должен вывести дефолтную страницу nginx
foo@bar:~/linux-router$ logout
```
