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



## Parte C - Implementação


