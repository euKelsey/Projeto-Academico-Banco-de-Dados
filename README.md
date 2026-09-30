# Projeto-Academico-Banco-de-Dados

Projeto acadêmico desenvolvido com foco em modelagem e implementação de um banco de dados relacional para um sistema de lava-rápido.

O objetivo deste repositório é aplicar, na prática, conceitos de:

- modelagem de banco de dados;
- entidades e relacionamentos;
- cardinalidades;
- chaves primárias e estrangeiras;
- integridade referencial;
- normalização;
- criação de tabelas;
- constraints;
- relacionamentos 1:N e N:N;
- scripts SQL;
- inserção de dados para testes.

> Projeto desenvolvido exclusivamente para fins acadêmicos e de estudo.

---

## Visão geral

O banco foi criado para representar as principais operações de um sistema de lava-rápido.

Entre os dados tratados pelo projeto estão:

- clientes;
- veículos;
- serviços;
- agendamentos;
- colaboradores;
- atendimentos;
- pagamentos.

O foco deste repositório é exclusivamente o banco de dados.

Uma versão dessa estrutura também é utilizada no projeto Full Stack, mas aqui o objetivo principal é estudar e documentar a modelagem, as regras e os scripts SQL.

---

## Tecnologias utilizadas

- MySQL
- MySQL Workbench
- SQL
- Git
- GitHub

---

## Banco de dados

O banco utilizado pelo projeto é:

```text
lava_rapido
```

Ele foi desenvolvido para representar um cenário acadêmico de gerenciamento de serviços de lava-rápido.

---

## Estrutura principal

Os arquivos mais importantes do projeto são:

```text
Projeto-Academico-Banco-de-Dados/
│
├── database/
│   ├── schema.sql
│   └── seed.sql
│
└── README.md
```

Caso a estrutura local possua arquivos adicionais de modelagem ou documentação, eles podem complementar os scripts SQL.

---

## Entidades principais

### Cliente

Representa os clientes cadastrados no sistema.

Principais informações:

- identificador do cliente;
- nome;
- CPF;
- telefone;
- e-mail;
- senha armazenada em formato de hash.

Regras importantes:

- CPF deve ser único;
- e-mail deve ser único.

### Veículo

Representa os veículos pertencentes aos clientes.

Principais informações:

- identificador do veículo;
- cliente proprietário;
- placa;
- marca;
- modelo;
- cor.

Regras importantes:

- cada veículo pertence a um cliente;
- a placa deve ser única.

Relacionamento principal:

```text
Cliente 1:N Veículo
```

Um cliente pode possuir vários veículos, mas cada veículo pertence a apenas um cliente.

### Serviço

Representa os serviços oferecidos pelo lava-rápido.

Principais informações:

- identificador do serviço;
- nome;
- descrição;
- preço.

O preço deve possuir valor válido e não negativo.

### Agendamento

Representa o agendamento de um serviço para determinado veículo.

Principais informações:

- identificador do agendamento;
- veículo;
- data;
- horário;
- status.

Status utilizados:

```text
AGENDADO
CANCELADO
CONCLUIDO
```

Relacionamento principal:

```text
Veículo 1:N Agendamento
```

Um veículo pode possuir vários agendamentos ao longo do tempo.

### Agendamento_Serviço

Tabela associativa responsável pelo relacionamento muitos-para-muitos entre agendamentos e serviços.

Relacionamento:

```text
Agendamento N:N Serviço
```

A tabela também armazena:

```text
valor_praticado
```

Esse campo representa o valor do serviço no momento em que o agendamento foi realizado.

Isso é importante porque o preço atual de um serviço pode mudar no futuro, mas o valor histórico do agendamento precisa permanecer preservado.

### Colaborador

Representa os colaboradores do sistema.

Principais informações:

- identificador;
- nome;
- cargo;
- nível de acesso;
- e-mail;
- senha armazenada como hash quando aplicável.

Níveis de acesso utilizados:

```text
ADMINISTRADOR
ATENDENTE
OPERACIONAL
```

### Atendimento

Representa a execução prática de um agendamento.

Principais informações:

- identificador do atendimento;
- agendamento relacionado;
- status;
- data e hora de início;
- data e hora de finalização.

Status utilizados:

```text
AGUARDANDO
EM_LAVAGEM
FINALIZADO
```

Relacionamento principal:

```text
Agendamento 1:1 Atendimento
```

Na estrutura atual, um agendamento possui no máximo um atendimento relacionado.

### Colaborador_Atendimento

Tabela utilizada para representar a associação entre colaboradores e atendimentos.

Ela permite relacionar colaboradores envolvidos na execução dos atendimentos.

### Pagamento

Representa as informações de pagamento de um agendamento.

Principais informações:

- identificador do pagamento;
- agendamento;
- data do pagamento;
- valor;
- forma de pagamento;
- status do pagamento.

Formas de pagamento utilizadas:

```text
DINHEIRO
PIX
CREDITO
DEBITO
```

Status utilizados:

```text
PENDENTE
PAGO
CANCELADO
```

Regras importantes:

- o valor não pode ser negativo;
- cada pagamento está relacionado a um agendamento;
- quando o pagamento está com status `PAGO`, os dados necessários de pagamento devem estar preenchidos.

---

## Relacionamentos principais

Uma visão simplificada dos principais relacionamentos:

```text
Cliente
   │
   └── 1:N ── Veículo
                  │
                  └── 1:N ── Agendamento
                                  │
                                  ├── N:N ── Serviço
                                  │
                                  ├── 1:1 ── Atendimento
                                  │
                                  └── 1:1 ── Pagamento
```

Também existe a associação entre:

```text
Colaborador ↔ Atendimento
```

por meio da tabela associativa `colaborador_atendimento`.

---

## Chaves primárias

As chaves primárias identificam de forma única cada registro.

Exemplos:

```text
id_cliente
id_veiculo
id_servico
id_agendamento
id_colaborador
id_atendimento
id_pagamento
```

---

## Chaves estrangeiras

As chaves estrangeiras garantem a ligação entre tabelas relacionadas.

Exemplos:

```text
veiculo.id_cliente
agendamento.id_veiculo
agendamento_servico.id_agendamento
agendamento_servico.id_servico
atendimento.id_agendamento
pagamento.id_agendamento
```

---

## Integridade referencial e constraints

O banco utiliza regras para impedir dados inconsistentes, como:

- `NOT NULL` para campos obrigatórios;
- `UNIQUE` para dados que não podem se repetir;
- `CHECK` para valores mínimos e regras específicas;
- `ENUM` para status e valores controlados;
- chaves estrangeiras para preservar relacionamentos válidos.

---

## Relacionamentos 1:N e N:N

Exemplo de 1:N:

```text
Cliente 1:N Veículo
```

Um cliente pode possuir vários veículos.

Outro exemplo:

```text
Veículo 1:N Agendamento
```

Um veículo pode possuir vários agendamentos.

Exemplo de N:N:

```text
Agendamento N:N Serviço
```

Esse relacionamento é resolvido pela tabela:

```text
agendamento_servico
```

---

## schema.sql

O arquivo `schema.sql` cria a estrutura do banco.

Ele é responsável por:

- criação do banco;
- criação das tabelas;
- chaves primárias;
- chaves estrangeiras;
- constraints;
- relacionamentos;
- tipos e regras estruturais.

---

## seed.sql

O arquivo `seed.sql` insere os dados iniciais utilizados para testes.

Os dados são fictícios e possuem finalidade exclusivamente acadêmica.

---

## Ordem de execução

Execute sempre nesta ordem:

```text
1. schema.sql
2. seed.sql
```

Primeiro é criada a estrutura.

Depois são inseridos os dados de teste.

---

## Como executar no MySQL Workbench

1. Abra o MySQL Workbench.
2. Conecte-se ao servidor MySQL.
3. Abra `schema.sql`.
4. Execute o script completo.
5. Abra `seed.sql`.
6. Execute o script completo.

Depois confira:

```sql
USE lava_rapido;
SHOW TABLES;
```

---

## Consultas básicas para teste

```sql
SELECT * FROM cliente;
SELECT * FROM veiculo;
SELECT * FROM servico;
SELECT * FROM agendamento;
SELECT * FROM agendamento_servico;
SELECT * FROM colaborador;
SELECT * FROM atendimento;
SELECT * FROM pagamento;
```

---

## Consulta com relacionamento

Exemplo para visualizar veículos e seus clientes:

```sql
SELECT
    c.nome AS cliente,
    v.placa,
    v.marca,
    v.modelo
FROM cliente c
INNER JOIN veiculo v
    ON v.id_cliente = c.id_cliente;
```

---

## Exemplo de consulta de agendamentos e serviços

```sql
SELECT
    a.id_agendamento,
    s.nome AS servico,
    ags.valor_praticado
FROM agendamento a
INNER JOIN agendamento_servico ags
    ON ags.id_agendamento = a.id_agendamento
INNER JOIN servico s
    ON s.id_servico = ags.id_servico;
```

---

## Regras de negócio representadas no banco

- um veículo pertence a um cliente;
- um cliente pode possuir vários veículos;
- um veículo pode possuir vários agendamentos;
- um agendamento pode conter vários serviços;
- um serviço pode estar presente em vários agendamentos;
- o valor praticado do serviço é preservado no momento do agendamento;
- pagamentos ficam associados aos agendamentos;
- valores financeiros não podem ser negativos;
- CPF e e-mail do cliente devem ser únicos;
- placa do veículo deve ser única;
- status são controlados por valores previamente definidos;
- relacionamentos são protegidos por chaves estrangeiras.

---

## Por que armazenar `valor_praticado`

O campo `valor_praticado` preserva o preço do serviço no momento do agendamento.

Exemplo:

```text
Preço no momento do agendamento: R$ 35,00
Preço atual do serviço:          R$ 45,00
```

Sem esse campo, um histórico antigo poderia passar a mostrar o preço atual em vez do preço realmente praticado na época.

---

## Segurança

Senhas não devem ser armazenadas em texto puro.

Na aplicação que utiliza este banco, as senhas são armazenadas em formato de hash.

O banco também utiliza restrições para reduzir a possibilidade de dados inválidos.

---

## Git e GitHub

Para verificar alterações:

```bash
git status
```

Para adicionar arquivos:

```bash
git add .
```

Para criar um commit:

```bash
git commit -m "Descricao da alteracao"
```

Para enviar alterações:

```bash
git push origin main
```

Para baixar alterações:

```bash
git pull origin main
```

---

## Relação com o projeto Full Stack

Uma versão desse banco também é utilizada no repositório:

```text
Projeto-Academico-FullStack
```

Nesse outro projeto, o banco é integrado a:

- backend Java;
- Servlets;
- JDBC;
- sistema Web;
- aplicativo Flutter.

Neste repositório, entretanto, o foco permanece sendo exclusivamente o estudo e desenvolvimento do banco de dados.

---

## Finalidade acadêmica

Este projeto foi desenvolvido com finalidade acadêmica, com foco no aprendizado prático de:

- Banco de Dados Relacional;
- SQL;
- modelagem conceitual;
- modelagem lógica;
- relacionamentos;
- cardinalidades;
- chaves primárias;
- chaves estrangeiras;
- constraints;
- integridade referencial;
- scripts de criação;
- scripts de dados;
- consultas SQL;
- MySQL Workbench;
- Git e GitHub.

---

## Observação final

Este banco não representa um sistema comercial real.

Clientes, veículos, serviços, colaboradores, pagamentos e demais dados utilizados foram criados exclusivamente para estudos e testes acadêmicos.
