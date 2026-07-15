# Desafio DevSecOps — Gerenciador de Tarefas

## Sobre o Projeto
Este repositório faz parte do desafio prático do módulo de DevSecOps da ADA Tech.
O objetivo do desafio foi implementar uma pipeline de segurança utilizando GitHub Actions, identificar vulnerabilidades propositalmente inseridas no projeto e realizar as correções necessárias para que a aplicação pudesse ser publicada de forma segura.



## Como a pipeline funciona

A pipeline foi implementada utilizando GitHub Actions e segue uma abordagem DevSecOps, incorporando verificações de segurança antes da publicação da aplicação.

O fluxo de execução é composto pelas seguintes etapas:

### 1. Checkout do Código

Nesta etapa, o código-fonte é obtido a partir do repositório GitHub para disponibilizar os arquivos que serão analisados e publicados durante a execução da pipeline.

### 2. Build

Realiza uma validação inicial da aplicação, verificando a estrutura dos arquivos e garantindo que o projeto está apto para seguir para as etapas de segurança.

### 3. Secrets Scanning com Gitleaks

O Gitleaks é utilizado para identificar segredos expostos no código-fonte, como tokens, chaves de API e senhas hardcoded.

Durante o desafio, essa etapa identificou credenciais expostas no arquivo `src/script.js`, impedindo que o deploy fosse executado até que o problema fosse corrigido. A solução foi colocar uma variável no lugar do valor hardcoded. Uma observação foi que apesar do leaks ter pego uma api_key, ele não chegou a pegar a senha que também estava hardcoded.
Essa verificação reduz o risco de exposição de credenciais em repositórios.

### 4. SAST com Semgrep

O Semgrep realiza análise estática de segurança (SAST), procurando padrões inseguros diretamente no código.

Durante a execução da pipeline foi detectado o uso da função `eval()`, considerada uma prática insegura por permitir a execução dinâmica de código (injection). Após a substituição por uma implementação segura utilizando `console.log()`, a análise passou a ser concluída sem findings bloqueantes.

Essa etapa ajuda a identificar vulnerabilidades antes da aplicação chegar ao ambiente de produção.

### 5. SCA com Grype

O Grype realiza Software Composition Analysis (SCA), analisando dependências e componentes utilizados pela aplicação em busca de vulnerabilidades conhecidas.
Durante o desafio, essa etapa não detectou vulnerabilidades.

Essa análise ajuda a reduzir riscos associados ao uso de bibliotecas de terceiros vulneráveis.

### 6. Deploy com GitHub Pages

Após a aprovação em todas as etapas anteriores, a pipeline publica automaticamente a aplicação utilizando GitHub Pages.

O deploy somente ocorre quando todas as verificações de segurança são concluídas com sucesso, garantindo que apenas versões validadas sejam disponibilizadas em produção.

### Fluxo Geral

```text
Checkout
   ↓
Build
   ↓
Gitleaks
   ↓
Semgrep
   ↓
Grype
   ↓
GitHub Pages
```
## URL de Produção
🔗 https://fernandofernandes-code.github.io/projeto-devsecop-desafio/
