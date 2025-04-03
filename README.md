# Projeto PixDama (Next.js + Express)

Este projeto consiste em um frontend desenvolvido com **Next.js** e um backend utilizando **Express.js**. Abaixo está a explicação da estrutura de diretórios de cada parte do projeto.

---

## 📂 Backend (PixDama-Backend)

O backend foi desenvolvido utilizando **Express.js** e **TypeScript**.

```
PIXDAMA-BACKEND/
│── node_modules/        # Dependências do projeto
│── src/                 # Código-fonte principal
│   ├── config/          # Configurações gerais do backend
│   ├── connectionPay/   # Gerenciamento de pagamentos
│   ├── controllers/     # Lógica dos endpoints
│   ├── middleware/      # Middlewares para tratamento de requisições
│   ├── routes/          # Definição das rotas do backend
│   ├── supabase/        # Integração com Supabase
│   ├── utils/           # Funções utilitárias
│   ├── validate/        # Validação de dados
│   ├── index.ts         # Arquivo principal do servidor
│── .env                 # Variáveis de ambiente (não versionado)
│── .env.example         # Exemplo de variáveis de ambiente
│── .gitignore           # Arquivos ignorados pelo Git
│── auxiliar.ts          # Código auxiliar
│── package.json         # Dependências do projeto
│── package-lock.json    # Controle de versões das dependências
│── tsconfig.json        # Configuração do TypeScript
```

### 🔹 Descrição das pastas principais

- **config/** → Contém configurações do projeto, como banco de dados e autenticação.
- **connectionPay/** → Gerencia integrações de pagamento (ex: Stripe, Pix, etc.).
- **controllers/** → Contém as funções que processam as requisições recebidas.
- **middleware/** → Middlewares para autenticação e validações antes de processar as requisições.
- **routes/** → Define as rotas da API e conecta aos controllers.
- **supabase/** → Módulos específicos para integração com Supabase.
- **utils/** → Funções auxiliares e utilitárias utilizadas no backend.
- **validate/** → Definições e regras para validação de dados recebidos.
- **index.ts** → Arquivo principal que inicia o servidor Express.

---

## 📂 Frontend (PixDama-Front)

O frontend foi desenvolvido utilizando **Next.js** com a nova estrutura de **App Router**.

```
PIXDAMA-FRONT/
│── .next/              # Build gerado pelo Next.js
│── app/                # Diretório principal das páginas e APIs
│   ├── actions/        # Funções assíncronas que interagem com a API
│   ├── alterar-senha/  # Página de alteração de senha
│   ├── api/            # Endpoints de API internos do Next.js
│   ├── components/     # Componentes reutilizáveis da aplicação
│   ├── context/        # Gerenciamento de estado global
│   ├── game/           # Página do jogo de damas
│   ├── painel-adm/     # Painel administrativo
│   ├── painel-usuario/ # Painel do usuário
│── services/           # Serviços para comunicação com o backend
│── termos/             # Página de termos e condições
│── styles/             # Estilos globais
│── globals.css         # Estilos CSS globais
│── layout.js           # Layout padrão da aplicação
│── page.js             # Página principal
│── lib/                # Configuração de bibliotecas externas
│── node_modules/       # Dependências do frontend
│── public/             # Arquivos estáticos como imagens e ícones
│── utils/              # Funções utilitárias
│── .env                # Variáveis de ambiente (não versionado)
│── .env.example        # Exemplo de variáveis de ambiente
│── .gitignore          # Arquivos ignorados pelo Git
│── jsconfig.json       # Configuração do JavaScript/TypeScript
│── next-env.d.ts       # Tipagem do Next.js
│── next.config.mjs     # Configuração do Next.js
│── package.json        # Dependências do projeto
│── package-lock.json   # Controle de versões das dependências
│── postcss.config.mjs  # Configuração do PostCSS
```

### 🔹 Descrição das pastas principais

- **app/** → Nova estrutura do Next.js contendo páginas, APIs e lógica de frontend.
- **actions/** → Funções assíncronas que interagem com o backend.
- **api/** → Endpoints internos do Next.js para processamento de dados.
- **components/** → Componentes reutilizáveis como botões, cards e inputs.
- **context/** → Context API para gerenciamento de estados globais.
- **game/** → Implementação do jogo de damas.
- **painel-adm/** → Interface para administração do sistema.
- **painel-usuario/** → Interface para usuários do sistema.
- **services/** → Camada de serviços para comunicação com a API do backend.
- **utils/** → Funções auxiliares para facilitar operações no frontend.

---

# Supabase Database Structure

Estrutura das tabelas no banco de dados do Supabase e suas respectivas funções.

## Tabelas

### 1. `users`
Responsável pelos dados dos usuários cadastrados no sistema.
#### Campos comuns:
- `id` (UUID, Primary Key) - Identificador único do usuário.
- `name` (TEXT) - Nome do usuário.
- `email` (TEXT, Unique) - Endereço de e-mail.
- `password_hash` (TEXT) - Hash da senha do usuário.
- `cpf` (TIMESTAMP) - CPF do user.
- `profile` (TIMESTAMP) - Nivel de autoridade.
- `birthdate` (TIMESTAMP) - Data de nascimento
- `total_wins` (TIMESTAMP) - Ganhos seguidos.

### 2. `wallet`
Gerencia as carteiras dos usuários.
#### Campos comuns:
- `id` (UUID, Primary Key) - Identificador único da carteira.
- `user_id` (UUID, Foreign Key) - Referência ao usuário dono da carteira.
- `balance` (DECIMAL) - Saldo disponível na carteira.

### 3. `cash_in`
Registra solicitações de QR Code Pix para depósitos.
#### Campos comuns:
- `id` (UUID, Primary Key) - Identificador único da transação.
- `user_id` (UUID, Foreign Key) - Referência ao usuário que solicitou o Pix.
- `amount` (DECIMAL) - Valor solicitado.
- `status` (TEXT) - Status da solicitação (pendente, concluída, cancelada).
- `created_at` (TIMESTAMP) - Data da solicitação.
- `external_id` (VARCHAR) - Chave de busca gateway e server

### 4. `history_transactions`
Histórico de depósitos e saques dos usuários.
#### Campos comuns:
- `id` (UUID, Primary Key) - Identificador único da transação.
- `user_id` (UUID, Foreign Key) - Referência ao usuário.
- `type` (TEXT) - Tipo da transação (depósito, saque).
- `value` (DECIMAL) - Valor da transação.
- `balance` (TEXT) - Saldo depois da transação.
- `date` (TIMESTAMP) - Data da transação.

### 5. `matches`
Armazena informações das partidas realizadas.
#### Campos comuns:
- `id` (UUID, Primary Key) - Identificador único da partida.
- `winner` (UUID, Foreign Key) - Referência ao jogador ganha.
- `loser` (UUID, Foreign Key) - Referência ao jogador que perde.
- `winner_id` (UUID, Foreign Key) - Referência ao jogador vencedor.
- `bet_amount` (DECIMAL) - Valor apostado na partida.
- `date` (TIMESTAMP) - Data da partida.

## Considerações finais
Tentei deixar o código o mais limpo possível. Os ajustes que fiz foram para garantir a funcionalidade. No frontend, ainda há resquícios do Supabase deixados pelo desenvolvedor anterior, que precisam ser limpos. 



