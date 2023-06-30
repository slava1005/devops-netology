__Домашнее задание к занятию 7. «Введение в Terraform»__

__Задача 1__

Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
```
root@slava:/home/slava/terraform01/01/src# git clone https://github.com/netology-code/ter-homeworks
Клонирование в «ter-homeworks»…
remote: Enumerating objects: 766, done.
remote: Counting objects: 100% (162/162), done.
remote: Compressing objects: 100% (68/68), done.
remote: Total 766 (delta 128), reused 94 (delta 94), pack-reused 604
Получение объектов: 100% (766/766), 174.14 KiB | 625.00 KiB/s, готово.
Определение изменений: 100% (389/389), готово.
```

Изучите файл .gitignore. В каком terraform файле допустимо сохранить личную, секретную информацию?
```
personal.auto.tfvars - позволяет именовать файлы с переменными (в том числе секретными)
```

Выполните код проекта. Найдите в State-файле секретное содержимое созданного ресурса random_password. Пришлите его в качестве ответа.

![terra1](https://github.com/slava1005/devops-netology/assets/114395964/55055e03-a170-43ed-8bc3-ebb798465133)

```
root@slava:~/01/src# terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_password.random_string will be created
  + resource "random_password" "random_string" {
      + bcrypt_hash = (sensitive value)
      + id          = (known after apply)
      + length      = 16
      + lower       = true
      + min_lower   = 1
      + min_numeric = 1
      + min_special = 0
      + min_upper   = 1
      + number      = true
      + numeric     = true
      + result      = (sensitive value)
      + special     = false
      + upper       = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

random_password.random_string: Creating...
random_password.random_string: Creation complete after 0s [id=none]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```


Раскомментируйте блок кода, примерно расположенный на строчках 29-42 файла main.tf. Выполните команду terraform -validate. Объясните в чем заключаются намеренно допущенные ошибки? Исправьте их.
```
Error: Missing name for resource (Ошибка: Отсутствует имя для ресурса)
Error: Missing name for resource.
│
│   on main.tf line 24, in resource "docker_image":
│   24: resource "docker_image" {
│
│ All resource blocks must have 2 labels (type, name).
```

Нужно добавить имя nginx
```
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}
```

Неверное имя ресурса, а именно 1nginx. (Имя должно начинаться с буквы или подчеркивания и может содержать только буквы, цифры, подчеркивания и тире.)
```
Error: Invalid resource name
│
│   on main.tf line 28, in resource "docker_container" "1nginx":
│   28: resource "docker_container" "1nginx" {
│
│ A name must start with a letter or underscore and may contain only letters, digits, underscores, and dashes.
```

Переименновали название имени nginx_1
```
resource "docker_container" "nginx_1" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"
  ports {
    internal = 80
    external = 8000
  }
}
```

Выполните код. В качестве ответа приложите вывод команды docker ps


```
root@slava:/home/slava/terraform01/01/src# terraform apply
random_password.random_string: Refreshing state... [id=none]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx_1 will be created
  + resource "docker_container" "nginx_1" {
      + attach                                      = false
      + bridge                                      = (known after apply)
      + command                                     = (known after apply)
      + container_logs                              = (known after apply)
      + container_read_refresh_timeout_milliseconds = 15000
      + entrypoint                                  = (known after apply)
      + env                                         = (known after apply)
      + exit_code                                   = (known after apply)
      + hostname                                    = (known after apply)
      + id                                          = (known after apply)
      + image                                       = (known after apply)
      + init                                        = (known after apply)
      + ipc_mode                                    = (known after apply)
      + log_driver                                  = (known after apply)
      + logs                                        = false
      + must_run                                    = true
      + name                                        = (sensitive value)
      + network_data                                = (known after apply)
      + read_only                                   = false
      + remove_volumes                              = true
      + restart                                     = "no"
      + rm                                          = false
      + runtime                                     = (known after apply)
      + security_opts                               = (known after apply)
      + shm_size                                    = (known after apply)
      + start                                       = true
      + stdin_open                                  = false
      + stop_signal                                 = (known after apply)
      + stop_timeout                                = (known after apply)
      + tty                                         = false
      + wait                                        = false
      + wait_timeout                                = 60

      + ports {
          + external = 8000
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id           = (known after apply)
      + image_id     = (known after apply)
      + keep_locally = true
      + name         = "nginx:latest"
      + repo_digest  = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 0s [id=sha256:f9c14fe76d502861ba0939bc3189e642c02e257f06f4c0214b1f8ca329326cdanginx:latest]
docker_container.nginx_1: Creating...
docker_container.nginx_1: Creation complete after 8s [id=1dd0f80c9f3707e37052d0e67b01594f32af0ac360f346da75fe8110f931e731]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

root@slava:/home/slava/terraform01/01/src# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
1dd0f80c9f37   f9c14fe76d50   "/docker-entrypoint.…"   24 seconds ago   Up 15 seconds   0.0.0.0:8000->80/tcp   example_sKlu1ABAL1NbgfnZ
```

Замените имя docker-контейнера в блоке кода на hello_world, выполните команду terraform apply -auto-approve. Объясните своими словами, в чем может быть опасность применения ключа -auto-approve ?
```
terraform apply -auto-approve Пропускает интерактивное утверждение плана перед применением
```

```
root@slava:/home/slava/terraform01/01/src# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                  NAMES
3aa5d23g5f21   f9c14fe76d50   "/docker-entrypoint.…"   59 seconds ago   Up 35 seconds   0.0.0.0:8000->80/tcp   hello_world
```

Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.

```
root@slava:/home/slava/terraform01/01/src# nano terraform.tfstate
{
  "version": 4,
  "terraform_version": "1.5.1",
  "serial": 9,
  "lineage": "f0a21746-5b84-5976-510e-c3d80c60b655",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```


Объясните, почему при этом не был удален docker образ nginx:latest? 
(Ответ найдите в коде проекта или документации).
```
Keep_locally - (Необязательно, логическое значение) Если true, то образ Docker не будет удален при операции уничтожения. Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения.
```
