# Relatório
Aqui vou detalhar o passo a passo para colocar todo o sistema (frontend, backend e banco de dados) para funcionar nas aws. 

## RDS
Primeramente, comecei criando um RDS na AWS. Selecionei o "Engyne type" como Postgres e deixei marcado o "Engine version" automaticamente selecionado. Selecionei o Templates como o "Free tier" para não gastar muito dinheiro. Coloquei o "DB instance identifier" como "prova2-mod7". "Master Username" deixei como "postgres". Em senhas, coloquei "minhasenha". Em conectividade marquei "Public Access" como "Yes" para permitir o acesso ao banco de dados e em "VPC Security group" deixei marcado o Default. O resto das opções deixei default. 
Após criado o RDS, fui no Security Group selecionado para ele e abri conexão TCP para a porta 5432 para todos os endereços (0.0.0.0).

## Backend
Primeiramente, mudei os seguintes códigos da backend para as configurações corretas do banco de dados RDS criado:

### main.py
````
  DB_USER = "postgres"
  DB_PASSWORD = "minhasenha"
  DB_HOST = "prova2-mod7.clvym8x1eppf.us-east-1.rds.amazonaws.com"
  DB_PORT = "5432"
  DB_NAME = "postgres"
````

### criar_banco.py
````
  DB_USER = "postgres"
  DB_PASSWORD = "minhasenha"
  DB_HOST = "prova2-mod7.clvym8x1eppf.us-east-1.rds.amazonaws.com"
  DB_PORT = "5432"
  DB_NAME = "postgres"
````

Depois, instalei as dependências localmente e tentei rodar o arquivo criar_banco.py para testar a conexão com o RDS.

````
  cd backend
  python3 criar_banco.py
````

Como o terminal fechou sem nenhum log de erro, assumi que a criação de tabelas foi realizada com sucesso.

### AWS
Depois, naveguei até o serviço de EC2 da AWS e comecei a criar um instância. Vou listar aqui apenas as configurações modificadas:

Name: "prova2-mod7-backend" <br/>
OS Image: Ubuntu <br/>
Architecture: "Arm" <br/>
Key pair: Selecionei um que já havia criado <br/>
Security group: Selecionei um que já havia criado <br/>

Depois, abri a porta 8000 para conexões TCP para todas os endereços (0.0.0.0) nas configurações do security group que havia selecionado.

Depois, subi todo o código que foi atualizado para meu repositório.

Próximo passo foi me conectar a instância EC2 por meio de ssh:
````
  ssh -i ./sec.pem ubuntu@ec2-54-175-27-81.compute-1.amazonaws.com
````

# Template para avaliação P2

Saída esperada após execução do programa:

<img src="./media/tela-front.png" display="flex">

# IMPORTANTE:

Para colocar o frontend para funcionar, colocar uma máquina EC2 rodando o Apache WebServer.
Para isso, instalar dentro da EC2:

```bash
sudo apt update
sudo apt upgrade
sudo apt install apache2
# os arquivos do projeto devem estar em /var/www/html
git clone https://github.com/Murilo-ZC/Avaliacao-P2-M7-2023-EC.git
sudo cp ./Avaliacao-P2-M7-2023-EC/frontend /var/www/html
```

Aqui pessoal, os arquivos já estaram disponíveis na porta 80, não necessário redirecionar.

> IMPORTANTE: Verificar as rotas e utilziar o seu próprio repositório com as modificações realizadas.
