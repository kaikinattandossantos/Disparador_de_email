# Microsserviço de Envio de E-mails com AWS SES 

![Java](https://img.shields.io/badge/Java-17-blue?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3.3-green?style=for-the-badge&logo=spring)
![AWS](https://img.shields.io/badge/AWS-SES_&_IAM-orange?style=for-the-badge&logo=amazon-aws)
![Maven](https://img.shields.io/badge/Maven-4.0.0-red?style=for-the-badge&logo=apache-maven)

Um microsserviço robusto desenvolvido em **Java** e **Spring Boot** para o envio de e-mails transacionais de forma simples e escalável, utilizando o **Amazon Simple Email Service (SES)**.

---

## 📋 Índice

- [Funcionalidades](#-funcionalidades)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Pré-requisitos](#-pré-requisitos)
- [Configuração do Ambiente](#️-configuração-do-ambiente)
- [Como Executar a Aplicação](#-como-executar-a-aplicação)
- [Uso da API](#-uso-da-api)
- [Integração com Serviços AWS](#-integração-com-serviços-aws)
- [Estrutura do Projeto](#-estrutura-do-projeto)

---

## ✨ Funcionalidades

-   **Endpoint único**: Uma API RESTful com um endpoint `POST` para solicitar o envio de e-mails.
-   **Integração com AWS SES**: Utiliza o poder e a confiabilidade do serviço de e-mail da AWS para garantir alta taxa de entrega.
-   **Arquitetura Limpa**: O projeto é estruturado em camadas (Controllers, Application, Core, Infra) para facilitar a manutenção e escalabilidade.
-   **Tratamento de Erros**: Captura exceções durante o envio e retorna respostas HTTP adequadas.

---

## 🛠️ Tecnologias Utilizadas

-   **Java 17**: Versão LTS mais recente da linguagem Java.
-   **Spring Boot 3.3.3**: Framework para criação de aplicações stand-alone e baseadas em micro-serviços.
-   **Spring Web**: Para criação de endpoints RESTful.
-   **Apache Maven**: Gerenciador de dependências e build do projeto.
-   **AWS SDK for Java v1**: Para integração com os serviços da Amazon Web Services.
-   **Lombok**: Para reduzir código boilerplate (getters, setters, construtores).

---

## 📦 Pré-requisitos

Antes de começar, você vai precisar ter instalado em sua máquina:

-   [JDK 17 ou superior](https://www.oracle.com/java/technologies/downloads/#java17)
-   [Apache Maven 3.x](https://maven.apache.org/download.cgi)
-   Uma conta na [Amazon Web Services (AWS)](https://aws.amazon.com/)

---

## ⚙️ Configuração do Ambiente

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/kaikinattandossantos/Disparador_de_email.git
    cd Disparador_de_email.git
    ```

2.  **Configure as variáveis de ambiente:**
    Na raiz do projeto, você encontrará o arquivo `src/main/resources/application.properties`. Preencha com suas credenciais da AWS e configurações.

    > **⚠️ Importante:** É uma boa prática não commitar suas credenciais diretamente no código. Considere usar variáveis de ambiente do sistema ou o AWS Secrets Manager para ambientes de produção.

    ```properties
    # Nome da aplicação
    spring.application.name=microsservico

    # Credenciais e Configuração da AWS
    # Substitua pelos seus valores
    aws.accessKeyId=SUA_ACCESS_KEY_ID
    aws.secretKey=SUA_SECRET_KEY
    aws.region=us-east-1

    # E-mail de origem verificado no AWS SES
    aws.ses.source-email=seu-email-verificado@seudominio.com
    ```

---

## ▶️ Como Executar a Aplicação

Para iniciar o microsserviço, execute o seguinte comando na raiz do projeto:

```bash
./mvnw spring-boot:run
```

A aplicação estará disponível em `http://localhost:8080`.

---

## 📬 Uso da API

Para enviar um e-mail, faça uma requisição `POST` para o endpoint `/api/email/send`.

**Endpoint:** `POST /api/email/send`

**Request Body (JSON):**

```json
{
  "to": "destinatario@exemplo.com",
  "subject": "Assunto do E-mail",
  "body": "Este é o corpo da mensagem que você deseja enviar."
}
```

**Exemplo com cURL:**

```bash
curl -X POST http://localhost:8080/api/email/send \
-H "Content-Type: application/json" \
-d '{
      "to": "destinatario@exemplo.com",
      "subject": "Teste via API",
      "body": "Olá! Este é um e-mail enviado pelo microsserviço."
    }'
```

### Respostas da API

-   **`200 OK`**: O pedido de envio de e-mail foi processado com sucesso.
    ```
    Email sent successfully!
    ```
-   **`500 Internal Server Error`**: Ocorreu um erro ao tentar enviar o e-mail. Verifique os logs da aplicação para mais detalhes.
    ```
    Email sending failed.
    ```

---

## ☁️ Integração com Serviços AWS

Este projeto utiliza dois serviços principais da AWS:

### Amazon SES (Simple Email Service)

É o serviço responsável pelo envio dos e-mails. Para que a aplicação funcione, você precisa ter uma **identidade verificada** (seja um domínio ou um endereço de e-mail específico) na sua conta do SES. O e-mail configurado em `aws.ses.source-email` deve ser uma dessas identidades verificadas.



### AWS IAM (Identity and Access Management)

Para garantir a segurança, as credenciais (`aws.accessKeyId` e `aws.secretKey`) devem pertencer a um **usuário IAM** com permissões mínimas necessárias. A política recomendada para este usuário é conceder apenas a permissão para enviar e-mails via SES.

**Exemplo de Política IAM (JSON):**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": "*"
        }
    ]
}
```

---

## 🏗️ Estrutura do Projeto

O projeto segue uma arquitetura baseada em camadas para separar as responsabilidades:

-   `com.base.demo.controllers`: Camada de entrada da aplicação (REST Controllers).
-   `com.base.demo.application`: Contém os serviços da aplicação e a lógica de orquestração.
-   `com.base.demo.core`: O núcleo do domínio, contendo as entidades, casos de uso (interfaces) e exceções.
-   `com.base.demo.infra`: Camada de infraestrutura, com implementações concretas de interfaces externas, como o cliente do AWS SES e configurações.

---

