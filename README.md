<div align="center">

<img src="img/logo-motalab.png" alt="MotaLab Laboratório e Clínica" width="220">

# MotaLab Laboratório e Clínica

**Site institucional e plataforma do Cartão Fidelidade MotaLab**

Exames laboratoriais, consultas com especialistas e um cartão de descontos para toda a família,<br>
em São Sebastião de Lagoa de Roça, Queimadas e Alagoa Nova (PB).

[![Site](https://img.shields.io/badge/site-motalabclinica.com.br-17474F?style=flat-square)](https://www.motalabclinica.com.br)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES_Modules-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-Auth_+_Postgres-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![Vercel](https://img.shields.io/badge/deploy-Vercel-000000?style=flat-square&logo=vercel&logoColor=white)

</div>

<img width="1359" height="482" alt="Página inicial do MotaLab" src="https://github.com/user-attachments/assets/965422fc-52de-4c1d-bf45-afb4a00301c6" />

---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Funcionalidades](#funcionalidades)
- [Tecnologias](#tecnologias)
- [Arquitetura](#arquitetura)
- [Banco de dados](#banco-de-dados)
- [Fluxos principais](#fluxos-principais)
- [Segurança](#segurança)
- [Estrutura de pastas](#estrutura-de-pastas)
- [Como rodar localmente](#como-rodar-localmente)
- [Deploy](#deploy)
- [Configuração](#configuração)
- [Histórico: migração do Firebase](#histórico-migração-do-firebase)
- [Contato](#contato)

---

## Sobre o projeto

O **MotaLab** é um laboratório de análises clínicas e clínica médica com três unidades na Paraíba. Este repositório contém o site público da empresa, que tem dois objetivos:

1. **Apresentar a clínica**: serviços, combos de exames, corpo de especialistas, equipe, unidades e contato, com agendamento direto pelo WhatsApp.
2. **Vender e gerenciar o Cartão Fidelidade MotaLab**: o paciente se cadastra pelo celular, inclui até 5 dependentes, paga a adesão via Pix e passa a usar um cartão digital que dá descontos na clínica e na farmácia parceira.

### O Cartão Fidelidade

| Benefício | Desconto |
| --- | --- |
| Exames laboratoriais | até **50%** |
| Consultas com especialistas | até **40%** |
| Clínico geral | **gratuito** |
| Farma Lagos (farmácia parceira) | desconto exclusivo |

| Condição | Valor |
| --- | --- |
| Adesão | **R$ 70,00** via Pix |
| Validade | 12 meses, com renovação anual |
| Cobertura | Titular + até 5 dependentes |
| Cartão adicional | R$ 20,00 por cartão |
| Onde usar | Lagoa de Roça, Queimadas e Alagoa Nova |

> O cartão é pessoal e intransferível e **não é um plano de saúde** regulamentado pela ANS.

---

## Funcionalidades

### Site institucional (`index.html`)
- Apresentação dos serviços: exames, consultas, combos e cartão.
- **Combos de exames** (Infantil, Feminino e Masculino) com modal do folheto e botão de agendamento pelo WhatsApp.
- **Carrossel de especialistas** (cardiologia, endocrinologia, ginecologia, otorrino, pediatria, geriatria, nefrologia, nutrição, fisioterapia, ultrassom e outras).
- Seção da equipe, unidades, endereço, telefones e redes sociais.
- SEO completo: metatags, Open Graph, dados estruturados **Schema.org** (`MedicalClinic`), `sitemap.xml` e `robots.txt`.

### Página do cartão (`cartao.html`)
- Landing page do cartão: benefícios, comparativo "com e sem o cartão", condições e passo a passo de contratação.
- **Cadastro em etapas**: dados pessoais → dependentes → pagamento Pix → criação da conta.
- **Pix** com chave copiável e QR Code, e mensagem pronta no WhatsApp para enviar o comprovante.
- **Tela "aguardando ativação"**, que se atualiza sozinha quando a equipe aprova o pagamento no painel.
- **Cartão digital** com frente e verso (animação de virar), número, validade, titular e dependentes.
- **Área "Minha Conta"**: editar nome e telefone, adicionar/remover dependentes, trocar e-mail e senha.
- **Login**, recuperação de senha por e-mail (ou pelo WhatsApp, para quem perdeu acesso ao e-mail) e verificação de e-mail.

### Página de segurança da conta (`acao.html`)
Recebe os links enviados por e-mail e conclui a ação:

| Parâmetro `type` | Ação |
| --- | --- |
| `recovery` | Criar nova senha |
| `email_change` | Confirmar a troca de e-mail |
| `magiclink` / `email` / `signup` | Confirmar o e-mail da conta |

O código do link só é usado quando a pessoa **clica no botão** da página. Assim, antivírus e filtros de e-mail que "abrem" links automaticamente não invalidam o link antes da hora.

---

## Tecnologias

| Camada | Tecnologia | Uso no projeto |
| --- | --- | --- |
| Front-end | **HTML5** semântico | Páginas estáticas (`index`, `cartao`, `acao`) |
| Estilo | **CSS3** puro (custom properties, responsivo) | `motalab.css`, design system próprio |
| Lógica | **JavaScript** (ES Modules, sem framework) | `motalab.js`, `acao.js`, `motalab-ui.js` |
| Tipografia | **IBM Plex Sans** (Google Fonts) | Fonte única do site |
| Autenticação | **Supabase Auth** | Login por e-mail e senha, recuperação, troca e verificação de e-mail |
| Banco de dados | **Supabase (PostgreSQL)** com Row Level Security | Pacientes, dependentes e cartões |
| Funções no servidor | **Supabase RPC** (funções SQL) e **Edge Function** | Criação do cadastro e migração de senhas antigas |
| E-mail | **Resend** (SMTP) | E-mails transacionais de `noreply@motalabclinica.com.br` |
| Pagamento | **Pix** (payload EMV / BR Code) | Adesão de R$ 70,00 |
| Atendimento | **WhatsApp** (links `wa.me`) | Agendamentos, comprovantes e suporte |
| Hospedagem | **Vercel** | Deploy, URLs limpas e cabeçalhos de segurança |
| Controle de versão | **Git + GitHub** | [dplayerf/mota-lab](https://github.com/dplayerf/mota-lab) |

O site **não tem etapa de build** nem dependências de npm: o SDK do Supabase está versionado localmente em `js/supabase-2.45.4.js`, o que também permite uma política de conteúdo (CSP) restrita a `'self'`.

---

## Arquitetura

```
                ┌──────────────────────────┐
  Visitante ──▶ │  Vercel (site estático)  │
                │  index · cartao · acao   │
                └────────────┬─────────────┘
                             │ supabase-js (chave anon)
                             ▼
        ┌────────────────────────────────────────────┐
        │                  Supabase                  │
        │  Auth ── PostgreSQL + RLS ── Edge Function │
        │            (pacientes, dependentes)        │
        └───────┬───────────────────────┬────────────┘
                │ SMTP                  │
                ▼                       ▼
          Resend (e-mails)     Painel administrativo
                               (mota-lab-admin, repositório separado)
```

- O **site** é 100% estático e fala direto com o Supabase pelo navegador usando a chave pública (`anon`).
- **Quem protege os dados é o banco**, por meio de Row Level Security e funções com regras próprias. O site nunca usa a chave `service_role`.
- O **painel administrativo** (repositório `mota-lab-admin`) é onde a equipe ativa, renova e inativa cartões. O esquema do banco (`supabase/schema.sql`) e os modelos de e-mail (`supabase/emails/`) ficam nesse repositório.

---

## Banco de dados

Banco **PostgreSQL** gerenciado pelo **Supabase**.

### Tabelas

#### `pacientes`
Um registro por titular do cartão. O `id` é o mesmo da conta de login no Supabase Auth.

| Coluna | Descrição |
| --- | --- |
| `id` | UUID da conta (`auth.users`) |
| `nome`, `cpf`, `nascimento` | Dados pessoais do titular |
| `email`, `telefone` | Contato |
| `num_cartao` | Número do cartão (8 dígitos, gerado pelo banco) |
| `data_adesao`, `data_validade`, `data_renovacao` | Datas do plano |
| `status` | `pendente` · `ativo` · `inativo` |
| `email_verificado_em` | Quando o e-mail foi confirmado |

#### `dependentes`
Até **5** por titular.

| Coluna | Descrição |
| --- | --- |
| `id` | Identificador |
| `paciente_id` | Referência ao titular em `pacientes` |
| `nome`, `nascimento`, `cpf` | Dados do dependente |
| `parentesco` | Cônjuge, Filho(a), Pai/Mãe, Irmão(ã) ou Outro |

```
pacientes (1) ──────< (0..5) dependentes
```

### Funções (RPC)

| Função | O que faz |
| --- | --- |
| `criar_meu_cadastro` | Cria o cadastro do usuário logado. **O banco** define o número do cartão, as datas e o status `pendente`; o site não consegue ativar o próprio cartão. Recusa CPF duplicado. |
| `apagar_minha_conta_sem_cadastro` | Desfaz uma conta de login que ficou sem cadastro (erro no meio do processo), para a pessoa poder tentar de novo com o mesmo e-mail. |
| `marcar_meu_email_verificado` | Registra a confirmação do e-mail feita pela página `/acao`. |

### Edge Function

| Função | O que faz |
| --- | --- |
| `login-legado` | Ponte para contas criadas na época do Firebase: valida a senha antiga e a transfere para o Supabase Auth no primeiro login, sem o paciente perceber. Tem limite de tentativas. |

### Regras de acesso (resumo)
- O paciente **só lê e altera o próprio cadastro**; não consegue listar outros pacientes.
- Pelo site, o paciente pode mudar apenas nome, telefone, e-mail e dependentes.
- **Status, validade, número do cartão e CPF** só mudam pelo painel administrativo.

---

## Fluxos principais

### Contratação do cartão
```
Cadastro (dados + senha)
   └─▶ Dependentes (opcional, até 5)
         └─▶ Pix R$ 70,00
               └─▶ supabase.auth.signUp()          → cria a conta
                     └─▶ rpc criar_meu_cadastro    → cria o cartão "pendente"
                           └─▶ WhatsApp com o comprovante
                                 └─▶ Equipe ativa no painel
                                       └─▶ Cartão aparece ativo sozinho no site
```
Se a criação do cadastro falhar, a conta vazia é removida (`apagar_minha_conta_sem_cadastro`) e o usuário é desconectado, evitando contas "órfãs".

### Ativação em tempo quase real
Enquanto o cartão está `pendente`, o site consulta o banco periodicamente (e sempre que a aba volta a ficar visível). Quando o painel muda o status para `ativo`, o cartão é exibido sem precisar recarregar a página.

### Recuperação de senha
1. O paciente informa o e-mail → `resetPasswordForEmail`.
2. O Supabase envia o e-mail pelo Resend com link para `/acao?token_hash=...&type=recovery`.
3. Na página `/acao`, o paciente clica no botão, o token é validado (`verifyOtp`) e ele define a nova senha.

Alternativa: quem não tem mais acesso ao e-mail pode pedir ajuda pelo WhatsApp informando o CPF.

### Troca de e-mail
`auth.updateUser({ email })` envia a confirmação para o novo endereço; a mudança só vale depois que o link é aberto em `/acao` (`type=email_change`).

---

## Segurança

Cabeçalhos HTTP definidos em `vercel.json` para todas as páginas:

| Cabeçalho | Efeito |
| --- | --- |
| `Content-Security-Policy` | Scripts apenas do próprio domínio; conexões apenas com o Supabase do projeto; fontes do Google Fonts |
| `Strict-Transport-Security` | Força HTTPS (2 anos, com preload) |
| `X-Frame-Options: DENY` + `frame-ancestors 'none'` | Impede que o site seja embutido em outros sites (clickjacking) |
| `X-Content-Type-Options: nosniff` | Bloqueia interpretação errada de tipos de arquivo |
| `Referrer-Policy` | Limita o envio da URL de origem |
| `Permissions-Policy` | Desliga câmera, microfone, localização, pagamento e sensores |
| `Cross-Origin-Opener-Policy` / `Resource-Policy` | Isola o site de outras origens |

Outras medidas:
- A página `/acao` não é indexada (`noindex`, `robots.txt`), não usa cache (`no-store`) e **remove o token da barra de endereço** assim que carrega.
- Todo texto vindo do banco é escapado antes de ir para a tela (proteção contra XSS).
- Validação de CPF, data de nascimento e telefone no cliente, e validação definitiva no banco.
- Tratamento de limite de tentativas (*rate limit*) no login e no cadastro.
- Arquivos internos (`.md`, `firebase.rules`, `.vscode`, `_versao-firebase`) não são publicados (`.vercelignore`).

---

## Estrutura de pastas

```
mota-lab/
├── index.html            # Página inicial (institucional)
├── cartao.html           # Landing page e área do Cartão Fidelidade
├── acao.html             # Página de segurança da conta (links dos e-mails)
├── motalab.css           # Estilos de todo o site (design system)
├── motalab.js            # Lógica do cartão: sessão, cadastro, Pix, conta
├── acao.js               # Lógica da página /acao (senha e e-mail)
├── motalab-ui.js         # Animações e interações da interface (sem dependências)
├── js/
│   └── supabase-2.45.4.js   # SDK do Supabase (versão fixa, servida localmente)
├── img/                  # Logos, favicons, fotos dos médicos, combos e equipe
├── vercel.json           # URLs limpas e cabeçalhos de segurança
├── .vercelignore         # Arquivos que não vão para o site publicado
├── robots.txt            # Regras para buscadores
├── sitemap.xml           # Mapa do site para o Google
├── site.webmanifest      # Manifesto (ícone e cor do app no celular)
├── CNAME                 # Domínio personalizado
├── modelos-de-email.md   # Configuração dos e-mails no Supabase + Resend
├── firebase.rules        # (legado) Regras da versão antiga em Firebase
└── _versao-firebase/     # (legado) Código da versão antiga em Firebase
```

---

## Como rodar localmente

Não é preciso instalar nada além de um servidor estático, porque o site usa ES Modules (que não funcionam abrindo o arquivo direto com `file://`).

**Opção 1: VS Code + Live Server** (já configurado na porta `5501`)
1. Abra a pasta no VS Code.
2. Clique com o botão direito em `index.html` → **Open with Live Server**.
3. Acesse `http://127.0.0.1:5501`.

**Opção 2: linha de comando**
```bash
# Python
python -m http.server 5501

# ou Node
npx serve -l 5501
```

**Opção 3: Vercel CLI** (reproduz URLs limpas e cabeçalhos do `vercel.json`)
```bash
npx vercel dev
```

> Em `localhost`, os links dos e-mails continuam apontando para o domínio oficial, pois a URL é definida no Supabase.

---

## Deploy

O site é publicado na **Vercel** a partir do repositório no GitHub:

1. Cada `git push` na branch principal gera um novo deploy automático.
2. `cleanUrls` faz `/cartao` e `/acao` funcionarem sem `.html`.
3. O domínio personalizado é configurado na Vercel (e registrado em `CNAME`).

---

## Configuração

### Supabase
A URL do projeto e a chave **pública** (`anon`) ficam em `motalab.js` e `acao.js`. Isso é seguro porque o acesso é controlado pelas regras do banco.

> ⚠️ Nunca coloque a chave `service_role` no código do site.

Se trocar de projeto Supabase, atualize também o `connect-src` da CSP em `vercel.json`.

### Dados do Pix e WhatsApp
No objeto `CFG`, no topo de `motalab.js`:

```js
const CFG = {
  CHAVE_PIX: "...",       // chave Pix
  NOME_PIX: "MotaLab Saude",
  CIDADE_PIX: "Lagoa de Roca",
  VALOR_PIX: "70.00",     // valor da adesão
  MAX_MBR: 5,             // máximo de dependentes
  WPP_NUM: "55839...",    // WhatsApp de atendimento
};
```

### E-mails
Remetente, SMTP (Resend) e modelos são configurados no painel do Supabase. O passo a passo está em [`modelos-de-email.md`](modelos-de-email.md).

| Onde (Supabase) | Configuração |
| --- | --- |
| Authentication → Providers → Email | *Confirm email* e *Secure email change* desligados |
| Authentication → URL Configuration | Site URL `https://www.motalabclinica.com.br` |
| Authentication → Emails → SMTP | `smtp.resend.com`, porta `465` |
| Authentication → Emails → Templates | *Reset Password* e *Change Email Address* |

---

## Histórico: migração do Firebase

A primeira versão do site usava **Firebase** (Authentication, Firestore e Storage). O projeto foi migrado para **Supabase** para ter um banco relacional (PostgreSQL), regras de negócio dentro do banco e e-mails pelo domínio próprio.

- O código antigo está em `_versao-firebase/` e as regras antigas em `firebase.rules`, apenas como referência. Nenhum dos dois é publicado.
- Pacientes cadastrados na época do Firebase continuam entrando com a **mesma senha**: a Edge Function `login-legado` migra a conta no primeiro login.
- Links de e-mail antigos do Firebase (`oobCode`) são reconhecidos pela página `/acao`, que orienta o paciente a pedir um novo link.

---

## Contato

**MotaLab Laboratório e Clínica**
Rua José Rodrigues Coura, 51, São Sebastião de Lagoa de Roça, PB
Unidades em Queimadas e Alagoa Nova (PB)
CNPJ 43.909.626/0001-28

📞 (83) 98869-5500 · (83) 99123-9706
🌐 [www.motalabclinica.com.br](https://www.motalabclinica.com.br)
📷 [@motalab.laboratorio](https://www.instagram.com/motalab.laboratorio)

---

<div align="center">
<sub>© MotaLab Laboratório e Clínica. Todos os direitos reservados.</sub>
</div>
