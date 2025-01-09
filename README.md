# Домашнее задание к занятию "`«Введение в Terraform»»`"

---

### Чек-лист и подготовка

Цели задания
Установить и настроить Terrafrom.

Научиться использовать готовый код.
Чек-лист готовности к домашнему заданию
Скачайте и установите Terraform версии ~>1.8.4 . Приложите скриншот вывода команды terraform --version.
Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории 01/src.
Убедитесь, что в вашей ОС установлен docker.
Зарегистрируйте аккаунт на сайте https://hub.docker.com/, выполните команду docker login и введите логин, пароль.

### Выполнения подготовки

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/0-terra.png)

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/0%20terra%202-3.png)

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/0-terra%20dock.png)


### Задание 1.1-8

1) Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
2) Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?
3) Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
4) Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5) Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
6) Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
7) Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
8) Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker.

### Выполнения задания 1.1

Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/1-1.png)

### Выполнения задания 1.2

Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/1-2.png)

Обычно это файлы с расширением .tfvars, такие как *.tfvars или конкретные файлы, например, secrets.tfvars.
Для хранения секретной информации подходит - personal.auto.tfvars

### Выполнения задания 1.3

 Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/1-3.png)

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/1-3-1.png)

### Выполнения задания 1.4

Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.


![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/4-1.png)

"docker_image" - ресурсу не назначено имя.
"docker_container" - имя начинается с цифры.

Исправил.

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/4-2.png)

### Выполнения задания 1.5

Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.

```bash
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}

```
![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/5.png)


### Выполнения задания 1.6

Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.

Открыл и заменил имя контейнера.

```bash
resource "docker_container" "nginx" {
  name  = "hello_world"
  image = "nginx:latest"
  ...
}
```

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/6-1.png)

Опасность использования ключа -auto-approve:
Ключ -auto-approve автоматически подтверждает все изменения, что может привести к случайному удалению или изменению ресурсов без моего подтверждения.

### Выполнения задания 1.7

Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.

```sh
terraform destroy -auto-approve
```

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/7-1.png)


### Выполнения задания 1.8

Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker.

Docker-образ nginx:latest не был удален, потому что Terraform управляет контейнерами, а не образами. Образы остаются в системе, чтобы их можно было использовать повторно.

https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image


keep_locally (логическое значение) Если значение true, то изображение Docker не будет удалено при операции destroy.
Если значение false, то изображение будет удалено из локального хранилища docker при операции destroy.

![image.jpg](https://github.com/temagraf/My_terraform_intro/blob/main/8.jpg)
