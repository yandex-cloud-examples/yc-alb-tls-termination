# Terminating TLS connections through Yandex Application Load Balancer

Yandex Application Load Balancer offers you [L7 load balancers](https://yandex.cloud/en/docs/application-load-balancer/concepts/application-load-balancer) that can _terminate_ TLS connections: send certificates to clients, decrypt incoming traffic to send to the [backends](https://yandex.cloud/en/docs/application-load-balancer/concepts/backend-group), and encrypt backend responses prior to forwarding them to clients. 

Follow this tutorial to configure a load balancer to terminate TLS connections using a [certificate](https://yandex.cloud/en/docs/certificate-manager/concepts/) from [Yandex Certificate Manager](https://yandex.cloud/en/docs/certificate-manager) and to redirect HTTP requests to HTTPS.

For details, see this step-by-step guide in the Yandex Cloud documentation: [Terminating TLS connections](https://yandex.cloud/en/docs/application-load-balancer/tutorials/tls-termination).

For more information about the provider resources, see the documentation on the [Terraform website](https://www.terraform.io/docs/providers/yandex/index.html) or [its mirror](https://terraform-provider.yandexcloud.net/).

To create the infrastructure for terminating TLS connections:

1. [Install Terraform](https://yandex.cloud/en/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform), [get the authentication credentials](https://yandex.cloud/en/docs/tutorials/infrastructure-management/terraform-quickstart#get-credentials), and specify the source for installing the Yandex Cloud provider (see [Step 1: Configuring a provider](https://yandex.cloud/en/docs/tutorials/infrastructure-management/terraform-quickstart#configure-provider)).

1. Clone the repository with the configuration files:

    ```bash
    git clone https://github.com/yandex-cloud-examples/yc-alb-tls-termination.git
    ```

1. Go to the directory with the repository. Make sure it contains the following files:
    * `tls-termination-config.tf`: New infrastructure configuration.
    * `tls-terminationg.auto.tfvars`: User data file.
1. In the `tls-termination.auto.tfvars` file, set the following user-defined properties:
    * `folder_id`: [Folder](https://yandex.cloud/en/docs/resource-manager/operations/folder/get-id) ID.
    * `vm_user`: VM user name.
    * `ssh_key_path`: Path to the file with the public SSH key. For details, see [Creating an SSH key pair](https://yandex.cloud/en/docs/compute/operations/vm-connect/ssh#creating-ssh-keys).
    * `domain`: Domain to host the website.
    * `certificate`: Path to the file with the [user certificate](https://yandex.cloud/en/docs/certificate-manager/operations/import/cert-create#create-file).
    * `private_key`: Path to the file with the user certificate's private key.
1. In the terminal, go to the folder where you edited the configuration file.
1. Make sure the configuration file is correct using this command:

   ```bash
   terraform validate
   ```

   If the configuration is correct, you will get this message:

   ```text
   Success! The configuration is valid.
   ```

1. Run this command:

   ```bash
   terraform plan
   ```

   The terminal will display a list of resources with parameters. No changes will be made at this step. If there are errors in the configuration, Terraform will point them out.
1. Apply the configuration changes:

   ```bash
   terraform apply
   ```

1. Confirm the changes: type `yes` into the terminal and press **Enter**.
1. Upload the website files to the VM.
1. Test the hosting.
