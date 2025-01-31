# Order Management Platform

## Descrição

Plataforma de gerenciamento de pedidos baseada em microserviços, utilizando RabbitMQ, PostgreSQL e Redis para
processamento e despacho de ordens. A comunicação entre serviços é feita via filas de mensagens, garantindo
escalabilidade e resiliência.

## Arquitetura

![Arquitetura do Projeto](https://github.com/thoggs/order-management-platform/blob/main/img/project_architecture.png)

## Como Subir o Projeto

### 1. Clonar o repositório

   ```sh
   git clone https://github.com/thoggs/order-management-platform.git
   ```

### 2. Acesse o diretório do projeto:

   ```sh
   cd order-management-platform
   ```

### 3. Configurar variáveis de ambiente

- Edite os arquivos em `environments/` para ajustar credenciais conforme necessário.

### 4. Subir a infraestrutura (RabbitMQ, PostgreSQL, Redis)

```sh
docker compose -f docker-compose-infra.yml up -d
```

### 5. Definir senha do RabbitMQ

```sh
docker exec -it order-message-broker rabbitmqctl change_password admin secret
```

### 6. Subir os serviços da aplicação

```sh
docker compose -f docker-compose-app.yml up -d
```

Os seguintes serviços Spring Boot são iniciados em background:

- **Order Processor**: [sboot-order-processor](https://github.com/thoggs/sboot-order-processor)
- **Order Dispatcher**: [sboot-order-dispatcher](https://github.com/thoggs/sboot-order-dispatcher)

### 7. Acessos rápidos

#### RabbitMQ Management UI

- URL: [http://localhost:15672](http://localhost:15672)
- Usuário: `admin`
- Senha: `secret`

#### Banco de dados Order-DB

```sh
docker exec -it order-db psql -U order_user -d order_db
```

#### Banco de dados Dispatch-DB

```sh
docker exec -it dispatch-db psql -U dispatch_user -d dispatch_db
```

#### Testar conexão com Redis

```sh
docker exec -it order-cache redis-cli
set test_key "Hello, Redis!"
get test_key
```

## Executando Teste de Carga

Para validar o desempenho da API de pedidos, execute o teste de carga:

1. Clone o repositório do teste de carga:

   ```sh
   git clone https://github.com/thoggs/py-order-burn-test.git
   ```

2. Acesse o diretório do teste de carga:

   ```sh
   cd order-load-test
   ```

3. Configure o ambiente virtual:

   ```sh
   python -m venv venv
   ```

4. Ative o ambiente virtual:

   **Linux/macOS:**

   ```sh
   source venv/bin/activate
   ```

   **Windows (cmd/powershell):**

   ```sh
   venv\Scripts\activate
   ```

5. Instale as dependências:

   ```sh
   pip install -r requirements.txt
   ```

6. Execute o teste de carga:

   ```sh
   python burn_test.py
   ```

O script enviará múltiplas requisições simulando uma carga alta na API.

## Considerações Finais

O projeto está pronto para uso. Certifique-se de configurar corretamente todas as variáveis de ambiente antes de
executar os containers.
