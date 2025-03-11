# Diagrama Entidade-Relacionamento (DER) - E-commerce

Este modelo foi criado para representar a estrutura de um sistema de e-commerce e pode ser expandido conforme necessário.

### **Entidades e Atributos**

1. **Cliente**
   - idCliente (INT, PK)
   - nome (VARCHAR)
   - email (VARCHAR)
   - cpf (VARCHAR, UNIQUE, opcional)
   - cnpj (VARCHAR, UNIQUE, opcional)
   - telefone (VARCHAR)
   - endereco (VARCHAR)

2. **Pedido**
   - idPedido (INT, PK)
   - idCliente (INT, FK)
   - data_pedido (DATE)
   - quantidade_pedido (INT)
   - status_do_pedido (VARCHAR)
   - endereco (VARCHAR)
   - frete (FLOAT)
   - idDevolucao (INT, FK, opcional)
   - idFrete (INT, FK)

3. **Frete**
   - idFrete (INT, PK)
   - idPedido (INT, FK)
   - valor (FLOAT)
   - prazo_entrega (VARCHAR)

4. **Devolucao**
   - idDevolucao (INT, PK)
   - idPedido (INT, FK)
   - data_solicitacao (DATE)
   - status_da_solicitacao (VARCHAR)

5. **Produto**
   - idProduto (INT, PK)
   - nome (VARCHAR)
   - descricao (VARCHAR)
   - valor (FLOAT)
   - categoria (VARCHAR)
   - idFornecedor (INT, FK)

6. **Terceiro - Vendedor**
   - idTerceiro_Vendedor (INT, PK)
   - razao_social (VARCHAR)
   - cnpj (VARCHAR, UNIQUE)
   - contato (VARCHAR)
   - endereco (VARCHAR)

7. **Fornecedor**
   - idFornecedor (INT, PK)
   - razao_social (VARCHAR)
   - cnpj (VARCHAR, UNIQUE)
   - contato (VARCHAR)
   - endereco (VARCHAR)

8. **Estoque**
   - idEstoque (INT, PK)
   - local (VARCHAR)
   - idProduto (INT, FK)
   - disponivel (INT)


- Um **Cliente** pode fazer vários **Pedidos** (1:N).
- Um **Pedido** pode conter vários **Produtos** (N:M), utilizando a tabela intermediária **Produto_Pedido**.
- Um **Produto** pode ser vendido por vários **Vendedores** (N:M), utilizando a relação **Produto_Vendedor**.
- Um **Produto** é fornecido por apenas um **Fornecedor** (N:1).
- O **Estoque** gerencia a disponibilidade de **Produtos** (1:N).
- Cada **Pedido** tem um **Frete** associado (1:1).
- Um **Pedido** pode ter uma **Devolução** (1:0..1).

1. **CPF e CNPJ no Cliente**
   - Apenas um dos dois pode ser preenchido, garantindo que um cliente seja pessoa física ou jurídica, mas nunca ambos.
   
```sql
ALTER TABLE Cliente
ADD CONSTRAINT chk_cpf_cnpj
CHECK (
    (cpf IS NOT NULL AND cnpj IS NULL)
    OR (cpf IS NULL AND cnpj IS NOT NULL)
);
```

2. **Status do Pedido**: pode ser "Pendente", "Enviado", "Entregue", "Cancelado".
3. **Carência para Devolução**: Devoluções são permitidas dentro de um período de carência definido.
4. **Frete**: O valor do frete pode ser calculado com base na distância ou no peso do pedido.
5. **Produtos e Vendedores**: Um mesmo produto pode ser vendido por vários vendedores diferentes.
