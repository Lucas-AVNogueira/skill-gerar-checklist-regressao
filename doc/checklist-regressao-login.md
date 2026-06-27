# Checklist de Regressão RCRCRC - Login

**Alteração:** Adicionada validação client-side em login.html: regex de e-mail (/^[^\s@]+@[^\s@]+.[^\s@]+$/), comprimento mínimo de 6 caracteres para senha, mensagens de erro inline (#emailError, #passwordError) e feedback visual de borda vermelha (#dc2626)

**Módulos Relacionados:** Login, Esqueci a senha, Cadastro

---

## Recent (Mudanças Recentes)
*Mentalidade: O que mudou e o que está no entorno direto da alteração?*

- [ ] Validar se a regex de e-mail `/^[^\s@]+@[^\s@]+.[^\s@]+$/` aceita endereços de e-mail válidos (ex: usuario@exemplo.com)
- [ ] Validar se a regex de e-mail rejeita e-mails sem @ (ex: usuarioexemplo.com)
- [ ] Validar se a regex de e-mail rejeita e-mails sem domínio (ex: usuario@)
- [ ] Validar se a regex de e-mail rejeita e-mails com espaços em branco (ex: usuario @exemplo.com)
- [ ] Validar se a validação de comprimento mínimo de 6 caracteres para senha funciona corretamente
- [ ] Validar se a mensagem de erro de e-mail (#emailError) aparece quando o e-mail é inválido
- [ ] Validar se a mensagem de erro de senha (#passwordError) aparece quando a senha tem menos de 6 caracteres
- [ ] Validar se a borda vermelha (#dc2626) é aplicada ao campo e-mail quando há erro
- [ ] Validar se a borda vermelha (#dc2626) é aplicada ao campo senha quando há erro
- [ ] Validar se a borda vermelha é removida quando o campo fica válido novamente

---

## Core (Funcionalidades Essenciais)
*Mentalidade: O que não pode quebrar de jeito nenhum no sistema?*

- [ ] Verificar se usuário com e-mail e senha válidos consegue fazer login com sucesso
- [ ] Verificar se o botão de submit está desabilitado ou se o formulário bloqueia submissão quando há erros de validação
- [ ] Verificar se o fluxo de login não é quebrado pela nova validação client-side
- [ ] Verificar se a validação não interfere no link "Esqueci a senha" (módulo relacionado)
- [ ] Verificar se a validação não interfere no link para página de "Cadastro" (módulo relacionado)
- [ ] Verificar se mensagens de erro não obstruem elementos importantes da interface

---

## Risky (Áreas de Alto Risco)
*Mentalidade: Onde o código é complexo ou sensível a falhas colaterais?*

- [ ] Validar se e-mails com subdomínios (ex: usuario@mail.empresa.com.br) passam na validação
- [ ] Validar se e-mails com caracteres especiais permitidos (ex: usuario+tag@exemplo.com) passam ou falham conforme esperado
- [ ] Validar se a regex não falha com e-mails muito longos (próximos ao limite RFC 5321)
- [ ] Validar se a senha com exatamente 6 caracteres é aceita
- [ ] Validar se a senha com 5 caracteres é rejeitada
- [ ] Validar se a regex de e-mail não causa ReDoS (Expressão Regular com negação de negação complexa)
- [ ] Verificar comportamento quando usuário tenta colar e-mail ou senha com espaços em branco
- [ ] Verificar se a validação funciona quando campos são preenchidos via autocomplete do navegador

---

## Configuration (Ambiente e Configurações)
*Mentalidade: Como as variáveis de ambiente ou dados de produção afetam isso?*

- [ ] Validar em navegador Chrome versão atual
- [ ] Validar em navegador Firefox versão atual
- [ ] Validar em navegador Safari
- [ ] Validar em navegador Edge
- [ ] Validar comportamento em dispositivos mobile (responsividade das mensagens de erro)
- [ ] Validar com JavaScript desabilitado (falha graciosa se validação client-side é obrigatória)
- [ ] Validar com diferentes localizações de teclado (ex: QWERTY, AZERTY)
- [ ] Validar se as cores de erro (#dc2626) estão em conformidade com padrão de acessibilidade (contraste)

---

## Repaired (Defeitos Corrigidos)
*Mentalidade: O que foi corrigido recentemente que pode ter gerado novos bugs?*

- [ ] Verificar se a validação client-side não enfraqueceu a validação server-side existente
- [ ] Verificar se o servidor ainda valida e-mail e senha mesmo que o cliente as deixe passar
- [ ] Verificar se mensagens de erro inline não cobrem o campo de entrada ou botões
- [ ] Verificar se as classes CSS para borda vermelha (#dc2626) não conflitam com estilos existentes
- [ ] Verificar se o foco do campo não é perdido após exibição da mensagem de erro

---

## Chronic (Problemas Crônicos)
*Mentalidade: Quais partes desses módulos sempre quebram e exigem atenção?*

- [ ] Validar se e-mails com espaços no início ou final são trimados antes da validação
- [ ] Validar se senhas com espaços no início ou final são trimadas ou aceitas conforme política definida
- [ ] Validar comportamento ao copiar e colar e-mail ou senha (incluindo espaços acidentais)
- [ ] Validar se múltiplas tentativas de validação (blur, change, input) não causam tremulação (flicker) na interface
- [ ] Verificar se a experiência é acessível para usuários com leitores de tela (aria-labels nas mensagens de erro)
- [ ] Verificar se a ordem de tabulação (tab order) entre e-mail, senha e botão submit permanece lógica
- [ ] Validar se testes de regressão automáticos cobrem estes cenários (entrada de dados borderline)

---

## Notas Adicionais

- A regex fornecida (`/^[^\s@]+@[^\s@]+.[^\s@]+$/`) tem limitações: o terceiro ponto (`.`) não está escapado e pode aceitar qualquer caractere. Considere usar `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` para maior precisão.
- Recomenda-se validação server-side robusta complementar a validação client-side.
- Considere adicionar feedback positivo (ex: borda verde ou ícone de check) quando campos são válidos, melhorando experiência do usuário.
