# 🍔 DeliveryCore API — Backend

API backend em **Java 21 + Spring Boot 3.3.6** que simula uma plataforma de delivery no
estilo marketplace (iFood, Uber Eats).

Projeto de **estudo de engenharia backend**, focado nos problemas que aparecem em sistemas
transacionais com dinheiro envolvido: consistência de estados, concorrência e integração
assíncrona de pagamento.

> **Status:** estudo concluído, não mantido ativamente. Serviu de base para um marketplace
> mais completo que desenvolvo hoje (Spring Boot 4, PostgreSQL, Flyway, Testcontainers).
> **Este repositório não tem testes automatizados** — foi aqui que exercitei modelagem de
> domínio, não disciplina de teste. Se você veio avaliar meu trabalho, a seção abaixo diz
> exatamente o que esperar encontrar.

---

## 🧠 O que este projeto demonstra

Implementado e funcionando no código:

* **Controle de concorrência com optimistic locking** — `@Version` em `Order` e `Payment`,
  as duas entidades onde escrita concorrente corromperia estado.
* **Idempotência de webhook de pagamento** — o mesmo evento chegando duas vezes não credita
  duas vezes (`PaymentController`, `PaymentRepository`).
* **Máquinas de estado** de pedido e pagamento, com transições validadas no domínio.
* **Autenticação e autorização** com Spring Security + JWT.
* **Tracking de entrega em tempo real** via WebSocket.
* **Documentação de API** gerada com springdoc-openapi (Swagger UI).

Não implementado: testes automatizados, catálogo de restaurantes, itens de pedido, cálculo
de taxa de entrega, busca por localização, notificações e observabilidade. O projeto parou
antes disso — a evolução foi para o marketplace novo.

---

## 🚚 Modelo de negócio

Marketplace de delivery. Fluxo principal:

```
Cliente cria pedido
   ↓
Restaurante recebe e aceita
   ↓
Restaurante prepara
   ↓
Entregador aceita a entrega
   ↓
Pedido é entregue
```

---

## 🏗 Domínio

Entidades principais: `User`, `Customer`, `Deliveryman`, `Restaurant`, `Order`, `Payment`,
`Review`.

Pedido e pagamento são governados por máquinas de estado — transição inválida é rejeitada
no domínio, não no controller.

---

## 💳 Segurança financeira

O núcleo financeiro foi o ponto do estudo. Mecanismos **implementados**:

* idempotência de webhook (evento duplicado não gera crédito duplicado)
* optimistic locking em `Order` e `Payment`
* validação de transição de estado de pagamento
* sincronização entre os estados de `Order` e `Payment`

---

## 🧰 Stack

| Camada | Tecnologia |
|---|---|
| Linguagem | Java 21 |
| Framework | Spring Boot 3.3.6 |
| Segurança | Spring Security, JWT (jjwt 0.11.5) |
| Persistência | Spring Data JPA / Hibernate |
| Banco | MySQL |
| Tempo real | Spring WebSocket |
| Documentação | springdoc-openapi 2.6.0 (Swagger UI) |
| Build | Maven |
| Utilitários | Lombok |

Conceitos aplicados: REST, Webhooks, Domain Modeling, State Machines, Transaction
Management, Optimistic Locking, Idempotência.

---

## ▶️ Como rodar

Requisitos: JDK 21, Maven e uma instância MySQL.

```bash
# configure a conexão em src/main/resources/application.yaml
./mvnw spring-boot:run
```

Swagger UI: `http://localhost:8080/swagger-ui.html`

---

## 👨‍💻 Autor

**Kauan Motta** — Desenvolvedor Backend Java
[LinkedIn](https://www.linkedin.com/in/kauanmotta-dev) · [GitHub](https://github.com/kauanmotta-dev)
