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

!Importante: Para funcionamento correto de testes locais.

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

---

✅ 1.1 Crie um serviço REST na API-1 para acionar a “carga inicial” dos dados de veículos.


✅ 1.2 Implemente a lógica na API-1 para buscar as “marcas” no serviço da FIPE


- API FIPE: [FIPE API HTTP REST](https://deividfortuna.github.io/fipe/)

✅ 1.3 Configure uma “fila” para receber as “marcas” da API-1 e enviar uma por vez para a API-2 para processamento assíncrono.


✅ 1.4 Implemente a lógica na API-2 para buscar os “códigos” e “modelos” dos veículos no serviço da FIPE com base nas “marcas” recebidas da fila.


✅ 1.5 Implemente a lógica na API-2 para salvar no banco de dados “SQL” as informações de "código", "marca" e "modelo" dos veículos encontrados no serviço da FIPE.


✅ 2 Crie um serviço REST na API-1 para buscar as "marcas" armazenadas no banco de dados.


✅ 3 Crie um serviço REST na API-1 para buscar os "códigos", "modelos" e “observações” dos
veículos por "marca" no banco de dados.


✅ 4 Crie um serviço REST na API-1 para salvar os dados alterados do veículo, como: "modelo" e
“observações” no banco de dados.


## TECNOLOGIAS UTILIZADAS: MuleSoft, Docker, Docker-Compose, SQL-Server, RabbitMQ

### Observações: 
Importante aplicar os conceitos e boas práticas de desenvolvimento e engenharia de software, ex.: padrões REST, SOLID, DDD, TDD e Design Patterns, - Contract First (Swagger), Clean Architecture, Clean Code, etc.

Estes requisitos são importantes para avaliação.

Prazo para Entrega 5 dias
