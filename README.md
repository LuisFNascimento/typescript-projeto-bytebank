# 📌 Projeto Conta Bancária (LocalStorage)

Este projeto implementa uma **simulação de conta bancária** utilizando
**TypeScript** e armazenamento no navegador via **LocalStorage**.

Ele permite **registrar transações** (depósitos, transferências,
pagamentos), atualizar o **saldo** e **agrupar as transações por
mês/ano**.

---

## 🚀 Funcionalidades

- **Saldo**:
  - Inicializado a partir do `localStorage` ou `0` se não existir.\
  - Persistido automaticamente após depósitos e débitos.
- **Transações (`Transacao`)**:
  - Armazenadas em `localStorage`.\
  - Recuperadas com conversão automática da propriedade `data` para
    `Date`.
- **Operações**:
  - `depositar(valor: number)` → Adiciona saldo (valor \> 0).\
  - `debitar(valor: number)` → Remove saldo (valor \> 0 e \<= saldo
    atual).\
  - `registrarTransacao(transacao: Transacao)` → Registra uma
    transação e atualiza o saldo conforme o tipo.
- **Agrupamento de transações (`GrupoTransacao`)**:
  - Retorna lista de transações agrupadas por **mês e ano**
    (`"agosto de 2025"`, por exemplo).

---

## 🛠️ Estrutura de Arquivos

    src/
     ├── Transacao.ts       # Classe/Interface que define uma transação
     ├── TipoTransacao.ts   # Enum com tipos de transações (DEPÓSITO, TRANSFERÊNCIA, PAGAMENTO_BOLETO)
     ├── GrupoTransacao.ts  # Interface que define grupo de transações (label + lista)
     ├── Conta.ts           # Implementação principal da Conta

---

## 📂 API da Conta

### 🔹 Métodos Públicos

---

Método Retorno Descrição

---

`getSaldo()` `number` Retorna o saldo atual.

`getDataAcesso()` `Date` Retorna a data/hora atual.

`getGruposTransacoes()` `GrupoTransacao[]` Retorna lista de transações
agrupadas por mês/ano.

`registrarTransacao(transacao: Transacao)` `void` Registra uma nova transação e
atualiza saldo/localStorage.

---

---

## 🏦 Tipos de Transação (`TipoTransacao`)

- `DEPÓSITO` → Adiciona saldo.\
- `TRANSFERENCIA` → Remove saldo (valor invertido para registrar
  negativo).\
- `PAGAMENTO_BOLETO` → Remove saldo (valor invertido para registrar
  negativo).

---

## 💾 Persistência

- `localStorage["saldo"]` → Guarda o saldo atual.\
- `localStorage["transacoes"]` → Lista de transações em JSON.

---

## 📌 Exemplo de Uso

```ts
import Conta from "./Conta.js";
import { Transacao } from "./Transacao.js";
import { TipoTransacao } from "./TipoTransacao.js";

const deposito: Transacao = {
  valor: 1000,
  tipoTransacao: TipoTransacao.DEPOSITO,
  data: new Date(),
};

Conta.registrarTransacao(deposito);

console.log("Saldo atual:", Conta.getSaldo());
console.log("Transações agrupadas:", Conta.getGruposTransacoes());
```

---

## ✅ Regras de Negócio

- Não é possível **depositar ou debitar valores ≤ 0**.\
- Não é possível **debitar valores maiores que o saldo atual**.\
- Tipos de transação inválidos geram **erro**.
