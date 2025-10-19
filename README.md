# 🌐 Projeto Fullstack

Este é um projeto **Fullstack** desenvolvido com **NestJS** no backend e **React** no frontend, utilizando **TypeScript**, **Tailwind CSS**, **shadcn/ui** e **React Hooks**.

O projeto segue a **Feature-Based Architecture**, organizado por módulos/funções, garantindo **escalabilidade**, **manutenção facilitada** e **separação clara de responsabilidades**.

## ⚡ Funcionalidades Principais

- **Autenticação e autorização** (JWT, roles)  
- **Gestão de usuários**  
- **Gestão de pedidos**  
- **Pagamentos**  
- **Notificações**  
- **Integração com APIs internas e externas**  
- **Suporte a testes unitários, de integração e E2E**  

# 🏗 Arquitetura do Projeto Backend (NestJS)

```
src/
├─ modules/
│   ├─ auth/
│   │   ├─ controllers/
│   │   │   └─ auth.controller.ts
│   │   ├─ services/
│   │   │   └─ auth.service.ts
│   │   ├─ strategies/
│   │   │   ├─ jwt.strategy.ts
│   │   │   └─ local.strategy.ts
│   │   ├─ guards/
│   │   │   └─ roles.guard.ts
│   │   ├─ dtos/
│   │   │   └─ login.dto.ts
│   │   └─ auth.module.ts
│   │
│   ├─ users/
│   │   ├─ controllers/
│   │   ├─ services/
│   │   ├─ repositories/
│   │   ├─ dtos/
│   │   ├─ entities/
│   │   ├─ events/
│   │   └─ users.module.ts
│   │
│   ├─ orders/
│   │   ├─ controllers/
│   │   ├─ services/
│   │   ├─ repositories/
│   │   ├─ dtos/
│   │   ├─ entities/
│   │   ├─ events/
│   │   └─ orders.module.ts
│   │
│   ├─ payments/
│   │   ├─ controllers/
│   │   ├─ services/
│   │   ├─ repositories/
│   │   ├─ dtos/
│   │   ├─ entities/
│   │   └─ payments.module.ts
│   │
│   └─ notifications/
│       ├─ services/
│       ├─ events/
│       └─ notifications.module.ts
│
├─ common/
│   ├─ filters/
│   ├─ guards/
│   ├─ interceptors/
│   ├─ pipes/
│   ├─ decorators/
│   └─ utils/
│
├─ infrastructure/
│   ├─ database/
│   │   ├─ typeorm/
│   │   │   ├─ entities/
│   │   │   └─ repositories/
│   │   └─ prisma/
│   ├─ api-clients/
│   │   ├─ internal/
│   │   │   └─ user-api.client.ts
│   │   └─ external/
│   │       ├─ payment-gateway.client.ts
│   │       └─ shipping-gateway.client.ts
│   ├─ cache/
│   │   └─ redis.service.ts
│   ├─ queue/
│   │   ├─ rabbitmq/
│   │   └─ kafka/
│   └─ logger/
│       └─ logger.service.ts
│
├─ events/
│   ├─ publishers/
│   └─ subscribers/
│
├─ shared/
│   ├─ constants/
│   ├─ enums/
│   └─ helpers/
│
├─ tests/
│   ├─ unit/
│   ├─ integration/
│   └─ e2e/
│
├─ app.module.ts
├─ main.ts
└─ config/
    ├─ env/
    └─ configuration.ts

```

---

## 🔹 **Explicando o fluxo complexo**

1. **Chamada API interna**
    - Módulos usam `api-clients/internal/` para chamar outros microservices dentro do mesmo sistema.
    - Ex.: `UserService` chama `OrdersService` via `orders-api.client.ts`.
2. **Chamada API externa**
    - `api-clients/external/` contém integrações com gateways de pagamento, serviços de frete, APIs de terceiros.
3. **Login e autenticação**
    - `auth/strategies` → JWT, OAuth, Local
    - `auth/guards` → roles e permissões
    - `auth/controllers` → endpoints de login/logout/refresh
4. **Eventos e mensagens**
    - `events/publishers` → envia eventos para microservices ou sistemas externos
    - `events/subscribers` → recebe eventos para processamento assíncrono
5. **Cache e filas**
    - `infrastructure/cache` → Redis
    - `infrastructure/queue` → RabbitMQ assíncrono
6. **Módulos principais**
    - `modules/users`, `modules/orders`, `modules/payments` → cada módulo encapsula controllers, services, DTOs, entidades e eventos
7. **Camadas comuns**
    - `common/` → filtros de exceção, interceptors, pipes, decorators e helpers reutilizáveis
8. **Testes**
    - `tests/unit`, `tests/integration`, `tests/e2e` → testes separados por nível
9. **Configurações**
    - `config/env/` → arquivos `.env` por ambiente
    - `config/configuration.ts` → centraliza configuração

---

💡 **Observações:**

- Essa arquitetura permite:
    - Microservices internos
    - Integração com APIs externas
    - Event-driven / Pub-Sub
    - Autenticação e autorização robusta
    - Testes em todos os níveis
    - Cache, logging e filas de processamento
- Foi combinado com a arquitetura **Clean + Hexagonal + CQRS + Event-Driven**, devido a complexidade do sistema.

# 🏗️ **Feature-Based Frontend Architecture (React + TypeScript + Tailwind + shadcn)**

```
src/
├─ features/
│   ├─ auth/
│   │   ├─ components/               # Componentes específicos da feature
│   │   │   ├─ LoginForm.tsx
│   │   │   └─ RegisterForm.tsx
│   │   ├─ hooks/                    # Hooks de dados e estado local da feature
│   │   │   └─ useAuth.ts
│   │   ├─ services/                 # Comunicação com API
│   │   │   └─ authService.ts        # POST /auth/login, /auth/register
│   │   ├─ pages/                    # Páginas da feature
│   │   │   ├─ LoginPage.tsx
│   │   │   └─ RegisterPage.tsx
│   │   ├─ types/                    # Tipos específicos da feature
│   │   │   └─ auth.ts
│   │   └─ utils/                    # Validações e helpers da feature
│   │       └─ validateAuth.ts
│   │
│   ├─ users/
│   │   ├─ components/
│   │   │   ├─ UserTable.tsx
│   │   │   └─ UserCard.tsx
│   │   ├─ hooks/
│   │   │   └─ useUsers.ts           # GET /users
│   │   ├─ services/
│   │   │   └─ usersService.ts
│   │   ├─ pages/
│   │   │   └─ UsersPage.tsx
│   │   ├─ types/
│   │   │   └─ user.ts
│   │   └─ utils/
│   │       └─ formatUser.ts
│   │
│   ├─ orders/
│   │   ├─ components/
│   │   │   ├─ OrderTable.tsx
│   │   │   └─ OrderDetail.tsx
│   │   ├─ hooks/
│   │   │   └─ useOrders.ts          # GET /orders, POST /orders
│   │   ├─ services/
│   │   │   └─ ordersService.ts
│   │   ├─ pages/
│   │   │   └─ OrdersPage.tsx
│   │   ├─ types/
│   │   │   └─ order.ts
│   │   └─ utils/
│   │       └─ calculateTotals.ts
│   │
│   ├─ payments/
│   │   ├─ components/
│   │   │   ├─ PaymentCard.tsx
│   │   │   └─ PaymentList.tsx
│   │   ├─ hooks/
│   │   │   └─ usePayments.ts        # GET /payments
│   │   ├─ services/
│   │   │   └─ paymentsService.ts
│   │   ├─ pages/
│   │   │   └─ PaymentsPage.tsx
│   │   ├─ types/
│   │   │   └─ payment.ts
│   │   └─ utils/
│   │       └─ formatPayment.ts
│   │
│   └─ notifications/
│       ├─ components/
│       │   └─ NotificationList.tsx
│       ├─ hooks/
│       │   └─ useNotifications.ts   # GET /notifications
│       ├─ services/
│       │   └─ notificationsService.ts
│       ├─ pages/
│       │   └─ NotificationsPage.tsx
│       ├─ types/
│       │   └─ notification.ts
│       └─ utils/
│           └─ formatNotification.ts
│
├─ shared/                           # Componentes, hooks e utils globais
│   ├─ components/
│   │   ├─ Button.tsx
│   │   ├─ Modal.tsx
│   │   └─ Loader.tsx
│   ├─ hooks/
│   │   └─ useDebounce.ts
│   ├─ utils/
│   │   ├─ fetcher.ts                 # Wrapper axios/fetch
│   │   ├─ formatDate.ts
│   │   └─ validate.ts
│   └─ constants/
│       └─ routes.ts
│
├─ store/                             # Estado global (Redux / Zustand / React Query)
│   ├─ slices/
│   │   ├─ authSlice.ts
│   │   ├─ usersSlice.ts
│   │   ├─ ordersSlice.ts
│   │   └─ paymentsSlice.ts
│   ├─ middleware/
│   │   └─ apiMiddleware.ts
│   └─ store.ts
│
├─ routes/
│   └─ AppRoutes.tsx                  # Roteamento e guards de rota
│
├─ App.tsx
└─ main.tsx

```

---

## 🔹 **Camadas e responsabilidades**

| Camada | Responsabilidade |
| --- | --- |
| `features/*` | Cada feature é autocontida: components, hooks, services, types, pages e utils. |
| `shared/components` | UI genérica (Botões, Modais, Loaders), estilizada com Tailwind + shadcn/ui. |
| `shared/hooks` | Hooks globais reutilizáveis (`useDebounce`, `useLocalStorage`). |
| `shared/utils` | Helpers, validações, fetcher genérico (`axios/fetch`). |
| `store` | Estado global, cache de API, slices de features. Pode integrar React Query ou Redux Toolkit. |
| `routes` | Roteamento público/privado, guards de autenticação. |
| `services` | Comunicação com APIs externas ou internas da feature. |
| `pages` | Páginas da feature consumindo hooks e components da mesma feature. |

---

## 🔹 **Fluxo de dados completo**

1. **Login**
    - `LoginPage.tsx` → chama hook `useAuth()` → usa `authService.login()` → POST `/auth/login`
    - Salva token no `store/authSlice` e localStorage
2. **Consumir usuários**
    - `UsersPage.tsx` → chama hook `useUsers()`
    - `useUsers()` → chama `usersService.getAll()` → GET `/users`
    - Dados populam `usersSlice` → componente renderiza tabela
3. **Orders & Payments**
    - Hooks específicos (`useOrders`, `usePayments`) chamam serviços
    - Serviços fazem fetch das APIs correspondentes (internas ou externas)
    - Resultado atualizado no slice ou cache do React Query → UI atualizada automaticamente
4. **Notificações**
    - Hook `useNotifications()` → SSE / WebSocket do backend
    - Atualiza store e renderiza `NotificationList.tsx` em tempo real
5. **Estilo**
    - Todos components usam **Tailwind + shadcn/ui** para consistência visual
    - Buttons, Modals, Cards são reutilizáveis em todas as features

    
## ⚡ Tecnologias Utilizadas

### Backend
- Node.js / TypeScript  
- NestJS  
- PostgreSQL (TypeORM)  
- Redis (cache)  
- RabbitMQ  
- JWT para autenticação  
- Axios para chamadas HTTP internas/externas  
- Jest para testes  

### Frontend
- React 18+ / TypeScript  
- Tailwind CSS  
- shadcn/ui  
- React Query  
- Axios para chamadas API  
- React Router v6  

---

## 🚀 Como Rodar o Projeto

### Pré-requisitos
- Node.js 20+  
- Docker (banco e filas)  

### Backend

# Entrar na pasta backend
cd backend

# Instalar dependências
npm install

# Rodar migrations (TypeORM)
npm run migrate

# Rodar aplicação
npm run start:dev

###  Front-end

# Entrar na pasta Front-end
cd frontend

# Instalar dependências
npm install

# Rodar aplicação
npm run dev
