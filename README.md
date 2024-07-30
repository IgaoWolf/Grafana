# Grafana

# Instalação Simplificada do Prometheus e Grafana

Este guia fornece um passo a passo para instalar e configurar o Prometheus e o Grafana em um servidor Ubuntu. O Prometheus é um sistema de monitoramento e alerta, enquanto o Grafana é uma plataforma de visualização e análise de dados.

## 1. Instalando os Repositórios

Primeiro, instale os pacotes necessários para adicionar repositórios e obtenha a chave GPG para o repositório do Grafana:

```bash
sudo apt-get install -y apt-transport-https software-properties-common wget

sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

Atualize a lista de pacotes disponíveis:

```bash

sudo apt-get update
```
2. Instalando o Grafana

Instale o Grafana:

```bash

sudo apt-get install grafana
```

3. Instalando o MySQL

Instale o MySQL e execute o script de configuração segura:

```bash

sudo apt install mysql-server -y
sudo mysql_secure_installation
```

4. Configurando o Banco de Dados MySQL para o Grafana

Acesse o MySQL como root:

```bash

sudo mysql -u root -p
```
Dentro do MySQL:

```sql

CREATE DATABASE grafana;
CREATE USER 'grafana'@'localhost' IDENTIFIED BY 'grafana-passwd';
GRANT ALL PRIVILEGES ON grafana.* TO 'grafana'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
5. Configurando o Grafana para Usar o MySQL

Edite o arquivo de configuração do Grafana para definir o banco de dados MySQL:

```bash

sudo vim /etc/grafana/grafana.ini
```

Localize a seção [database] e ajuste as configurações para:


[database]
type = mysql
host = 127.0.0.1:3306
name = grafana
user = grafana
password = grafana-passwd

6. Iniciando e Habilitando o Grafana

Inicie o Grafana e configure-o para iniciar automaticamente com o sistema:

```bash

sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

7. Acessando o Grafana

Abra um navegador e acesse a interface web do Grafana para finalizar a configuração:

http://<IP-SERVER>:3000

Faça login com as credenciais padrão:

    Usuário: admin
    Senha: admin

Substitua <IP-SERVER> pelo endereço IP do seu servidor.
