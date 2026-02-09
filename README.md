# Basic API Master

Uma API RESTful completa para gerenciamento de tours e experiências de viagem, desenvolvida com Node.js, Express.js e MongoDB.

## 🚀 Funcionalidades

- ✅ **CRUD de Tours** - Criar, ler, atualizar e deletar tours
- 🔐 **Autenticação JWT** - Sistema seguro de autenticação de usuários
- 💳 **Pagamento com Stripe** - Integração completa de pagamentos
- ⭐ **Sistema de Avaliações** - Reviews e ratings de tours
- 📧 **Notificações por E-mail** - Confirmações e notificações automáticas
- 👤 **Gerenciamento de Usuários** - Perfis, permissões e controle de acesso
- 🖼️ **Upload de Imagens** - Processamento e compressão de imagens
- 🔒 **Segurança Avançada** - Rate limiting, sanitização, proteção XSS
- 🔍 **Filtros e Busca** - Filtros, ordenação e paginação de dados
- 📱 **Renderização de Views** - Interface web com Pug templates

## 🛠️ Stack Tecnológico

### Backend
- **Node.js** - Runtime JavaScript
- **Express.js** - Framework web
- **MongoDB** - Banco de dados NoSQL
- **Mongoose** - ODM para MongoDB

### Segurança
- **JWT** - Autenticação
- **Helmet** - Headers de segurança
- **express-mongo-sanitize** - Proteção contra NoSQL injection
- **xss-clean** - Proteção contra XSS
- **express-rate-limit** - Rate limiting
- **hpp** - Prevenção de parameter pollution

### Integração
- **Stripe** - Processamento de pagamentos
- **Nodemailer** - Envio de e-mails
- **Sharp** - Processamento de imagens
- **Multer** - Upload de arquivos

### Ferramentas de Desenvolvimento
- **Nodemon** - Auto-reload
- **Parcel** - Bundler para JavaScript
- **ESLint** - Linter
- **Prettier** - Formatter

## 📋 Pré-requisitos

- Node.js >= 10.0.0
- MongoDB instalado e rodando
- npm ou yarn

## 🔧 Instalação

### 1. Clone o repositório
```bash
git clone <url-do-repositorio>
cd natours
```

### 2. Instale as dependências
```bash
npm install
```

### 3. Configure as variáveis de ambiente
Crie um arquivo `.env` na raiz do projeto:

```env
NODE_ENV=development
PORT=3000

DATABASE=mongodb://localhost:27017/natours
DATABASE_PASSWORD=sua-senha

JWT_SECRET=sua-chave-super-secreta
JWT_EXPIRES_IN=90d

STRIPE_API_KEY=sua-chave-stripe
STRIPE_WEBHOOK_SECRET=seu-webhook-secret

EMAIL_HOST=seu-smtp-host
EMAIL_PORT=587
EMAIL_USER=seu-email
EMAIL_PASSWORD=sua-senha-email
EMAIL_FROM=noreply@natours.com

STRIPE_WEBHOOK_ENDPOINT_SECRET=secret
```

### 4. Importe dados de desenvolvimento (opcional)
```bash
npm run dev:import-data
```

## 🚀 Como Executar

### Desenvolvimento
```bash
npm start
```

A API estará disponível em `http://localhost:3000`

### Produção
```bash
npm run start:prod
```

### Debug
```bash
npm run debug
```

### Build JavaScript
```bash
npm run build:js
```

## 📚 Endpoints da API

### Tours (`/api/v1/tours`)
- `GET /` - Listar todos os tours
- `GET /:id` - Obter um tour específico
- `POST /` - Criar novo tour (admin)
- `PATCH /:id` - Atualizar tour (admin)
- `DELETE /:id` - Deletar tour (admin)
- `GET /top-5-cheap` - Top 5 tours mais baratos
- `GET /tour-stats` - Estatísticas de tours
- `GET /monthly-plan/:year` - Plano mensal de tours

### Usuários (`/api/v1/users`)
- `GET /` - Listar usuários (admin)
- `GET /:id` - Obter usuário específico
- `POST /signup` - Registrar novo usuário
- `POST /login` - Fazer login
- `POST /logout` - Fazer logout
- `GET /me` - Obter dados do usuário logado
- `PATCH /updateMe` - Atualizar dados do usuário
- `PATCH /updateMyPassword` - Alterar senha
- `POST /forgotPassword` - Solicitar reset de senha
- `PATCH /resetPassword/:token` - Resetar senha
- `DELETE /:id` - Deletar usuário (admin)

### Avaliações (`/api/v1/reviews`)
- `GET /` - Listar todas as avaliações
- `GET /:id` - Obter avaliação específica
- `POST /` - Criar nova avaliação (usuário logado)
- `PATCH /:id` - Atualizar avaliação
- `DELETE /:id` - Deletar avaliação

### Reservas (`/api/v1/bookings`)
- `GET /` - Listar reservas (admin)
- `GET /checkout-session/:tourID` - Criar sessão de checkout Stripe
- `POST /` - Criar reserva (admin)
- `GET /:id` - Obter detalhes da reserva
- `PATCH /:id` - Atualizar reserva (admin)
- `DELETE /:id` - Cancelar reserva (admin)

### Views
- `GET /` - Página inicial
- `GET /tours` - Listagem de tours
- `GET /tour/:slug` - Detalhes do tour
- `GET /login` - Página de login
- `GET /me` - Meu perfil
- `GET /my-tours` - Minhas reservas

## 🔑 Autenticação

A API utiliza JWT (JSON Web Tokens) para autenticação. 

### Fluxo de login
1. Usuário envia credenciais para `POST /api/v1/users/login`
2. API retorna um JWT no cookie `jwt`
3. Token é enviado automaticamente em requisições subsequentes

### Headers de Autenticação
```bash
Authorization: Bearer <seu-jwt-token>
```

## 🛡️ Recursos de Segurança

- **Rate Limiting** - 100 requisições por hora por IP
- **Helmet** - Headers de segurança HTTP
- **CORS** - Controle de origem cruzada
- **Sanitização** - Proteção contra NoSQL injection e XSS
- **HTTPS** - Recomendado em produção
- **Validação** - Validação em client e server side
- **Encriptação de Senha** - bcryptjs com 12 rounds

## 📧 Sistema de E-mails

A API envia e-mails para:
- Bem-vindas de novos usuários
- Confirmação de reservas
- Reset de senha
- Notificações de pagamento

Configure seu provedor SMTP no `.env`

## 💳 Integração com Stripe

Para processar pagamentos:
1. Obtenha suas chaves Stripe em [stripe.com](https://stripe.com)
2. Configure `STRIPE_API_KEY` no `.env`
3. Acesse `/api/v1/bookings/checkout-session/:tourID` para criar sessão de checkout

## 🗄️ Estrutura do Banco de Dados

### Tours
```javascript
{
  name: String,
  rating: Number,
  price: Number,
  description: String,
  imageCover: String,
  images: [String],
  createdAt: Date,
  summary: String,
  ...
}
```

### Usuários
```javascript
{
  name: String,
  email: String,
  password: String (criptografada),
  passwordChangedAt: Date,
  role: String (user/admin),
  ...
}
```

### Avaliações
```javascript
{
  review: String,
  rating: Number,
  createdAt: Date,
  tour: ObjectId,
  user: ObjectId,
  ...
}
```

### Reservas
```javascript
{
  tour: ObjectId,
  user: ObjectId,
  price: Number,
  createdAt: Date,
  paid: Boolean,
  ...
}
```

## 🤝 Contribuindo

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está sob a licença ISC.

## 👨‍💻 Autor

**Alberto Dgedge**

## 📞 Suporte

Para dúvidas ou problemas, abra uma issue no repositório.

## 🔄 Versão

v1.0.0

---

Desenvolvido com ❤️ usando Node.js e Express.js
