---
name: gerar-checklist-regressao
description: Gera cenários de teste de regressão de QA detalhados e práticos usando estritamente a heurística RCRCRC de Karen N. Johnson. Use quando o usuário solicitar a criação de listas de verificação (checklists) de regressão, mapeamento de cenários a partir de uma alteração recente de código, análise de impactos diretos e indiretos em módulos relacionados ou produção de casos de teste prontos para automação a partir das entradas de 'alteracao' e 'modulos-relacionados'.
---

# Gerar Cenários 

Use esta skill para gerar cenários profissionais de regressão de QA a partir de uma alteração recente no sistema, utilizando estritamente as seis categorias do RCRCRC: Recent (Recente), Core (Essencial), Risky (De Risco), Configuration (Configuração), Repaired (Corrigido) e Chronic (Crônico).

## Como Invocar a Skill

A skill deve aceitar chamada com argumentos nomeados no formato abaixo:

```text
/gerar-checklist-regressao --alteracao="..." --modulos-relacionados="..."
```

Também aceite a invocação explícita por nome de skill:

```text
Use $gerar-checklist-regressao com --alteracao="..." e --modulos-relacionados="..."
```

Parâmetros esperados na invocação:

- `--alteracao`: descrição da mudança recente (correção, ajuste, diff, comportamento alterado).
- `--modulos-relacionados`: módulos/componentes impactados direta ou indiretamente.

Exemplo:

```text
/gerar-checklist-regressao --alteracao="Ajuste na regra de cálculo de coparticipação no fechamento da guia" --modulos-relacionados="autorizacao, faturamento, auditoria"
```

Exemplo com geração de automação em arquivo MD:

```text
$gerar-checklist-regressao com --alteracao="Caracteres especiais no campo de login" e --modulos-relacionados="Login, esqueci a senha, cadastro"

Após o mapeamento dos cenários de teste, crie os códigos de automação dos testes levantados, em um arquivo MD, considerando adicionarmos esses novos cenários no regressivo já automatizado.
```

Se os argumentos vierem vazios ou ausentes, solicite apenas o que faltar de forma objetiva.

## Entradas (Inputs)

Extraia estas entradas a partir da invocação da skill e/ou da solicitação do usuário:

- `alteracao`: valor de `--alteracao`; representa a mudança recente, correção de defeito, ajuste de funcionalidade ou código/diff enviado.
- `modulos-relacionados`: valor de `--modulos-relacionados`; representa a lista de módulos ou componentes afetados.
- Solicitação opcional de automação: se o usuário deseja código de automação de teste, um arquivo TXT ou integração com uma suíte de regressão existente.

Se alguma entrada estiver faltando e não puder ser inferida com segurança, faça uma pergunta concisa. Caso contrário, prossiga com premissas razoáveis e mencione-as brevemente.

## Regras da Lista de Verificação (Checklist)

Gere apenas cenários que façam sentido para a alteração fornecida e os módulos relacionados. Não invente regras de negócio, módulos, integrações ou comportamentos de produto que não tenham relação com o contexto.

Cada item da lista de verificação deve ser uma ação de teste clara, preferencialmente começando com:

- `Validar se...`
- `Verificar se...`
- `Verificar o comportamento de...`

Priorize a cobertura de regressão de alto risco e inclua os impactos diretos e indiretos da alteração.

## Formato de Saída Obrigatório

A saída padrão desta skill é sempre um **arquivo `.md`** salvo no workspace. Mesmo quando o usuário solicitar apenas os cenários ou a lista de verificação (sem código de automação), crie o arquivo `.md` contendo o checklist no formato abaixo:

Nomeie o arquivo como `checklist-regressao-<modulo>.md` (ex.: `checklist-regressao-login.md`), salvando-o na raiz do workspace ou no diretório indicado pelo usuário.

Use exatamente esta estrutura:

```markdown
###  Checklist de Regressão RCRCRC

####  Recent (Mudanças Recentes)
*Mentalidade: O que mudou e o que está no entorno direto da alteração?*
- [ ] ...

####  Core (Funcionalidades Essenciais)
*Mentalidade: O que não pode quebrar de jeito nenhum no sistema?*
- [ ] ...

#### Risky (Áreas de Alto Risco)
*Mentalidade: Onde o código é complexo ou sensível a falhas colaterais?*
- [ ] ...

####  Configuration (Ambiente e Configurações)
*Mentalidade: Como as variáveis de ambiente ou dados de produção afetam isso?*
- [ ] ...

#### Repaired (Defeitos Corrigidos)
*Mentalidade: O que foi corrigido recentemente que pode ter gerado novos bugs?*
- [ ] ...

#### Chronic (Problemas Crônicos)
*Mentalidade: Quais partes desses módulos sempre quebram e exigem atenção?*
- [ ] ...
```

Use caixas de seleção `[ ]` para cada item de teste.

## Diretrizes do RCRCRC

Aplique as categorias da seguinte forma:

- **Recent**: Valide a alteração exata do código e o comportamento imediatamente ao redor.
- **Core**: Proteja os fluxos principais (happy paths) e as validações obrigatórias dos módulos relacionados.
- **Risky**: Explore casos de borda (edge cases), dados inválidos, conversão de tipos (parsing), limites de validação, manipulação sensível à segurança e fluxos frágeis impactados pela mudança.
- **Configuration**: Cubra diferenças relevantes de ambiente, dados, permissões, navegador/dispositivo, feature flags ou estados de conta que possam afetar o mesmo módulo.
- **Repaired**: Teste novamente a correção em si e confirme se o ajuste não enfraqueceu as validações existentes ou o tratamento de erros.
- **Chronic**: Cubra padrões historicamente frágeis para aquele módulo, tais como entradas vazias, copiar/colar, remoção de espaços (trimming), tentativas repetidas, tempo de validação e comportamento inconsistente da interface (UI), quando aplicável.

## Código de Automação

Quando o usuário solicitar código de automação:

1. Inspecione o projeto atual antes de criar arquivos, sempre que houver um workspace disponível.
2. Identifique a estrutura de automação existente, estilo, framework de testes, nomenclatura e organização das suítes.
3. Se não houver uma implementação correspondente para o módulo solicitado, crie a automação como uma proposta adaptável e identifique claramente os nomes de classes/serviços que precisam ser ajustados.
4. Mantenha os testes gerados alinhados à lista de verificação RCRCRC.
5. Escreva os testes seguindo o padrão AAA (Arrange, Act, Assert), deixando explícita a preparação do cenário, a ação executada e a validação esperada.
6. Crie sempre um arquivo `.md` (independentemente de o usuário solicitar explicitamente ou não), contendo:
   - a lista de verificação no formato obrigatório;
   - uma seção de `Automação sugerida`;
   - código compatível com a estrutura de testes existente (se identificável);
   - orientações de registro da suíte, quando aplicável.
7. Quando o pedido mencionar adicionar os novos cenários ao regressivo já automatizado, trate como solicitação explícita para gerar o arquivo `.md` com checklist, código de automação sugerido e instrução de inclusão na suíte regressiva existente.

Não crie código de produção, a menos que o usuário solicite explicitamente.

## Critérios de Qualidade

Antes de finalizar:

- Confirme se todas as seis categorias do RCRCRC estão presentes e na ordem correta.
- Confirme se cada linha da lista de verificação é prática e executável.
- Confirme se os cenários permanecem estritamente dentro do escopo de `alteracao` e `modulos-relacionados`.
- Confirme se os exemplos de automação correspondem à estrutura do repositório ou se estão claramente identificados como propostas adaptáveis.