# Order Management Platform

## Descrição

Plataforma de gerenciamento de pedidos utilizando RabbitMQ, PostgreSQL e Redis para processamento e despacho de ordens.
O projeto é composto por múltiplos serviços que se comunicam via filas de mensagens, garantindo escalabilidade e
resiliência.

## Arquitetura

![Arquitetura do Projeto](https://github.com/thoggs/order-management-platform/blob/main/img/project_architecture.png)

## Configuração das Variáveis de Ambiente

### RabbitMQ

```env
RABBITMQ_DEFAULT_USER=admin
RABBITMQ_DEFAULT_PASS=secret
RABBITMQ_LOAD_DEFINITIONS=/etc/rabbitmq/definitions.json
```

### Banco de Dados Order-DB

```env
POSTGRES_USER=order_user
POSTGRES_PASSWORD=order_password
POSTGRES_DB=order_db
```

### Banco de Dados Dispatch-DB

```env
POSTGRES_USER=dispatch_user
POSTGRES_PASSWORD=dispatch_password
POSTGRES_DB=dispatch_db
```

### Redis

```env
REDIS_HOST=order-cache
REDIS_PORT=6379
```

## Como Subir o Projeto

1. Clone o repositório:
   ```sh
   git clone https://github.com/thoggs/order-management-platform.git
   cd order-management-platform
   ```

2. Configure as variáveis de ambiente:
    - Edite os arquivos em `environments/` para ajustar as credenciais conforme necessário.

3. Suba os containers com Docker Compose:
   ```sh
   docker-compose up -d
   ```

4. Verifique se os containers estão rodando corretamente:
   ```sh
   docker ps
   ```

5. Definir senha do RabbitMQ:
   ```sh
   docker exec -it order-message-broker rabbitmqctl change_password admin secret
   ```

6. Para acessar o RabbitMQ Management UI:
    - URL: [http://localhost:15672](http://localhost:15672)
    - Usuário: `admin`
    - Senha: `secret`

7. Para acessar o banco de dados Order-DB:
   ```sh
   docker exec -it order-db psql -U order_user -d order_db
   ```

8. Para acessar o banco de dados Dispatch-DB:
   ```sh
   docker exec -it dispatch-db psql -U dispatch_user -d dispatch_db
   ```

9. Para testar o Redis:
   ```sh
   docker exec -it order-cache redis-cli
   set test_key "Hello, Redis!"
   get test_key
   ```

## Considerações Finais

O projeto está pronto para uso. Certifique-se de configurar corretamente todas as variáveis de ambiente antes de
executar os containers. Caso encontre problemas, verifique os logs com:

```sh
docker-compose logs -f
```

