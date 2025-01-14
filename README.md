# Magnum Bank Chalenge API2 - mgb-api2-papp

### TECNOLOGIAS UTILIZADAS: MuleSoft, Docker, Docker-Compose, SQL-Server, RabbitMQ

### Pre-requisitos: Docker for Windows.
Realize o download e instalação do Docker para seguir com a implantação do containers das imagens do RabbitMQ e MSSQL.

![image](https://github.com/user-attachments/assets/22f901cf-9905-43ef-85b6-d4c52561b700)

# Instalando a imagem do RabbitMQ

Passo 1: Utilize o comando abaixo para realizar o download da imagem do RabbitMQ no Docker.

    docker pull rabbitmq:3-management

Passo 2: Utilize a o comando abaixo para rodar a imagem do RabbitMQ no  Container Docker.

    docker run -d --name mgb-mq -p 5672:5672 -p 15672:15672 rabbitmq:3-management

Passo 3: Acesse o RabbitMQ Management através do navegador com o endereço: http://localhost:15672/

Passo 4: REalize o login no Rabbit Management com o usuario Guest
    User:  guest
    Pass:  guest

Passo 5: Configurando RabbitMQ

!Importante: Para funcionamento correto de testes locais, é necessario que as imagens dos containers tenham sido criadas com os valores apresentados acima, pois o Mulesfot esta com as properties configuradas para estes valores.

#### Criando a Excchange e a Queue
No RabbitMQ crie uma exchange de nome "ex-mgb-post-marcas" e uma Queue de nome "post.marcas"

![image](https://github.com/user-attachments/assets/ada0a9b5-3915-4bd9-8915-adcb9fd2e389)

#### Configurando o Binding
Após criar a Exchange e a Queue, acesse cada uma delas e faça o Binding de roteamento.
- Na Exchange apontando da exchange "ex-mgb-post-marcas" para a Queue "post.marcas" 
- Na Queue apontando da queue "post.marcas"  para a exchange "ex-mgb-post-marcas"  

![image](https://github.com/user-attachments/assets/1f494535-8005-4bb5-8017-e921a3fee586)


# Instalando a imagem do Microsoft SQL Server 2022

Passo 1: Utilize o comando abaixo para realizar o download da imagem do MSSQL Server 2022.


    docker pull mcr.microsoft.com/mssql/server:2022-latest

Passo 2: Utilize a o comando abaixo para rodar a imagem do MSSQL no  Container Docker.

    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Guest1234$#@!" -p 1433:1433 --name mgb-db --hostname mgb-db -d mcr.microsoft.com/mssql/server:2022-latest

Nota: Após implementado o container com a imagem do MSSQL, basta roda-lo no Docker. 
Não é necessario criar nenhuma tabela, a configuração de criação da tabela ja esta implementada no mulesoft através de um componente Script Database.

![image](https://github.com/user-attachments/assets/20a0bc63-14d5-4c27-bca7-f7ff82641bec)

Desta forma, ao rodar o Mule no anypoint Studio, e chamar o endpoint /carga-inicial
Ele se encarregara de fazer a criação da tabela veiculos e carregar os dados de veiculos recebidos na api da Fipe.

# Chalenge pratico Magnum Bank

System API do desafio Magnum Bank Chalenge 

1. Teste Prático: Tabela FIPE

Este teste avalia suas habilidades em 
- integrar APIs,
- implementar filas para processamento assíncrono
- gerenciar dados em banco SQL.

O objetivo é criar soluções eficientes e bem estruturadas para um cenário de integração com o serviço FIPE.
[Uploading API FIPE.postman_collection.json…]()

---

✅ 1.1 Crie um serviço REST na API-1 para acionar a “carga inicial” dos dados de veículos.
O endpoint "/carga-inicial" é responsavel por realizar a carga inicial dos dados dos veiculos, voce pode baixar e importar a Collection mgb-api-sapi no Postman através do link: 
[mgb-api1-sapi.postman_collection.zip](https://github.com/user-attachments/files/18415685/mgb-api1-sapi.postman_collection.zip)

-> API FIPE: [FIPE API HTTP REST](https://deividfortuna.github.io/fipe/)

✅ 1.2 Implemente a lógica na API-1 para buscar as “marcas” no serviço da FIPE
✅ 1.3 Configure uma “fila” para receber as “marcas” da API-1 e enviar uma por vez para a API-2 para processamento assíncrono.
✅ 1.4 Implemente a lógica na API-2 para buscar os “códigos” e “modelos” dos veículos no serviço da FIPE com base nas “marcas” recebidas da fila.
✅ 1.5 Implemente a lógica na API-2 para salvar no banco de dados “SQL” as informações de "código", "marca" e "modelo" dos veículos encontrados no serviço da FIPE.

### Codigo Mulesoft
Foram criados 4 recursos: 

- api-health: retornar a saude da api
- carga-inicial: Cria/recria a tabela no banco e faz as chamadas na API Fipe ao final salvar os dados no banco de dados SQL.
- buscar-marcas: Busca os veiculos no banco de dados.
- put-veiculo: Atualiza dados de veiculos no banco de dados. 

![image](https://github.com/user-attachments/assets/430c18a3-c6a4-4e5b-bbed-ed9f11ec2a42)

![image](https://github.com/user-attachments/assets/7cad008e-c025-4f60-bb98-70b9df0955b4)

![image](https://github.com/user-attachments/assets/79e6d0f0-ed46-4dec-bb8d-7378078b8cb1)

![image](https://github.com/user-attachments/assets/20a82224-c100-4da8-96e3-0315e2e7e5fa)

![image](https://github.com/user-attachments/assets/ad08131f-bef6-4a31-98fe-bf1fc6ddf304)


### Curl:

curl --location 'http://localhost:8081/api/veiculos/carga-inicial'

✅ 2 Crie um serviço REST na API-1 para buscar as "marcas" armazenadas no banco de dados.
✅ 3 Crie um serviço REST na API-1 para buscar os "códigos", "modelos" e “observações” dos
veículos por "marca" no banco de dados.

### Codigo Mulesoft

![image](https://github.com/user-attachments/assets/12461730-21e2-448e-833a-c4ed99c013d9)

### Curl:

curl --location --request GET 'http://localhost:8081/api/veiculos?marca=Acura' \
--header 'Content-Type: application/json' \
--data '{}'

![image](https://github.com/user-attachments/assets/4f768607-e6ab-45b3-9943-64d97446b2f9)


✅ 4 Crie um serviço REST na API-1 para salvar os dados alterados do veículo, como: "modelo" e
“observações” no banco de dados.

### Codigo Mulesoft

![image](https://github.com/user-attachments/assets/b506322a-b84b-408a-b1fb-f9e23d4cf47e)

### Curl:

curl --location --request PUT 'http://localhost:8081/api/veiculos/038003-2' \
--header 'Content-Type: application/json' \
--data '    {
        "Valor": "R$ 999.999,99",
        "CodigoModelo": "999",
        "SiglaCombustivel": "E"
    }'

![image](https://github.com/user-attachments/assets/e77795d5-9e7b-4a22-a976-16684bae3cec)


## TECNOLOGIAS UTILIZADAS: MuleSoft, Docker, Docker-Compose, SQL-Server, RabbitMQ
