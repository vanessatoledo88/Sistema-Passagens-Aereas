# Estudo Guiado SQL e CRUD
## Resolução dos Exercícios

**Disciplina:** Banco de Dados  
**Aluna:** Vanessa Prudêncio Toledo  
**Projeto:** Van Linhas Aéreas  

---

# 4. SELECT: escolhendo o que visualizar

## Exercício 4.1

### Liste `codigo_iata`, `nome`, `cidade` e `pais` de todos os aeroportos. Renomeie `codigo_iata` para `codigo`.

### Consulta

```sql
SELECT codigo_iata AS codigo,
       nome,
       cidade,
       pais
FROM aeroporto;
```

### Resultado

| codigo | nome | cidade | pais |
|---------|---------|---------|---------|
| UDI | Aeroporto Ten. Cel. Aviador César Bombonato | Uberlândia | Brasil |
| GRU | Aeroporto Internacional de Guarulhos | São Paulo | Brasil |
| BSB | Aeroporto Internacional de Brasília | Brasília | Brasil |
| REC | Aeroporto Internacional do Recife | Recife | Brasil |

### Análise

Foram retornados os quatro aeroportos cadastrados na base de dados da companhia.

---

## Exercício 4.2

### Liste `numero_voo`, `data_hora_partida`, `data_hora_chegada` e `preco_base` de todos os voos.

### Consulta

```sql
SELECT numero_voo,
       data_hora_partida,
       data_hora_chegada,
       preco_base
FROM voo;
```

### Resultado

| numero_voo | data_hora_partida | data_hora_chegada | preco_base |
|------------|------------|------------|------------|
| VAN101 | 2026-09-10 08:00:00 | 2026-09-10 09:20:00 | 350.00 |
| VAN102 | 2026-09-10 11:00:00 | 2026-09-10 14:00:00 | 450.00 |
| VAN201 | 2026-09-11 07:00:00 | 2026-09-11 08:00:00 | 280.00 |
| VAN202 | 2026-09-11 09:00:00 | 2026-09-11 11:30:00 | 390.00 |

### Análise

A consulta apresentou todos os voos cadastrados, incluindo horários e preços.

---

## Exercício 4.3

### Liste `codigo`, `modelo` e `capacidade` de todas as aeronaves. Use o alias `aeronave` para a coluna `codigo`.

### Consulta

```sql
SELECT codigo AS aeronave,
       modelo,
       capacidade
FROM aeronave;
```

### Resultado

| aeronave | modelo | capacidade |
|-----------|-----------|-----------|
| VLA001 | Airbus A666 | 150 |
| VLA002 | Boeing 777-888 | 180 |

### Análise

Foram encontradas duas aeronaves cadastradas na companhia aérea.

---

# 5. Filtros e operadores

## Exercício 5.1

### Liste os voos com `preco_base` menor que 700 e partida a partir de 2026-01-01.

### Consulta

```sql
SELECT numero_voo,
       data_hora_partida,
       preco_base
FROM voo
WHERE preco_base < 700
  AND data_hora_partida >= TIMESTAMP '2026-01-01 00:00:00';
```

### Resultado

| numero_voo | data_hora_partida | preco_base |
|------------|------------|------------|
| VAN101 | 2026-09-10 08:00:00 | 350.00 |
| VAN102 | 2026-09-10 11:00:00 | 450.00 |
| VAN201 | 2026-09-11 07:00:00 | 280.00 |
| VAN202 | 2026-09-11 09:00:00 | 390.00 |

### Análise

Todos os voos cadastrados satisfazem as condições do filtro.

---

## Exercício 5.2

### Liste os registros de `reserva_voo` cujo `preco_pago` esteja entre 350 e 850.

### Consulta

```sql
SELECT id_reserva,
       id_voo,
       assento,
       preco_pago
FROM reserva_voo
WHERE preco_pago BETWEEN 350 AND 850;
```

### Resultado

| id_reserva | id_voo | assento | preco_pago |
|------------|------------|------------|------------|
| 1 | 2 | 1A | 420.00 |
| 3 | 4 | 10B | 380.00 |

### Análise

Apenas duas reservas possuem valores dentro do intervalo solicitado.

---

## Exercício 5.3

### Liste nome e email dos passageiros cujo nome contenha a letra "a", sem diferenciar maiúsculas e minúsculas.

### Consulta

```sql
SELECT nome,
       email
FROM passageiro
WHERE nome ILIKE '%a%';
```

### Resultado

| nome | email |
|---------|---------|
| Vanessa Toledo | vanessat@vanlinhas.com |
| Daniela de Paula | danielap@vanlinhas.com |

### Análise

Foram localizados os passageiros cujos nomes contêm a letra "a".

---

## Exercício 5.4

### Liste reservas com status CONFIRMADA ou CANCELADA.

### Consulta

```sql
SELECT id_reserva,
       data_reserva,
       status,
       id_passageiro
FROM reserva
WHERE status IN ('CONFIRMADA', 'CANCELADA');
```

### Resultado

```text
Nenhum registro encontrado.
```

### Análise

Os dados cadastrados utilizam os valores "Confirmada", "Pendente" e "Alterada". Por isso não houve correspondência exata com os valores pesquisados em caixa alta.

---

# 6. Ordenação

## Exercício 6.1

### Liste `numero_voo`, `data_hora_partida` e `preco_base` do maior preço para o menor; em empate, partida mais antiga primeiro.

### Consulta

```sql
SELECT numero_voo,
       data_hora_partida,
       preco_base
FROM voo
ORDER BY preco_base DESC,
         data_hora_partida ASC;
```

### Resultado

| numero_voo | data_hora_partida | preco_base |
|------------|------------|------------|
| VAN102 | 2026-09-10 11:00:00 | 450.00 |
| VAN202 | 2026-09-11 09:00:00 | 390.00 |
| VAN101 | 2026-09-10 08:00:00 | 350.00 |
| VAN201 | 2026-09-11 07:00:00 | 280.00 |

### Análise

Os voos foram ordenados corretamente do maior para o menor preço.

---

## Exercício 6.2

### Liste código, modelo e capacidade das aeronaves, ordenando pela maior capacidade e depois pelo modelo em ordem alfabética.

### Consulta

```sql
SELECT codigo,
       modelo,
       capacidade
FROM aeronave
ORDER BY capacidade DESC,
         modelo ASC;
```

### Resultado

| codigo | modelo | capacidade |
|---------|---------|---------|
| VLA002 | Boeing 777-888 | 180 |
| VLA001 | Airbus A666 | 150 |

### Análise

As aeronaves foram classificadas pela capacidade máxima.

---

## Exercício 6.3

### Mostre somente os três registros de `reserva_voo` com maior `preco_pago`.

### Consulta

```sql
SELECT id_reserva,
       id_voo,
       assento,
       preco_pago
FROM reserva_voo
ORDER BY preco_pago DESC
LIMIT 3;
```

### Resultado

| id_reserva | id_voo | assento | preco_pago |
|------------|------------|------------|------------|
| 1 | 2 | 1A | 420.00 |
| 3 | 4 | 10B | 380.00 |
| 1 | 1 | 1A | 320.00 |

### Análise

Foram retornados os três maiores valores pagos nas reservas.

---

# 7. Funções

## Exercício 7.1

### Mostre quantidade, menor preço pago, maior preço pago e preço médio de `reserva_voo`.

### Consulta

```sql
SELECT COUNT(*) AS quantidade,
       MIN(preco_pago) AS menor_preco,
       MAX(preco_pago) AS maior_preco,
       ROUND(AVG(preco_pago), 2) AS preco_medio
FROM reserva_voo;
```

### Resultado

| quantidade | menor_preco | maior_preco | preco_medio |
|------------|------------|------------|------------|
| 5 | 250.00 | 420.00 | 326.00 |

### Análise

A tabela reserva_voo possui cinco registros e preço médio de 326,00.

---

## Exercício 7.2

### Mostre cada status de reserva e a quantidade de reservas nesse status.

### Consulta

```sql
SELECT status,
       COUNT(*) AS quantidade
FROM reserva
GROUP BY status
ORDER BY quantidade DESC;
```

### Resultado

| status | quantidade |
|---------|---------|
| Confirmada | 1 |
| Pendente | 1 |
| Alterada | 1 |

### Análise

Cada status possui uma ocorrência na base de dados.

---

## Exercício 7.3

### Para cada id_voo de reserva_voo, mostre a quantidade de reservas e o preço médio pago. Exiba apenas voos com pelo menos duas reservas.

### Consulta

```sql
SELECT id_voo,
       COUNT(*) AS qtd_reservas,
       ROUND(AVG(preco_pago),2) AS preco_medio
FROM reserva_voo
GROUP BY id_voo
HAVING COUNT(*) >= 2
ORDER BY qtd_reservas DESC;
```

### Resultado

| id_voo | qtd_reservas | preco_medio |
|---------|---------|---------|
| 3 | 2 | 255.00 |

### Análise

O voo de ID 3 aparece em duas reservas e possui preço médio de 255,00.

---

## Exercício 7.4

### Mostre a capacidade mínima, máxima e média das aeronaves.

### Consulta

```sql
SELECT MIN(capacidade) AS menor_capacidade,
       MAX(capacidade) AS maior_capacidade,
       ROUND(AVG(capacidade),2) AS capacidade_media
FROM aeronave;
```

### Resultado

| menor_capacidade | maior_capacidade | capacidade_media |
|---------|---------|---------|
| 150 | 180 | 165.00 |

### Análise

A capacidade média da frota cadastrada é de 165 passageiros.

---
