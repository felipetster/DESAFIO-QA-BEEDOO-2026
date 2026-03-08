# 🐝 Beedoo QA Challenge - Testes Manuais e Análise de Qualidade

Este repositório contém a documentação e execução do desafio técnico de Quality Assurance (QA) para a plataforma de gestão de cursos da Beedoo. O objetivo principal foi mapear, modelar e executar casos de teste para garantir a integridade, usabilidade e segurança da aplicação.

## 📂 Entregáveis Principais

* 📊 **[Planilha de Casos de Teste e Bugs (Google Sheets)](https://docs.google.com/spreadsheets/d/16erjL92i2tdmKGA22FcGsY0swQR2CxbEHv-HJGO0SyY/edit?usp=sharing)**
* 📁 **[Pasta de Evidências (Google Drive)](https://drive.google.com/drive/folders/1-pdh3Yrm_jhBf8aPC1r1A5zpfBYMkseo?usp=sharing)**

---

## 1. Análise Inicial e Exploração da Aplicação

**Qual o objetivo da aplicação?**
Acredito que o objetivo principal seja atuar como um módulo administrativo (CRUD) de uma plataforma de capacitação corporativa. O sistema visa permitir que gestores ou instrutores cadastrem novos conteúdos educacionais e acompanhem a listagem dos cursos disponíveis para os usuários (possivelmente alimentando funcionalidades futuras como a *Bee AI*).

**Quais são os principais fluxos disponíveis?**
1. **Fluxo de Cadastro (Escrita):** Preenchimento de formulário com dados de texto, URL, números (vagas) e datas, incluindo lógicas condicionais (ex: Endereço visível apenas para cursos presenciais).
2. **Fluxo de Listagem (Leitura e Exclusão):** Renderização em cards dos dados salvos no banco, com a opção de exclusão (Delete) do registro.

**Quais pontos do sistema considero mais críticos para teste?**
A integridade dos dados (salvar exatamente o que foi digitado sem corromper), as validações de limites (datas conflitantes, números negativos) e a operação de exclusão. Se o CRUD básico estiver quebrado, todo o ecossistema que consumir esses dados (como o *Beebot Navigator*) apresentará falhas.

---

## 2. Raciocínio e Decisões Tomadas na Criação dos Testes

Para a modelagem, optei por uma abordagem baseada em risco e particionamento de equivalência, dividindo a suíte em 4 pilares:
1. **Cadastro (CAD):** Foco em validação de campos obrigatórios, tipos de input e regras de negócio lógicas (Ex: Data de Fim maior que Data de Início).
2. **Listagem (LIST):** Foco em integridade visual, *Empty States* e validação de operações (evitar falsos positivos).
3. **Segurança (SEC):** Testes de estresse de inputs (XSS, SQL Injection e Maxlength) para garantir sanitização básica.
4. **User Experience (UX):** Prevenção de perda de dados (Loss Aversion) e fluxos de navegação.

Os casos de teste foram escritos utilizando a sintaxe Híbrida (**BDD/Gherkin** misturado com passos estruturados), garantindo que o comportamento esperado ficasse claramente separado da massa de dados (*Test Data*).

---

## 3. Registro de Bugs (Top 3 Defeitos Críticos)

Abaixo estão detalhados os principais defeitos encontrados, formatados para abertura imediata de tickets. *A lista completa de bugs (14 no total) encontra-se na Planilha do Google Sheets.*

### Bug 1: [Listagem] Falso Positivo na exclusão de cursos
* **Passos para reproduzir:** 1. Acessar Listagem. 2. Clicar em "Excluir Curso". 3. Atualizar a página (F5).
* **Resultado atual:** O sistema exibe um alerta de sucesso ("Curso excluído"), mas o curso continua existindo na listagem e no banco de dados.
* **Resultado esperado:** O curso deve ser permanentemente removido e desaparecer da interface.
* **Severidade:** Alta

### Bug 2: [Segurança] Ausência de limite de caracteres (Maxlength) quebra a interface
* **Passos para reproduzir:** 1. Acessar Cadastro. 2. Inserir ~5000 caracteres no "Nome do curso". 3. Salvar e acessar a Listagem.
* **Resultado atual:** O sistema aceita a carga massiva. Na listagem, o texto gigante vaza do card e quebra o layout da tela.
* **Resultado esperado:** O campo deve ter uma trava de limite (ex: `maxlength="100"`).
* **Severidade:** Alta

### Bug 3: [Cadastro] Validação numérica ausente permite cursos com 0 vagas
* **Passos para reproduzir:** 1. Acessar Cadastro. 2. Preencher os dados e inserir `0` no campo "Vagas". 3. Salvar.
* **Resultado atual:** O sistema aceita o valor inválido e conclui o cadastro silenciosamente.
* **Resultado esperado:** Bloquear a submissão e exibir erro exigindo no mínimo 1 vaga.
* **Severidade:** Média

---

## 4. Transparência: Uso de Inteligência Artificial

Em alinhamento com as diretrizes do processo seletivo, utilizei IA generativa como apoio estratégico (um *"pair QA"*), aplicada estritamente para:
* Refinamento da sintaxe estrutural (BDD) para maior clareza.
* Formatação de artefatos visuais (Dashboard Executivo na planilha).
* Brainstorming de nomenclaturas técnicas para o reporte de bugs.

**Avaliação e Senso Crítico:**
A modelagem exploratória, captura de evidências e análise de riscos foram 100% humanas. Durante o uso da IA, apliquei forte senso crítico para avaliar e refutar sugestões que divergiam da realidade. Por exemplo: corrigi a IA quando ela interpretou erroneamente que a renderização do *Card* havia falhado por falta de dados (quando, na verdade, os dados estavam corretos) e atestei os testes de injeção (XSS/SQL) como "Passou", garantindo que o veredito final fosse exclusivamente embasado nas evidências coletadas por mim.
