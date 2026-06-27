#  Gerador de Checklist de Regressão

Bem-vindo! Este é um projeto para facilitar testes de regressão usando a **skill `gerar-checklist-regressao`**.

Se você já sentiu aquele frio na espinha ao fazer uma alteração no código e se perguntou *"será que isso quebrou algo em outro lugar?"* — esta skill é para você! 

## O Que Ela Faz?

A skill **gera listas de teste estruturadas e profissionais** a partir de uma alteração no código. Ela usa a heurística **RCRCRC** (desenvolvida por Karen N. Johnson) para organizar os testes em 6 perspectivas diferentes:

| 📍 Categoria | 💭 Pergunta | 🎯 Foco |
|-------------|-----------|--------|
| **Recent** | O que mudou? | Alterações diretas no código |
| **Core** | Não pode quebrar? | Funcionalidades críticas |
| **Risky** | Onde pode dar ruim? | Código complexo e sensível |
| **Configuration** | Depende do ambiente? | Navegadores, dispositivos, permissões |
| **Repaired** | Enfraqueceu algo? | Validações e tratamento de erros |
| **Chronic** | Sempre quebra aqui? | Problemas históricos do módulo |

---

##  Começo Rápido

Nunca usou? Sem problema! Aqui está tudo o que você precisa saber:

### O Comando Básico

Escreva no GitHub Copilot Chat:

```bash
/gerar-checklist-regressao --alteracao="<o que você mudou>" --modulos-relacionados="<que partes são afetadas>"
```

###  Explicando os Parâmetros

**`--alteracao`**: Descreva a mudança com suas palavras
- ✅ Bom: `"Adicionei validação de e-mail no formulário de login"`
- ❌ Evite: `"mudança no código"` (muito vago!)

**`--modulos-relacionados`**: Liste as partes do sistema que podem ser afetadas
- ✅ Bom: `"Login, Esqueci a senha, Cadastro, API de autenticação"`
- ❌ Evite: `"tudo"` (seja específico!)

### Exemplo Prático #1: Teste Simples

Você corrigiu um bug na validação de login:

```bash
/gerar-checklist-regressao --alteracao="Adicionada validação de e-mail com regex, mínimo 6 caracteres para senha, mensagens de erro inline" --modulos-relacionados="Login, Esqueci a senha, Cadastro"
```

**O que acontece?** 
- Skill gera arquivo `checklist-regressao-login.md`
- Você recebe **56 cenários de teste** já organizados
- Pronto para executar testes manualmente ou automatizar

###  Exemplo Prático #2: Com Automação

Se você quer que a skill gere até **código de teste** pronto para usar:

```bash
$gerar-checklist-regressao com --alteracao="Validação de e-mail no login" e --modulos-relacionados="Login, Esqueci a senha"

Agora crie código de automação dos testes em um arquivo MD. Quero adicionar esses cenários no regressivo já automatizado.
```

**O que você recebe?** 
- Checklist completo
- **Seção com código de automação** (pronto para adaptar)
- Instruções de como adicionar à suíte de testes existente

---

## 📁 Estrutura do Projeto

```
teste skill/
├── README.md                           # Este arquivo
├── login.html                          # Arquivo HTML do exemplo (formulário de login)
├── checklist-regressao-login.md        # Checklist inicial (exemplo)
├── doc/
│   └── checklist-regressao-login.md    # Checklist gerado em pasta doc/
└── .github/
    └── skills/
        └── gerar-checklist-regressao/
            └── SKILL.md                # Documentação da skill
```

---

##  Exemplo de Saída

Quando você invoca a skill, ela gera um arquivo Markdown com estrutura semelhante a:

```markdown
# Checklist de Regressão RCRCRC - Login

## Recent (Mudanças Recentes)
*Mentalidade: O que mudou e o que está no entorno direto da alteração?*
- [ ] Validar se a regex de e-mail aceita endereços válidos
- [ ] Validar se a regex de e-mail rejeita e-mails sem @
- ...

## Core (Funcionalidades Essenciais)
*Mentalidade: O que não pode quebrar de jeito nenhum no sistema?*
- [ ] Verificar se usuário com dados válidos consegue fazer login
- ...

## Risky (Áreas de Alto Risco)
*Mentalidade: Onde o código é complexo ou sensível a falhas?*
- [ ] Validar e-mails com subdomínios
- ...

## Configuration (Ambiente e Configurações)
*Mentalidade: Como o ambiente afeta isso?*
- [ ] Validar em navegador Chrome
- [ ] Validar em navegador Firefox
- ...

## Repaired (Defeitos Corrigidos)
*Mentalidade: O que foi corrigido que pode gerar novos bugs?*
- [ ] Verificar se validação client-side não enfraqueceu a server-side
- ...

## Chronic (Problemas Crônicos)
*Mentalidade: Quais partes sempre quebram?*
- [ ] Validar se e-mails com espaços são trimados
- ...
```

---

## ✅ Depois de Gerar o Checklist

Agora você tem um arquivo com 50+ cenários. O que fazer?

1. 📂 **Abra o arquivo** (ex: `checklist-regressao-login.md`)
2. 🧪 **Execute os testes** (na ordem que fizer sentido)
3. ☑️ **Marque o que testou** (clique na caixa `[ ]` para `[x]`)
4. 🐛 **Encontrou um bug?** Anote junto ao item que falhou
5. 🔄 **Reutilize no futuro** — guarde para próximas alterações similares

---

## 🎯 Quando Usar (e Quando Não)

### ✅ Use a Skill Se:

- Fez uma alteração e quer garantir que nada quebrou
- Precisa testar impactos em múltiplos módulos
- Quer uma lista de teste estruturada (sem adivinhar o que testar)
- Quer gerar automação de testes baseada em regressão
- Seu time quer padronizar testes de regressão

### ❌ Talvez Não Precise Se:

- A mudança é minúscula (ex: ajuste de espaçamento CSS)
- Você tem um teste de regressão automático 100% cobrindo essa área

---

## 📚 Entendo RCRCRC

Curioso sobre a heurística? Aqui está o resumo:

| Categoria | O que testar | Por quê? | Exemplo |
|-----------|------------|---------|---------|
| **Recent** | A mudança exata | Óbvio: validar o que você mudou | Testar nova regex de e-mail |
| **Core** | Fluxo principal (happy path) | Não pode quebrar | Fazer login com dados válidos |
| **Risky** | Edge cases, limites | Bugs gostam de esconder aqui | E-mail com 254 caracteres, senhas com emoji |
| **Configuration** | Ambiente & navegador | Bugs específicos de ambiente | Chrome, Firefox, mobile, locale diferente |
| **Repaired** | O que não deve quebrar | Validações que já existem | Server-side validation ainda funciona? |
| **Chronic** | Problemas históricos | Padrões que sempre quebram | Trim de espaços, autocomplete do navegador |

---

##  Personalização

### Adicionando Novos Módulos

Para criar checklists para diferentes módulos, simplesmente invoque a skill com os novos parâmetros:

```bash
/gerar-checklist-regressao --alteracao="Novo endpoint de autenticação via API" --modulos-relacionados="API, Autenticação, Sessão, Cache"
```

### Salvando em Pastas Específicas

Você pode especificar uma pasta de destino:

```bash
/gerar-checklist-regressao --alteracao="..." --modulos-relacionados="..." --pasta="doc/api"
```

---

## ❓ Perguntas Frequentes

**P: Quanto tempo leva para gerar um checklist?**
R: Segundos! A skill cria um arquivo completo em poucos segundos.

**P: Preciso executar todos os testes?**
R: Idealmente sim, mas priorize os da categoria **Core** e **Risky** primeiro.

**P: Posso editar o checklist depois?**
R: Claro! Adicione, remova ou customize cenários conforme necessário.

**P: E se eu não souber todos os módulos relacionados?**
R: Comece com os óbvios. A skill vai ajudar a descobrir mais impactos.

---

## 📖 Quer Saber Mais?

Para detalhes técnicos e configurações avançadas, veja:

👉 [.github/skills/gerar-checklist-regressao/SKILL.md](.github/skills/gerar-checklist-regressao/SKILL.md)

---

## 💡 Dicas  🤓

**1. Detalhe a mudança**
- Ruim: `"Ajuste na validação"`
- Bom: `"Regex para validar e-mail, mín 6 chars para senha, erro em vermelho #dc2626"`

**2. Pense em cascata**
- Não esqueça módulos indiretos (ex: se você mexe em Login, pense em "Esqueci a senha", "Cadastro", "API")

**3. Reutilize & adapte**
- Guarde checklists antigos — quando tiver uma mudança similar, adapte o anterior

**4. Peça automação**
- A skill pode gerar código de teste! Basta pedir no final

**5. Documente tudo**
- Guarde os checklists em um repositório ou wiki — seus futuros eu e seu time agradecem!

---

##  Suporte

Teve problema? Dúvida?
- Consulte a documentação da skill: `SKILL.md`
- Revise os exemplos deste README


---

## � Como Subir para o GitHub

Quer compartilhar essa skill com o mundo? Siga este passo a passo para fazer upload do projeto:

### 📋 Pré-requisitos

- ✅ Git instalado ([download aqui](https://git-scm.com/))
- ✅ Conta GitHub ([criar aqui](https://github.com/signup))
- ✅ Acesso ao repositório `https://github.com/Lucas-AVNogueira/skill-gerar-checklist-regressao`

### 🔑 Passo 1: Configurar Git (primeira vez apenas)

Abra o terminal/PowerShell e configure seu nome e e-mail:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@example.com"
```

### 📍 Passo 2: Clonar o Repositório

Navegue até a pasta onde quer trabalhar e clone o repositório:

```bash
git clone https://github.com/Lucas-AVNogueira/skill-gerar-checklist-regressao.git
cd skill-gerar-checklist-regressao
```

### 📁 Passo 3: Copiar Seus Arquivos

Copie os arquivos do seu projeto local para a pasta clonada:

```
De: c:\projetos\teste skill\
Para: skill-gerar-checklist-regressao\
```

**Arquivos principais a copiar:**
- `README.md` (atualizado)
- `.github/skills/gerar-checklist-regressao/SKILL.md`
- `doc/checklist-regressao-login.md`
- `login.html`
- `checklist-regressao-login.md`

### ✅ Passo 4: Ver Status das Mudanças

Verifique quais arquivos foram adicionados ou modificados:

```bash
git status
```

**Você verá algo como:**
```
On branch main
Changes not staged for commit:
  modified:   README.md
  new file:   doc/checklist-regressao-login.md
  ...
```

### 📝 Passo 5: Adicionar Arquivos ao Staging

Adicione todos os arquivos modificados:

```bash
git add .
```

Ou adicione apenas arquivos específicos:

```bash
git add README.md
git add doc/checklist-regressao-login.md
git add .github/skills/gerar-checklist-regressao/SKILL.md
```

### 💬 Passo 6: Fazer Commit

Crie um commit com uma mensagem descritiva:

```bash
git commit -m "docs: Atualizar README humanizado e adicionar checklist de exemplo"
```

**Dicas de bom commit:**
- ✅ Bom: `"feat: Adicionar validação RCRCRC para testes"`
- ✅ Bom: `"docs: Humanizar README com exemplos práticos"`
- ❌ Evite: `"alterações"` ou `"fix bug"`

### 🚀 Passo 7: Fazer Push (Enviar para GitHub)

Envie seus commits para o repositório remoto:

```bash
git push origin main
```

**Se pedir autenticação:**
- Use seu token de acesso pessoal (PAT) ou senha

### ✨ Pronto!

Seu projeto está no GitHub! Visite:

👉 [github.com/Lucas-AVNogueira/skill-gerar-checklist-regressao](https://github.com/Lucas-AVNogueira/skill-gerar-checklist-regressao)

---

## 🔄 Para Próximas Atualizações

Próxima vez será bem mais rápido:

```bash
cd skill-gerar-checklist-regressao
git add .
git commit -m "docs: Descrever o que mudou"
git push origin main
```

---

## ❓ Dúvidas Comuns

**P: Recebi erro "Permission denied"?**
R: Verifique se tem acesso ao repositório ou se precisa de autenticação SSH/PAT.

**P: Como desfazer um commit?**
R: Use `git revert HEAD` ou `git reset --soft HEAD~1` para voltar atrás.

**P: Posso fazer branches?**
R: Sim! Use `git checkout -b minha-branch` para criar branches antes de fazer push.

**P: Como ver o histórico?**
R: Use `git log` para ver todos os commits.

---

## �📄 Licença

Este projeto é para fins de demonstração e teste da skill `gerar-checklist-regressao`.

---
