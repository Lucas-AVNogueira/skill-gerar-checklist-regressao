### Checklist de Regressão RCRCRC

> **Alteração:** Adicionada validação client-side em `login.html`: regex de e-mail (`/^[^\s@]+@[^\s@]+\.[^\s@]+$/`), comprimento mínimo de 6 caracteres para senha, mensagens de erro inline (`#emailError`, `#passwordError`) e feedback visual de borda vermelha (`#dc2626`).
>
> **Módulos relacionados:** Login, Esqueci a Senha, Cadastro

---

#### Recent (Mudanças Recentes)
*Mentalidade: O que mudou e o que está no entorno direto da alteração?*

- [ ] Validar se ao submeter o formulário com e-mail inválido (sem `@`) a mensagem "Informe um e-mail válido." é exibida em `#emailError`
- [ ] Validar se ao submeter com e-mail inválido a borda do campo `#email` muda para `#dc2626`
- [ ] Validar se ao submeter com senha com menos de 6 caracteres a mensagem "A senha deve ter pelo menos 6 caracteres." é exibida em `#passwordError`
- [ ] Validar se ao submeter com senha inválida a borda do campo `#password` muda para `#dc2626`
- [ ] Validar se, após corrigir o e-mail e resubmeter, `#emailError` é ocultado e a borda vermelha é removida
- [ ] Validar se, após corrigir a senha e resubmeter, `#passwordError` é ocultado e a borda vermelha é removida
- [ ] Validar se o formulário não é submetido (navegação não ocorre) enquanto houver erros de validação
- [ ] Verificar se o comportamento de `trim()` aplicado ao e-mail antes da validação regex está correto (espaços no início/fim não devem ser aceitos como parte do e-mail válido)

---

#### Core (Funcionalidades Essenciais)
*Mentalidade: O que não pode quebrar de jeito nenhum no sistema?*

- [ ] Validar se o fluxo de login com e-mail e senha válidos exibe o feedback de sucesso ("Login realizado com sucesso!")
- [ ] Validar se o campo de e-mail aceita o formato `usuario@dominio.com` sem exibir erros
- [ ] Validar se o campo de senha aceita exatamente 6 caracteres sem exibir erros
- [ ] Validar se o atributo `novalidate` no `<form>` desabilita a validação nativa do browser, delegando ao script client-side
- [ ] Verificar se o botão "Entrar" continua funcional e dispara a validação ao ser clicado
- [ ] Validar se o link "Esqueci a senha" está visível e acessível na tela de login
- [ ] Validar se o link "Cadastre-se" está visível e acessível na tela de login
- [ ] Verificar se os campos de e-mail e senha possuem os atributos `autocomplete` corretos (`email` e `current-password`)

---

#### Risky (Áreas de Alto Risco)
*Mentalidade: Onde o código é complexo ou sensível a falhas colaterais?*

- [ ] Validar se e-mails com múltiplos `@` (ex.: `a@b@c.com`) são rejeitados pela regex
- [ ] Validar se e-mails com espaço no meio (ex.: `usuario @dominio.com`) são rejeitados
- [ ] Validar se e-mails sem domínio completo (ex.: `usuario@dominio`) são rejeitados pela regex (sem ponto após o `@`)
- [ ] Validar se e-mail composto apenas de espaços após `trim()` é rejeitado
- [ ] Validar se senha com exatamente 5 caracteres exibe o erro e senha com exatamente 6 não exibe
- [ ] Verificar o comportamento com senha contendo apenas espaços (ex.: `"      "` — 6 espaços)
- [ ] Validar se a submissão com ambos os campos inválidos ao mesmo tempo exibe os dois erros simultaneamente
- [ ] Verificar se a borda vermelha sobrescreve corretamente o estado de foco (`border-color: #4f46e5`) sem conflito de estilos
- [ ] Validar o comportamento de validação ao pressionar Enter com foco no campo de senha
- [ ] Verificar se e-mails com caracteres especiais válidos (ex.: `usuario+tag@dominio.com.br`) passam na regex

---

#### Configuration (Ambiente e Configurações)
*Mentalidade: Como as variáveis de ambiente ou dados de produção afetam isso?*

- [ ] Verificar o comportamento da validação e das mensagens de erro no Chrome, Firefox, Edge e Safari
- [ ] Verificar se em dispositivos móveis (iOS/Android) a exibição das mensagens inline `#emailError` e `#passwordError` não é bloqueada pelo teclado virtual
- [ ] Validar se em modo de alto contraste (acessibilidade do SO) a borda `#dc2626` permanece visível
- [ ] Verificar se com zoom do browser em 150% ou 200% os campos e mensagens de erro não se sobrepõem
- [ ] Validar se o formulário funciona corretamente com o preenchimento automático (autofill) do browser para e-mail e senha
- [ ] Verificar se a página carrega e valida corretamente sem conexão com servidor (validação é 100% client-side)

---

#### Repaired (Defeitos Corrigidos)
*Mentalidade: O que foi corrigido recentemente que pode ter gerado novos bugs?*

- [ ] Validar se a remoção da validação nativa HTML5 (`novalidate`) não criou uma janela de submissão sem validação em browsers mais antigos
- [ ] Verificar se a lógica de reset da borda (`emailInput.style.borderColor = ''`) realmente reverte ao estilo padrão do CSS (borda cinza `#d1d5db`) após correção do campo
- [ ] Validar se as mensagens de erro (`#emailError`, `#passwordError`) iniciam ocultas (`display: none`) no carregamento da página, sem piscar ao carregar
- [ ] Verificar se ao submeter o formulário válido pela segunda vez (após um erro anterior corrigido) nenhuma mensagem de erro residual permanece visível
- [ ] Validar se a validação client-side não interfere com possível validação server-side futura (dupla validação ao integrar com API)

---

#### Chronic (Problemas Crônicos)
*Mentalidade: Quais partes desses módulos sempre quebram e exigem atenção?*

- [ ] Verificar o comportamento ao colar (Ctrl+V) um e-mail com espaços no início ou fim no campo `#email`
- [ ] Verificar o comportamento ao colar uma senha com menos de 6 caracteres no campo `#password`
- [ ] Validar se clicar rapidamente várias vezes no botão "Entrar" não dispara múltiplas validações ou submissões simultâneas
- [ ] Verificar se o campo de e-mail com valor `null` ou vazio (`""`) ao submeter exibe a mensagem de erro corretamente
- [ ] Validar se o módulo "Esqueci a senha" não utiliza a mesma regex de e-mail do login de forma inconsistente (regras divergentes)
- [ ] Verificar se o módulo "Cadastro" aplica validação de e-mail equivalente, evitando que e-mails aceitos no cadastro sejam rejeitados no login
- [ ] Validar se ao navegar pelos campos com Tab a ordem de foco está correta: E-mail → Senha → Lembrar-me → Entrar
- [ ] Verificar se em navegadores com extensões de gerenciamento de senhas (ex.: LastPass, Bitwarden) a validação client-side ainda é acionada corretamente

---

## Automação Sugerida

> **Nota:** O workspace atual contém apenas `login.html` sem estrutura de automação existente. Os testes abaixo são uma **proposta adaptável** usando [Playwright](https://playwright.dev/) com JavaScript/TypeScript, adequado para testes E2E de páginas HTML estáticas. Ajuste os seletores e o runner conforme a suíte de regressão adotada pelo projeto.

### Estrutura de Arquivos Sugerida

```
tests/
  login/
    login-validation.spec.js
```

### Configuração Inicial (caso não exista)

```bash
npm init -y
npm install -D @playwright/test
npx playwright install
```

Crie o arquivo `playwright.config.js` na raiz:

```js
// playwright.config.js
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  use: {
    baseURL: 'file://' + process.cwd().replace(/\\/g, '/') + '/login.html',
    headless: true,
  },
});
```

---

### `tests/login/login-validation.spec.js`

```js
import { test, expect } from '@playwright/test';

const PAGE_URL = `file://${process.cwd().replace(/\\/g, '/')}/login.html`;

test.describe('Login – Validação Client-Side (RCRCRC)', () => {

  test.beforeEach(async ({ page }) => {
    await page.goto(PAGE_URL);
  });

  // ──────────────────────────────────────────────
  // RECENT – Mudanças Recentes
  // ──────────────────────────────────────────────

  test('[RECENT] Exibe erro de e-mail inválido (sem @) ao submeter', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'emailinvalido');
    await page.fill('#password', 'senha123');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeVisible();
    await expect(page.locator('#emailError')).toHaveText('Informe um e-mail válido.');
    await expect(page.locator('#email')).toHaveCSS('border-color', 'rgb(220, 38, 38)');
  });

  test('[RECENT] Exibe erro de senha curta (< 6 chars) ao submeter', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario@dominio.com');
    await page.fill('#password', '123');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#passwordError')).toBeVisible();
    await expect(page.locator('#passwordError')).toHaveText('A senha deve ter pelo menos 6 caracteres.');
    await expect(page.locator('#password')).toHaveCSS('border-color', 'rgb(220, 38, 38)');
  });

  test('[RECENT] Remove erro de e-mail após correção e resubmissão', async ({ page }) => {
    // Arrange – submete com e-mail inválido primeiro
    await page.fill('#email', 'invalido');
    await page.fill('#password', 'senha123');
    await page.click('button[type="submit"]');
    await expect(page.locator('#emailError')).toBeVisible();

    // Act – corrige o e-mail e resubmete
    await page.fill('#email', 'valido@dominio.com');
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeHidden();
  });

  test('[RECENT] Remove erro de senha após correção e resubmissão', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'valido@dominio.com');
    await page.fill('#password', '123');
    await page.click('button[type="submit"]');
    await expect(page.locator('#passwordError')).toBeVisible();

    // Act
    await page.fill('#password', 'senha123');
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#passwordError')).toBeHidden();
  });

  // ──────────────────────────────────────────────
  // CORE – Funcionalidades Essenciais
  // ──────────────────────────────────────────────

  test('[CORE] Login bem-sucedido com credenciais válidas', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario@dominio.com');
    await page.fill('#password', 'senha123');

    // Act
    page.on('dialog', async dialog => {
      // Assert
      expect(dialog.message()).toBe('Login realizado com sucesso!');
      await dialog.dismiss();
    });
    await page.click('button[type="submit"]');
  });

  test('[CORE] Senha com exatamente 6 caracteres não exibe erro', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario@dominio.com');
    await page.fill('#password', 'abc123');

    // Act
    page.on('dialog', async dialog => await dialog.dismiss());
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#passwordError')).toBeHidden();
  });

  test('[CORE] Mensagens de erro estão ocultas no carregamento inicial', async ({ page }) => {
    // Assert – sem nenhuma interação
    await expect(page.locator('#emailError')).toBeHidden();
    await expect(page.locator('#passwordError')).toBeHidden();
  });

  test('[CORE] Links "Esqueci a senha" e "Cadastre-se" estão visíveis', async ({ page }) => {
    await expect(page.locator('text=Esqueci a senha')).toBeVisible();
    await expect(page.locator('text=Cadastre-se')).toBeVisible();
  });

  // ──────────────────────────────────────────────
  // RISKY – Áreas de Alto Risco
  // ──────────────────────────────────────────────

  test('[RISKY] Rejeita e-mail com múltiplos @ (a@b@c.com)', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'a@b@c.com');
    await page.fill('#password', 'senha123');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeVisible();
  });

  test('[RISKY] Rejeita e-mail sem domínio com ponto (usuario@dominio)', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario@dominio');
    await page.fill('#password', 'senha123');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeVisible();
  });

  test('[RISKY] Aceita e-mail com + e subdomínio (usuario+tag@sub.dominio.com)', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario+tag@sub.dominio.com');
    await page.fill('#password', 'senha123');

    // Act
    page.on('dialog', async dialog => await dialog.dismiss());
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeHidden();
  });

  test('[RISKY] Exibe ambos os erros quando e-mail e senha são inválidos simultaneamente', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'invalido');
    await page.fill('#password', '123');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeVisible();
    await expect(page.locator('#passwordError')).toBeVisible();
  });

  test('[RISKY] Senha com exatamente 5 caracteres exibe erro', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'usuario@dominio.com');
    await page.fill('#password', '12345');

    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#passwordError')).toBeVisible();
  });

  // ──────────────────────────────────────────────
  // REPAIRED – Defeitos Corrigidos
  // ──────────────────────────────────────────────

  test('[REPAIRED] Borda do e-mail reverte ao padrão cinza após correção', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'invalido');
    await page.fill('#password', 'senha123');
    await page.click('button[type="submit"]');
    await expect(page.locator('#email')).toHaveCSS('border-color', 'rgb(220, 38, 38)');

    // Act
    await page.fill('#email', 'valido@dominio.com');
    page.on('dialog', async dialog => await dialog.dismiss());
    await page.click('button[type="submit"]');

    // Assert – borda deve ser removida (inline style limpo)
    const borderColor = await page.locator('#email').evaluate(el => el.style.borderColor);
    expect(borderColor).toBe('');
  });

  test('[REPAIRED] Segunda submissão válida não exibe erros residuais', async ({ page }) => {
    // Arrange – primeiro submit com erro
    await page.fill('#email', 'invalido');
    await page.fill('#password', '123');
    await page.click('button[type="submit"]');

    // Act – corrige e resubmete
    await page.fill('#email', 'valido@dominio.com');
    await page.fill('#password', 'senha123');
    page.on('dialog', async dialog => await dialog.dismiss());
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeHidden();
    await expect(page.locator('#passwordError')).toBeHidden();
  });

  // ──────────────────────────────────────────────
  // CHRONIC – Problemas Crônicos
  // ──────────────────────────────────────────────

  test('[CHRONIC] Rejeita e-mail com espaços colados no início e fim', async ({ page }) => {
    // Arrange – simula colar com espaços; trim() é aplicado antes da regex
    await page.fill('#email', '  usuario@dominio.com  ');
    await page.fill('#password', 'senha123');

    // Act
    page.on('dialog', async dialog => await dialog.dismiss());
    await page.click('button[type="submit"]');

    // Assert – trim remove espaços e e-mail é aceito (comportamento atual do código)
    await expect(page.locator('#emailError')).toBeHidden();
  });

  test('[CHRONIC] Campos vazios ao submeter exibem os dois erros', async ({ page }) => {
    // Act
    await page.click('button[type="submit"]');

    // Assert
    await expect(page.locator('#emailError')).toBeVisible();
    await expect(page.locator('#passwordError')).toBeVisible();
  });

  test('[CHRONIC] Múltiplos cliques rápidos no botão Entrar não duplicam erros', async ({ page }) => {
    // Arrange
    await page.fill('#email', 'invalido');
    await page.fill('#password', '123');

    // Act
    await page.click('button[type="submit"]');
    await page.click('button[type="submit"]');
    await page.click('button[type="submit"]');

    // Assert – apenas uma instância de cada mensagem deve existir no DOM
    expect(await page.locator('#emailError').count()).toBe(1);
    expect(await page.locator('#passwordError').count()).toBe(1);
  });

  test('[CHRONIC] Ordem de foco via Tab está correta: e-mail → senha → lembrar-me → botão', async ({ page }) => {
    // Arrange
    await page.locator('#email').focus();

    // Act & Assert
    await page.keyboard.press('Tab');
    await expect(page.locator('#password')).toBeFocused();

    await page.keyboard.press('Tab');
    await expect(page.locator('input[name="remember"]')).toBeFocused();

    await page.keyboard.press('Tab');
    await expect(page.locator('button[type="submit"]')).toBeFocused();
  });

});
```

---

### Instrução de Inclusão na Suíte Regressiva

1. **Adicione a spec acima** em `tests/login/login-validation.spec.js` no repositório da suíte de regressão existente.
2. **Ajuste o `baseURL`** no `playwright.config.js` para apontar para o ambiente de teste (dev/staging) quando o `login.html` for servido por um servidor HTTP em vez de `file://`.
3. **Execute apenas o módulo** de login para validar a integração antes de rodar o regressivo completo:
   ```bash
   npx playwright test tests/login/login-validation.spec.js
   ```
4. **Inclua no pipeline CI/CD** adicionando o comando acima ao step de testes E2E existente.
