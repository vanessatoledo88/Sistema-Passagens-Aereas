# Sistema de Passagens Aéreas - Van Linhas Aéreas

Disciplina: Banco de Dados

Aluna: Vanessa Toledo

## Parte A - Requisitos

### Entidades

| Entidade | Descrição |
|-----------|-----------|
| Passageiro | Pessoa que realiza reservas de passagens. |
| Aeroporto | Local de origem ou destino dos voos. |
| Aeronave | Veículo responsável pela realização dos voos. |
| Voo | Trajeto realizado pela companhia aérea. |
| Reserva | Registro da compra realizada pelo passageiro. |
| Reserva_Voo | Entidade associativa responsável por relacionar reservas e voos. |

---

### Atributos

| Entidade | Atributos |
|-----------|-----------|
| Passageiro | id_passageiro, cpf, nome, data_nascimento, email |
| Aeroporto | id_aeroporto, codigo_iata, nome, cidade, pais |
| Aeronave | id_aeronave, codigo, modelo, capacidade |
| Voo | id_voo, numero_voo, data_hora_partida, data_hora_chegada, preco_base, id_aeroporto_origem, id_aeroporto_destino, id_aeronave |
| Reserva | id_reserva, data_reserva, status, id_passageiro |
| Reserva_Voo | id_reserva, id_voo, assento, preco_pago |

---

### Relacionamentos

| Entidade A | Relacionamento | Entidade B |
|------------|----------------|------------|
| Passageiro | Realiza | Reserva |
| Reserva | Contém | Reserva_Voo |
| Reserva_Voo | Refere-se a | Voo |
| Voo | Origina-se em | Aeroporto |
| Voo | Tem destino em | Aeroporto |
| Voo | É realizado por | Aeronave |

#### Observação

O relacionamento entre **Reserva** e **Voo** é do tipo **N:N (muitos para muitos)**. Para resolver esse relacionamento foi criada a entidade associativa **Reserva_Voo**, responsável por armazenar informações específicas do trecho da viagem, como assento e preço pago.

---

### Regras de Negócio

**RN01** – Todo passageiro deve possuir CPF, nome, data de nascimento e e-mail cadastrados.

**RN02** – Todo aeroporto deve possuir um código IATA único.

**RN03** – Todo voo deve possuir um número identificador único.

**RN04** – Todo voo deve possuir exatamente um aeroporto de origem e um aeroporto de destino.

**RN05** – Todo voo deve ser realizado por uma única aeronave.

**RN06** – Um passageiro pode realizar nenhuma, uma ou várias reservas, mas cada reserva pertence a apenas um passageiro.

**RN07** – Uma reserva pode conter um ou mais voos, e um mesmo voo pode estar presente em várias reservas.

**RN08** – Para cada voo incluído em uma reserva devem ser registrados obrigatoriamente o assento e o preço efetivamente pago pelo trecho.

---

## Parte B - Modelagem

### Cardinalidades

| Relacionamento | Cardinalidade |
|---------------|---------------|
| Passageiro → Reserva | 1:N |
| Reserva → Voo | N:N |
| Aeronave → Voo | 1:N |
| Aeroporto → Voo (Origem) | 1:N |
| Aeroporto → Voo (Destino) | 1:N |

---

### Diagrama (DrawDB)

O diagrama abaixo representa o modelo lógico desenvolvido para o sistema de passagens aéreas da Van Linhas Aéreas.

diagrama.jpeg

#### Visualizar em tamanho completo

[Clique aquipeg

---

### Chaves Primárias (PK)

| Entidade | Chave Primária |
|-----------|-----------|
| Passageiro | id_passageiro |
| Aeroporto | id_aeroporto |
| Aeronave | id_aeronave |
| Voo | id_voo |
| Reserva | id_reserva |
| Reserva_Voo | (id_reserva, id_voo) |

#### Observação

Na entidade associativa **Reserva_Voo**, a chave primária é composta pelos atributos **id_reserva** e **id_voo**. É necessário utilizar a combinação desses dois atributos para que cada associação entre uma reserva e um voo seja identificada de forma única.

---

### Chaves Estrangeiras (FK)

| Tabela | Chave Estrangeira | Referência |
|----------|----------|----------|
| Reserva | id_passageiro | Passageiro |
| Voo | id_aeroporto_origem | Aeroporto |
| Voo | id_aeroporto_destino | Aeroporto |
| Voo | id_aeronave | Aeronave |
| Reserva_Voo | id_reserva | Reserva |
| Reserva_Voo | id_voo | Voo |

#### Observação

Os atributos **id_reserva** e **id_voo** da entidade associativa **Reserva_Voo** também atuam como chaves estrangeiras. O atributo **id_reserva** referencia a entidade Reserva, enquanto **id_voo** referencia a entidade Voo.

---

### Tabela Associativa

A tabela **Reserva_Voo** foi criada para resolver o relacionamento **N:N (muitos para muitos)** entre as entidades **Reserva** e **Voo**.

#### Estrutura

```text
Reserva_Voo
|
├── id_reserva (PK/FK)
├── id_voo (PK/FK)
├── assento
└── preco_pago
```

Essa tabela permite que uma mesma reserva possua vários voos e que um voo possa participar de várias reservas, além de armazenar informações específicas de cada trecho, como assento e preço pago.

## Parte C - Implementação

Nesta etapa foi realizada a implementação física do banco de dados da **Van Linhas Aéreas** utilizando PostgreSQL no Supabase.

---

### SQL de Criação

O script completo de criação do banco encontra-se no arquivo:

📄 **modelo.sql**

---

### Restrições Implementadas

As restrições foram utilizadas para garantir a integridade dos dados e impedir o cadastro de informações inválidas.

| Restrição | Regra Implementada | Explicação |
|------------|------------|------------|
| `UNIQUE (cpf)` | Não permite CPF duplicado. | Cada passageiro deve possuir um CPF único. |
| `UNIQUE (email)` | Não permite e-mails duplicados. | Um mesmo e-mail não pode ser associado a mais de um passageiro. |
| `UNIQUE (codigo_iata)` | Não permite códigos IATA repetidos. | Cada aeroporto deve possuir um identificador único. |
| `UNIQUE (codigo)` | Não permite códigos de aeronave repetidos. | Cada aeronave deve possuir um código exclusivo. |
| `NOT NULL` | Campos obrigatórios. | Impede que informações essenciais sejam registradas em branco. |
| `CHECK (capacidade > 0)` | Capacidade da aeronave deve ser maior que zero. | Evita o cadastro de aeronaves com capacidade inválida. |
| `CHECK (preco_base >= 0)` | Preço base não pode ser negativo. | Garante que os valores dos voos sejam válidos. |
| `CHECK (id_aeroporto_origem <> id_aeroporto_destino)` | Origem e destino devem ser diferentes. | Um voo não pode sair e chegar no mesmo aeroporto. |
| `CHECK (data_hora_chegada > data_hora_partida)` | Chegada após a partida. | Garante coerência entre horários do voo. |
| `CHECK (status IN (...))` | Status permitido. | Aceita apenas os status: Pendente, Confirmada, Cancelada e Alterada. |
| `CHECK (preco_pago >= 0)` | Preço pago não pode ser negativo. | Impede valores inválidos nas reservas. |

---

### Registros Inseridos

Foram cadastrados dados fictícios para validar o funcionamento do banco de dados.

#### Passageiro

| id_passageiro | CPF | Nome | Data de Nascimento | E-mail |
|---------------|------|------|------|------|
| 1 | 06778016612 | Vanessa Toledo | 30/08/1988 | vanessat@vanlinhas.com |
| 2 | 31663058822 | Daniela de Paula | 12/05/1983 | danielap@vanlinhas.com |
| 3 | 47766412837 | Miguel de Paula | 25/04/2011 | miguelp@vanlinhas.com |

---

#### Aeroporto

| id_aeroporto | Código IATA | Nome | Cidade | País |
|--------------|-------------|-------------|-------------|-------------|
| 1 | UDI | Aeroporto Ten. Cel. Aviador César Bombonato | Uberlândia | Brasil |
| 2 | GRU | Aeroporto Internacional de Guarulhos | São Paulo | Brasil |
| 3 | BSB | Aeroporto Internacional de Brasília | Brasília | Brasil |
| 4 | REC | Aeroporto Internacional do Recife | Recife | Brasil |

---

#### Aeronave

| id_aeronave | Código | Modelo | Capacidade |
|-------------|---------|---------|---------|
| 1 | VLA001 | Airbus A666 | 150 |
| 2 | VLA002 | Boeing 777-888 | 180 |

---

#### Voo

| id_voo | Número do Voo | Origem | Destino | Partida | Chegada | Preço Base |
|---------|---------|---------|---------|---------|---------|---------|
| 1 | VAN101 | UDI | GRU | 10/09/2026 08:00 | 10/09/2026 09:20 | R$ 350,00 |
| 2 | VAN102 | GRU | REC | 10/09/2026 11:00 | 10/09/2026 14:00 | R$ 450,00 |
| 3 | VAN201 | UDI | BSB | 11/09/2026 07:00 | 11/09/2026 08:00 | R$ 280,00 |
| 4 | VAN202 | BSB | REC | 11/09/2026 09:00 | 11/09/2026 11:30 | R$ 390,00 |

---

#### Reserva

| id_reserva | Data da Reserva | Status | Passageiro |
|------------|------------|------------|------------|
| 1 | 01/09/2026 | Confirmada | Vanessa Toledo |
| 2 | 02/09/2026 | Pendente | Daniela de Paula |
| 3 | 03/09/2026 | Alterada | Miguel de Paula |

---

#### Reserva_Voo

| Reserva | Voo | Assento | Preço Pago |
|----------|----------|----------|----------|
| 1 | VAN101 | 1A | R$ 320,00 |
| 1 | VAN102 | 1A | R$ 420,00 |
| 2 | VAN201 | 8C | R$ 260,00 |
| 3 | VAN201 | 10B | R$ 250,00 |
| 3 | VAN202 | 10B | R$ 380,00 |

---

### Validação de Viagens com Conexão

O modelo permite representar viagens com conexão através da entidade associativa **Reserva_Voo**.

#### Reserva 1

```text
Uberlândia (UDI)
        ↓
São Paulo (GRU)
        ↓
Recife (REC)
```

Voos:

- VAN101
- VAN102

---

#### Reserva 3

```text
Uberlândia (UDI)
        ↓
Brasília (BSB)
        ↓
Recife (REC)
```

Voos:

- VAN201
- VAN202

Esses exemplos demonstram que uma mesma reserva pode conter vários voos, validando corretamente o relacionamento N:N entre as entidades Reserva e Voo.

## Parte D - Testes

Para validar as regras de integridade implementadas no banco de dados, foram realizadas tentativas de inserção de dados inválidos. Em todos os casos, o banco rejeitou a operação, garantindo a consistência das informações.

---

### Testes de Violação de Regras

| Teste | Resultado Esperado | Resultado Obtido | Regra Protegida |
|---------|---------|---------|---------|
| CPF duplicado | Inserção rejeitada | Operação rejeitada pela restrição UNIQUE | Um passageiro não pode possuir o mesmo CPF de outro passageiro |
| FK inválida | Inserção rejeitada | Operação rejeitada pela FOREIGN KEY | Toda reserva deve estar associada a um passageiro existente |
| Preço negativo | Inserção rejeitada | Operação rejeitada pela CHECK `ck_voo_preco_base` | O preço base do voo não pode ser negativo |
| Origem = Destino | Inserção rejeitada | Operação rejeitada pela CHECK `ck_voo_origem_destino` | O aeroporto de origem deve ser diferente do aeroporto de destino |

---

### Teste 1 — CPF Duplicado

#### Comando executado

```sql
INSERT INTO passageiro
(cpf, nome, data_nascimento, email)
VALUES
('06778016612', 'Teste', '2000-01-01', 'teste@email.com');
```

#### Resultado esperado

O banco deve rejeitar a inserção.

#### Resultado obtido

```text
ERROR:  23505: duplicate key value violates unique constraint "passageiro_cpf_key"
```

#### Regra protegida

O CPF deve ser único para cada passageiro.

---

### Teste 2 — Chave Estrangeira Inválida

#### Comando executado

```sql
INSERT INTO reserva
(data_reserva, status, id_passageiro)
VALUES
(CURRENT_DATE, 'Pendente', 99999);
```

#### Resultado esperado

O banco deve rejeitar a inserção.

#### Resultado obtido

```text
ERROR:  23503: insert or update on table "reserva" violates foreign key constraint "reserva_id_passageiro_fkey"
```

#### Regra protegida

Toda reserva deve estar vinculada a um passageiro existente.

---

### Teste 3 — Preço Negativo

#### Comando executado

```sql
INSERT INTO voo
(
numero_voo,
data_hora_partida,
data_hora_chegada,
preco_base,
id_aeroporto_origem,
id_aeroporto_destino,
id_aeronave
)
VALUES
(
'VAN998',
'2026-09-02 14:45:06',
'2026-09-02 16:45:18',
-200.00,
1,
2,
1
);
```

#### Resultado esperado

O banco deve rejeitar a inserção.

#### Resultado obtido

```text
23514: new row for relation "voo" violates check constraint "ck_voo_preco_base"
```

#### Regra protegida

O preço base do voo não pode ser negativo.

---

### Teste 4 — Origem Igual ao Destino

#### Comando executado

```sql
INSERT INTO voo
(
numero_voo,
data_hora_partida,
data_hora_chegada,
preco_base,
id_aeroporto_origem,
id_aeroporto_destino,
id_aeronave
)
VALUES
(
'VAN999',
'2026-09-02 14:45:06',
'2026-09-02 16:45:18',
200.00,
1,
1,
1
);
```

#### Resultado esperado

O banco deve rejeitar a inserção.

#### Resultado obtido

```text
ERROR:  23514: new row for relation "voo" violates check constraint "ck_voo_origem_destino"
```

#### Regra protegida

O aeroporto de origem deve ser diferente do aeroporto de destino.

---

## Parte E - Consultas

Foram desenvolvidas consultas SQL utilizando relacionamentos entre tabelas, funções de agregação e operações de junção (JOIN).

---

### Consulta 1 — Reservas dos Passageiros

#### SQL

```sql
SELECT
    passageiro.nome AS passageiro, /*AS nomeia a coluna que está sendo selecionada*/
    reserva.id_reserva AS reserva,
    reserva.status
FROM passageiro /*JOIN une os dados das tabelas passageiro e reserva*/
JOIN reserva
    ON passageiro.id_passageiro = reserva.id_passageiro; /*ON define qual campo será utilizado para relacionar as tabelas*/
```

#### Resultado Obtido

| Passageiro | Reserva | Status |
|------------|----------|----------|
| Vanessa Toledo | 1 | Confirmada |
| Daniela de Paula | 2 | Pendente |
| Miguel de Paula | 3 | Alterada |

---

### Consulta 2 — Voos de uma Reserva

#### SQL

```sql
SELECT
    reserva.id_reserva AS reserva,
    voo.numero_voo,
    aeroporto_origem.cidade AS origem,
    aeroporto_destino.cidade AS destino,
    reserva_voo.assento,
    reserva_voo.preco_pago

FROM reserva

/* Relaciona reserva com os voos incluídos na reserva */
JOIN reserva_voo
    ON reserva.id_reserva = reserva_voo.id_reserva

/* Relaciona os registros da reserva com a tabela voo */
JOIN voo
    ON reserva_voo.id_voo = voo.id_voo

/* Obtém o aeroporto de origem do voo */
JOIN aeroporto AS aeroporto_origem
    ON voo.id_aeroporto_origem = aeroporto_origem.id_aeroporto

/* Obtém o aeroporto de destino do voo */
JOIN aeroporto AS aeroporto_destino
    ON voo.id_aeroporto_destino = aeroporto_destino.id_aeroporto;
```

#### Resultado Obtido

| Reserva | Número do Voo | Origem | Destino | Assento | Preço Pago |
|----------|----------|----------|----------|----------|----------|
| 1 | VAN101 | Uberlândia | São Paulo | 1A | 320,00 |
| 1 | VAN102 | São Paulo | Recife | 1A | 420,00 |
| 2 | VAN201 | Uberlândia | Brasília | 8C | 260,00 |
| 3 | VAN201 | Uberlândia | Brasília | 10B | 250,00 |
| 3 | VAN202 | Brasília | Recife | 10* | 380,00 |

---

### Consulta 3 — Valor Total da Reserva

#### SQL
```sql
SELECT
id_reserva AS reserva,

/* Soma todos os valores pagos pelos voos da reserva */
SUM(preco_pago) AS total

FROM reserva_voo

/* Agrupa os registros por reserva */
GROUP BY*id_reserva;
```

#### Resultado Obtido

| Reserva | To*al Pago (R$) |
|----------|----------|
| 1 | 740,00 |
| 2 | 260,00 |
| 3 | 630,00 |
