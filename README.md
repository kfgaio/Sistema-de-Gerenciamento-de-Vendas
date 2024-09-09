# Sistema de Gerenciamento de Vendas

## Descrição do Projeto
Este projeto implementa um sistema de gerenciamento de vendas para uma loja, utilizando um banco de dados relacional. O sistema é capaz de armazenar e gerenciar informações sobre produtos, clientes e vendas realizadas, permitindo a administração eficiente dos dados e a geração de relatórios.

## Funcionalidades

- **Criação do Banco de Dados**: Criação de um banco de dados chamado `sistema_vendas`.
- **Criação de Tabelas**: Tabelas para armazenar informações de clientes, produtos e vendas.
- **Inserção de Dados**: Adição de registros de clientes, produtos e vendas.
- **Consultas e Relatórios**: Consultas para listar vendas, compras de clientes específicos e total de vendas por produto.
- **Atualização e Deleção de Dados**: Atualização de informações de clientes e estoque de produtos; deleção de vendas e clientes.
  
## Requisitos do Projeto

1. **Criação do Banco de Dados**:
   - Crie um banco de dados chamado `sistema_vendas`.

2. **Criação das Tabelas**:
   - Tabela de Clientes: Armazena informações como ID, nome, email e telefone.
   - Tabela de Produtos: Armazena informações como ID, nome, preço e quantidade em estoque.
   - Tabela de Vendas: Registra as vendas realizadas, incluindo ID da venda, ID do cliente, ID do produto, quantidade vendida e data da venda.

3. **Inserção de Dados**:
   - Insira pelo menos 3 clientes, 3 produtos e 5 vendas no banco de dados.

4. **Consultas e Relatórios**:
   - Listar todas as vendas realizadas, incluindo o nome do cliente e do produto.
   - Listar todas as compras realizadas por um cliente específico.
   - Exibir o total de vendas realizadas por produto.

5. **Atualização e Deleção de Dados**:
   - Atualizar o estoque de um produto após uma venda.
   - Atualizar as informações de um cliente.
   - Deletar uma venda e, se necessário, deletar o cliente associado.

## Como Executar

1. **Copie e cole o código SQL** abaixo em seu ambiente de gerenciamento de banco de dados (como MySQL Workbench, phpMyAdmin ou outro):
   
   ```sql
   -- 1. Criação do Banco de Dados
   CREATE DATABASE sistema_vendas;
   USE sistema_vendas;

   -- 2. Criação das Tabelas
   CREATE TABLE clientes (
       id_cliente INT AUTO_INCREMENT PRIMARY KEY,
       nome VARCHAR(100),
       email VARCHAR(100),
       telefone VARCHAR(20)
   );

   CREATE TABLE produtos (
       id_produto INT AUTO_INCREMENT PRIMARY KEY,
       nome VARCHAR(100),
       preco DECIMAL(10, 2),
       quantidade_estoque INT
   );

   CREATE TABLE vendas (
       id_venda INT AUTO_INCREMENT PRIMARY KEY,
       id_cliente INT,
       id_produto INT,
       quantidade_vendida INT,
       data_venda DATE,
       FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente),
       FOREIGN KEY (id_produto) REFERENCES produtos(id_produto)
   );

   -- 3. Inserção de Dados
   INSERT INTO clientes (nome, email, telefone) VALUES
   ('Maria Silva', 'maria.silva@example.com', '1234-5678'),
   ('João Souza', 'joao.souza@example.com', '2345-6789'),
   ('Ana Oliveira', 'ana.oliveira@example.com', '3456-7890');

   INSERT INTO produtos (nome, preco, quantidade_estoque) VALUES
   ('Produto A', 10.00, 100),
   ('Produto B', 20.00, 200),
   ('Produto C', 30.00, 300);

   INSERT INTO vendas (id_cliente, id_produto, quantidade_vendida, data_venda) VALUES
   (1, 1, 2, '2024-09-01'),
   (2, 2, 1, '2024-09-02'),
   (3, 3, 3, '2024-09-03'),
   (1, 2, 2, '2024-09-04'),
   (2, 1, 1, '2024-09-05');

   -- 4. Consultas e Relatórios
   -- Consulta de Todas as Vendas
   SELECT vendas.id_venda, clientes.nome AS nome_cliente, produtos.nome AS nome_produto, vendas.quantidade_vendida, vendas.data_venda
   FROM vendas
   JOIN clientes ON vendas.id_cliente = clientes.id_cliente
   JOIN produtos ON vendas.id_produto = produtos.id_produto;

   -- Consulta de Compras por Cliente Específico
   SELECT vendas.id_venda, produtos.nome AS nome_produto, vendas.quantidade_vendida, vendas.data_venda
   FROM vendas
   JOIN produtos ON vendas.id_produto = produtos.id_produto
   WHERE vendas.id_cliente = 1; -- Substitua o '1' pelo id_cliente desejado

   -- Consulta do Total de Vendas por Produto
   SELECT produtos.nome, SUM(vendas.quantidade_vendida) AS total_vendido
   FROM vendas
   JOIN produtos ON vendas.id_produto = produtos.id_produto
   GROUP BY produtos.nome;

   -- 5. Atualização e Deleção de Dados
   -- Atualização do Estoque após uma Venda
   UPDATE produtos
   SET quantidade_estoque = quantidade_estoque - 2 -- Substitua '2' pela quantidade vendida
   WHERE id_produto = 1; -- Substitua '1' pelo id_produto vendido

   -- Atualização das Informações de um Cliente
   UPDATE clientes
   SET nome = 'Maria Santos', email = 'maria.santos@example.com'
   WHERE id_cliente = 1; -- Substitua '1' pelo id_cliente desejado

   -- Deleção de uma Venda e, se Necessário, Deleção do Cliente Associado
   DELETE FROM vendas
   WHERE id_venda = 1; -- Substitua '1' pelo id_venda desejado

   DELETE FROM clientes
   WHERE id_cliente = 1; -- Substitua '1' pelo id_cliente se desejar deletá-lo
