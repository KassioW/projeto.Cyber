# 🛡️ Painel de Monitoramento Seguro (Web Dashboard)

**Projeto Aplicado: Práticas de Mercado**  
*Pós-graduação em Segurança da Informação e Análise Forense*

## 📌 Visão Geral do Projeto
Este repositório contém o protótipo de um Dashboard de Monitoramento de Ativos de TI (Mock Data). O sistema foi desenvolvido com foco no princípio de *Secure by Design*, implementando controles de segurança desde a concepção do código até a sua implantação em infraestrutura Cloud. 

O projeto é composto por uma interface Front-end estática (HTML, CSS e Vanilla JavaScript) dividida em duas telas principais:
1. **Login (`index.html`):** Autenticação segura de usuários.
2. **Página Interna (`dashboard.html`):** Tabela de status de equipamentos, restrita a usuários autenticados, com funcionalidade de *Logout*.

As ações de desenvolvimento, estruturação e auditoria de código contaram com o suporte de inteligência artificial (Google Antigravity / IA Assistente).

---

## 🔒 Mitigação de Vulnerabilidades (OWASP Top 10)

Em conformidade com os requisitos de segurança, o código-fonte deste projeto mitiga ativamente 3 (três) vulnerabilidades baseadas nas diretrizes da OWASP Top 10. Abaixo está a documentação técnica das defesas implementadas:

### 1. Broken Access Control (Falha de Controle de Acesso) - OWASP A01
* **Onde foi mitigado:** Arquivo `dashboard.html`, nas primeiras linhas da tag `<script>`.
* **Como previne:** O código implementa uma verificação de sessão (`sessionStorage.getItem('auth_token')`) antes de renderizar qualquer conteúdo sensível. Se um usuário mal-intencionado tentar acessar a URL da página interna diretamente sem ter passado pelo processo de login, o script bloqueia o acesso e força um redirecionamento imediato para a página `index.html`. 

### 2. Identification and Authentication Failures (Falhas de Autenticação) - OWASP A07
* **Onde foi mitigado:** Arquivo `index.html`, nas funções `gerarHash()` e `realizarLogin()`.
* **Como previne:** Para evitar a exposição de credenciais, o sistema não armazena e não compara a senha em texto claro. O input digitado pelo usuário na tela de login é submetido à API nativa `crypto.subtle.digest`, que gera um hash criptográfico **SHA-256**. A autenticação só ocorre se o hash gerado coincidir com o hash esperado pelo sistema. Isso simula a prática de mercado de nunca transacionar senhas em texto puro.

### 3. Injection / Cross-Site Scripting (XSS) - OWASP A03
* **Onde foi mitigado:** Arquivo `dashboard.html`, dentro do laço de repetição `equipamentos.forEach` que renderiza a tabela.
* **Como previne:** A injeção de dados dinâmicos na interface (construção do DOM) é feita exclusivamente utilizando a propriedade `textContent` na criação das células da tabela (`tdNome.textContent = eq.nome`). O uso do `textContent` em detrimento do `innerHTML` garante que os dados processados sejam interpretados estritamente como texto puro, neutralizando qualquer tentativa de execução de scripts nocivos (XSS) ou tags HTML maliciosas injetadas através de manipulação de dados.

---

## ☁️ Infraestrutura e Implantação
Este sistema é implantado de forma contínua através de uma esteira CI/CD utilizando **GitHub Actions**. A aplicação é hospedada em uma infraestrutura Cloud pública rodando Ubuntu Server, protegida por configurações de Least Privilege (Firewall/Security Groups), restrição de acesso administrativo via chaves SSH, e tráfego 100% criptografado através de um Web Server rodando HTTPS.