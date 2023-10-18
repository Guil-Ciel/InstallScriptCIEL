# [Odoo](https://www.odoo.com "Odoo's Homepage") Install Script

Este script é baseado no script de instalação de André Schenkels (https://github.com/aschenkels-ictstudio/openerp-install-scripts), mas vai um pouco mais longe e foi aprimorado. Este script também lhe dará a capacidade de definir uma porta xmlrpc_port no arquivo .conf que é gerado em /etc/. Este script pode ser usado com segurança em um servidor de base de código multi-Odoo porque a porta padrão do Odoo é alterada ANTES que o Odoo seja iniciado.

## Instalação do Nginx
Se você definir o parâmetro ```INSTALL_NGINX``` como ```True```, você também deve configurar os workers. Sem os workers, é provável que você tenha problemas de perda de conexão. Consulte o guia [the deployment guide from Odoo](https://www.odoo.com/documentation/16.0/administration/install/deploy.html) de implantação do Odoo sobre como configurar os workers.

## Processos de Instalação

##### 1. Baixe a pasta do script:
```
Já dentro da sua VM/Cloud/Terminal, copie e de enter:
git clone https://github.com/guilbezerra/InstallScriptCIEL.git
```

##### 2. Permissão de execução:
```
Comece dando a permissão de execução para os dois scripts que iremos usar:
sudo chmod +x odoo_install.sh
sudo chmod +x create_role.sh
```
##### 2. Modifique os parâmetros.
Existem algumas configurações que podemos mudar, segue a lista: There are a few things you can configure, this is the most used list:<br/>
```OE_USER``` will be the username for the system user.<br/>
```GENERATE_RANDOM_PASSWORD``` if this is set to ```True``` the script will generate a random password, if set to ```False```we'll set the password that is configured in ```OE_SUPERADMIN```. By default the value is ```True``` and the script will generate a random and secure password.<br/>
```INSTALL_WKHTMLTOPDF``` set to ```False``` if you do not want to install Wkhtmltopdf, if you want to install it you should set it to ```True```.<br/>
```OE_PORT``` is the port where Odoo should run on, for example 8069.<br/>
```OE_VERSION``` is the Odoo version to install, for example ```16.0``` for Odoo V16.<br/>
```IS_ENTERPRISE``` will install the Enterprise version on top of ```16.0``` if you set it to ```True```, set it to ```False``` if you want the community version of Odoo 16.<br/>
```OE_SUPERADMIN``` is the master password for this Odoo installation.<br/>
```INSTALL_NGINX``` is set to ```False``` by default. Set this to ```True``` if you want to install Nginx.<br/>
```WEBSITE_NAME``` Set the website name here for nginx configuration<br/>
```ENABLE_SSL``` Set this to ```True``` to install [certbot](https://github.com/certbot/certbot) and configure nginx with https using a free Let's Encrypted certificate<br/>
```ADMIN_EMAIL``` Email is needed to register for Let's Encrypt registration. Replace the default placeholder with an email of your organisation.<br/>
```INSTALL_NGINX``` and ```ENABLE_SSL``` must be set to ```True``` and the placeholder in ```ADMIN_EMAIL``` must be replaced with a valid email address for certbot installation<br/>

Existem algumas configurações que podemos mudar, segue a lista: :<br/>
´´´OE_USER´´´ será o nome de usuário para o usuário do sistema.<br/>
´´´GENERATE_RANDOM_PASSWORD´´´ se isso estiver definido como True, o script gerará uma senha aleatória; se definido como False, a senha será configurada em OE_SUPERADMIN. O valor padrão é True e o script gerará uma senha aleatória e segura.<br/>
´´´INSTALL_WKHTMLTOPDF´´´ definido como False se você não quiser instalar o Wkhtmltopdf; definido como True se desejar instalá-lo.
´´´OE_PORT´´´ é a porta em que o Odoo deve ser executado, por exemplo, 8069.<br/>
´´´OE_VERSION´´´ é a versão do Odoo a ser instalada, por exemplo, 16.0 para Odoo V16.<br/>
´´´IS_ENTERPRISE´´´ instalará a versão Enterprise sobre 16.0 se definido como True, ou a versão da comunidade do Odoo 16 se definido como False.<br/>
´´´OE_SUPERADMIN´´´ é a senha principal para esta instalação do Odoo.<br/>
´´´INSTALL_NGINX´´´ é definido como False por padrão. Defina como True se desejar instalar o Nginx.<br/>
´´´WEBSITE_NAME´´´ Defina o nome do site aqui para a configuração do Nginx.<br/>
´´´ENABLE_SSL´´´ Defina como True para instalar certbot e configurar o Nginx com https usando um certificado gratuito do Let's Encrypt.<br/>
´´´ADMIN_EMAIL´´´ O e-mail é necessário para o registro do Let's Encrypt. Substitua o espaço reservado padrão por um e-mail da sua organização.<br/>
´´´INSTALL_NGINX´´´ e ´´´ENABLE_SSL´´´ devem ser definidos como True e o espaço reservado em ´´´ADMIN_EMAI´´´L deve ser substituído por um endereço de e-mail válido para a instalação do certbot.<br/>
  _Ao ativar o SSL por meio do Let's Encrypt, você concorda com as seguintes políticas <br/>

#### 3. Make the script executable
```
sudo chmod +x odoo_install.sh
```
##### 4. Execute the script:
```
sudo ./odoo_install.sh
```

## Where should I host Odoo?
There are plenty of great services that offer good hosting. The script has been tested with a few major players such as [Google Cloud](https://cloud.google.com/), [Hetzner](https://www.hetzner.com/), [Amazon AWS](https://aws.amazon.com/) and [DigitalOcean](https://www.digitalocean.com/products/droplets/).
If you'd like you can use my [DigitalOcean referral link](https://m.do.co/c/d605cc420682) which gives you a 200$ voucher for free for the first 60 days.

## Minimal server requirements
While technically you can run an Odoo instance on 1GB (1024MB) of RAM it is absolutely not advised. A Linux instance typically uses 300MB-500MB and the rest has to be split among Odoo, postgreSQL and others. If you install an Odoo you should make sure to use at least 2GB of RAM. This script might fail with less resources too.
There are known issues on DigitalOcean for example where the installation crashes on 1GB RAM machines. See https://github.com/Yenthe666/InstallScript/issues/243

