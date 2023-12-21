# Терминирование TLS-соединений с помощью Yandex Application Load Balancer

[L7-балансировщики](https://cloud.yandex.ru/docs/application-load-balancer/concepts/application-load-balancer) Yandex Application Load Balancer могут _терминировать_ TLS-соединения: отправлять клиентам сертификаты, дешифровать входящий трафик для отправки [бэкендам](https://cloud.yandex.ru/docs/application-load-balancer/concepts/backend-group) и шифровать ответы бэкендов для отправки клиентам. 

Вы настроите балансировщик так, чтобы он терминировал TLS-соединения с помощью [сертификата](https://cloud.yandex.ru/docs/certificate-manager/concepts/) из [Yandex Certificate Manager](https://cloud.yandex.ru/docs/certificate-manager) и перенаправлял HTTP-запросы на HTTPS.

Подробное руководство см. в документации Yandex Cloud: [Терминирование TLS-соединений](https://cloud.yandex.ru/docs/application-load-balancer/tutorials/tls-termination).

Подробную информацию о ресурсах провайдера смотрите в документации на сайте [Terraform](https://www.terraform.io/docs/providers/yandex/index.html) или в [зеркале](https://terraform-provider.yandexcloud.net/).

Чтобы создать инфрастуктуру для терминирования TLS-соединений:

1. [Установите Terraform](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform), [получите данные для аутентификации](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#get-credentials) и укажите источник для установки провайдера Yandex Cloud (раздел [Настройте провайдер](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#configure-provider), шаг 1).

1. Клонируйте репозиторий с конфигурационными файлами:

    ```bash
    git clone https://github.com/yandex-cloud-examples/yc-alb-tls-termination.git
    ```

1. Перейдите в директорию с репозиторием. В ней должны появиться файлы:
    * `tls-termination-config.tf` — конфигурация создаваемой инфраструктуры;
    * `tls-terminationg.auto.tfvars` — файл с пользовательскими данными.
1. В файле `tls-termination.auto.tfvars` задайте пользовательские параметры:
    * `folder_id` — [идентификатор каталога](https://cloud.yandex.ru/docs/resource-manager/operations/folder/get-id).
    * `vm_user` — имя пользователя ВМ.
    * `ssh_key_path` — путь к файлу с публичным SSH-ключом. Подробнее см. [Создание пары ключей SSH](https://cloud.yandex.ru/docs/compute/operations/vm-connect/ssh#creating-ssh-keys).
    * `domain` — домен, на котором будет размещен сайт.
    * `certificate` — путь к файлу с [пользовательским сертификатом](https://cloud.yandex.ru/docs/certificate-manager/operations/import/cert-create#create-file).
    * `private_key` — путь к файлу с закрытым ключом пользовательского сертификата.
1. В терминале перейдите в папку, где вы отредактировали конфигурационный файл.
1. Проверьте корректность конфигурационного файла с помощью команды:

   ```bash
   terraform validate
   ```

   Если конфигурация является корректной, появится сообщение:

   ```text
   Success! The configuration is valid.
   ```

1. Выполните команду:

   ```bash
   terraform plan
   ```

   В терминале будет выведен список ресурсов с параметрами. На этом этапе изменения не будут внесены. Если в конфигурации есть ошибки, Terraform на них укажет.
1. Примените изменения конфигурации:

   ```bash
   terraform apply
   ```

1. Подтвердите изменения: введите в терминале слово `yes` и нажмите **Enter**.
1. Загрузите файлы сайта на ВМ.
1. Проверьте работу хостинга.