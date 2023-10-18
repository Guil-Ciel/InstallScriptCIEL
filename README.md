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
```
##### 2. Modifique os parâmetros:
  Existem algumas configurações que podemos mudar, segue a lista: :<br/>
```OE_USER```. Será o nome de usuário para o usuário do sistema.<br/>
```GENERATE_RANDOM_PASSWORD```. Se isso estiver definido como True, o script gerará uma senha aleatória; se definido como False, a senha será configurada em ```OE_SUPERADMIN.``` O valor padrão é True, e o script gerará uma senha aleatória e segura.<br/>
```OE_SUPERADMIN``` É a senha principal para esta instalação do Odoo. Caso queira modificar a senha, certifique-se de usar letras maiúsculas e minúsculas, números e caracteres especiais, ou ele continuará gerando uma senha aleatória por ser mais "segura".<br/>
```INSTALL_WKHTMLTOPDF``` Definido como False se você não quiser instalar o [Wkhmtltopdf](https://wkhtmltopdf.org/usage/wkhtmltopdf.txt); definido como True se desejar instalá-lo. Essa é uma ferramentas de linha de comando para renderizar HTML em PDF e vários formatos de imagem usando o Qt WebKit 
```OE_PORT``` É a porta em que o Odoo deve ser executado, por exemplo, 8069.<br/>
```OE_VERSION``` É a versão do Odoo a ser instalada, por exemplo, 16.0 para Odoo V16.<br/>
```IS_ENTERPRISE``` Instalará a versão Enterprise sobre 16.0 se definido como True, ou a versão da comunidade do Odoo 16 se definido como False.<br/>
```INSTALL_NGINX``` é definido como False por padrão. Defina como True se desejar instalar o Nginx.<br/>
```WEBSITE_NAME```Defina o nome do site aqui para a configuração do Nginx.<br/>
```ENABLE_SSL``` Defina como True para instalar [certbot](https://github.com/certbot/certbot) e configurar o Nginx com https usando um certificado gratuito do Let's Encrypt.<br/>
```ADMIN_EMAIL``` O e-mail é necessário para o registro do Let's Encrypt. Substitua o espaço reservado padrão por um e-mail da sua organização.<br/>
```INSTALL_NGINX``` e ´´´ENABLE_SSL´´´ devem ser definidos como True e o espaço reservado em ´´´ADMIN_EMAIL´´´ deve ser substituído por um endereço de e-mail válido para a instalação do certbot.<br/>
  _Ao ativar o SSL por meio do Let's Encrypt, você concorda com as seguintes políticas <br/>

#### 3. Execute o script
```
sudo ./odoo_install.sh

Aguarde até o final, pressionando ENTER e digitando "Y" sempre que o script for pedindo. ao finalizar,
você deve receber uma mensagem no terminal assim:

Done! The Odoo server is up and running. Specifications:
Port: ```OE_PORT```.
User service: ```OE_USER```.
Configuraton file location: /etc/odoo-server.conf
Logfile location: /var/log/odoo
User PostgreSQL: ```OE_USER```.
Code location: ```OE_USER```.
Addons folder: ```OE_USER```./odoo-server/addons/
Password superadmin (database): "Sua senha colocada nos parâmetros" / Uma senha aleatória, exemplo: J4EzGDdGYvq8Eddz
Start Odoo service: sudo service odoo-server start
Stop Odoo service: sudo service odoo-server stop
Restart Odoo service: sudo service odoo-server restart
```
##### 4. Inicie os serviços:
```
Como está instalação foi criada em WSL usando o Ubuntu disponivel na Microsoft Store, para iniciar os serviços ```odoo-server``` e ```postgresql```, digite:
sudo service odoo-server start
sudo service postgresql start

Entretanto, caso esteja fazendo esse processo em uma máquina linux, o comando pode alterar para systemctl, sendo assim:
sudo systemctl odoo-server start
sudo systemctl postgresql start
```

Iniciando o serviços, seu odoo já estará rodando local/cloud e pode ser acessado por localhost:8069 ou "Seu ip do cloud":8069 (possivelmente o Cloud pode ter problemas com a porta, precisando ser liberada no firewall do mesmo)

##### 5. Troubleshoot:

Ao finalizar a instalação, podemos nós deparar com alguns problemas. Os mais comuns entre eles são:

### 1 - Database creation error: Access Denied
  Se  você está recebendo essa mensagem, a senha do ```OE_SUPERADMIN``` não foi definida, precisando ser alterada seguindo o seguinte caminho:
  cd /etc/
  E alterando o seguinte arquivo:
  nano odoo-server.conf
  Mudando a linha ```admin_passwd =``` e apertando CTRL+X para sair do arquivo, "Y" para confirmar a mudança que fizemos na senha e "Enter" para sair.


## Where should I host Odoo?
There are plenty of great services that offer good hosting. The script has been tested with a few major players such as [Google Cloud](https://cloud.google.com/), [Hetzner](https://www.hetzner.com/), [Amazon AWS](https://aws.amazon.com/) and [DigitalOcean](https://www.digitalocean.com/products/droplets/).
If you'd like you can use my [DigitalOcean referral link](https://m.do.co/c/d605cc420682) which gives you a 200$ voucher for free for the first 60 days.

## Minimal server requirements
While technically you can run an Odoo instance on 1GB (1024MB) of RAM it is absolutely not advised. A Linux instance typically uses 300MB-500MB and the rest has to be split among Odoo, postgreSQL and others. If you install an Odoo you should make sure to use at least 2GB of RAM. This script might fail with less resources too.
There are known issues on DigitalOcean for example where the installation crashes on 1GB RAM machines. See https://github.com/Yenthe666/InstallScript/issues/243

