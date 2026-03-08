# DESAFIO QA - BEEDOO 2026

## Informações do Candidato

**Nome:** Felipe Castro  
**Data de Realização:** 06/03/2026  
**Repositório:** [DESAFIO-QA-BEEDOO-2026](https://github.com/felipetster/DESAFIO-QA-BEEDOO-2026)  
**Aplicação Testada:** [Beedoo QA Challenge](https://creative-sherbet-a51eac.netlify.app/)

---

##  Sumário

1. [Análise da Aplicação](#-análise-da-aplicação)
2. [Estratégia de Testes](#-estratégia-de-testes)
3. [Casos de Teste](#-casos-de-teste)
4. [Bugs Encontrados](#-bugs-encontrados)
5. [Evidências](#-evidências)
6. [Conclusão](#-conclusão)

---

## Recursos Principais

- **Planilha de Casos de Teste:** [https://docs.google.com/spreadsheets/d/16erjL92i2tdmKGA22FcGsY0swQR2CxbEHv-HJGO0SyY/edit?usp=sharing]
- **Evidências (Drive):** [https://drive.google.com/drive/folders/1-pdh3Yrm_jhBf8aPC1r1A5zpfBYMkseo?usp=sharing]

---
## Análise da Aplicação

### Objetivo da Aplicação

A aplicação **Beedoo QA Challenge** é um sistema de gerenciamento de cursos que permite cadastrar, listar e excluir cursos educacionais. O sistema oferece suporte para dois formatos de curso: **Online** (com campo de link) e **Presencial** (com campo de endereço), adaptando dinamicamente os campos obrigatórios conforme a modalidade selecionada.

### Principais Fluxos Identificados

#### 1. **Cadastro de Curso**
- Usuário acessa página de cadastro
- Preenche informações do curso (nome, descrição, instrutor, datas, vagas)
- Seleciona modalidade (Online/Presencial)
- Sistema exibe campos condicionais específicos
- Salva curso e redireciona para listagem

#### 2. **Listagem de Cursos**
- Sistema exibe cards com cursos cadastrados
- Apresenta informações principais: nome, descrição, datas, vagas, imagem
- Permite exclusão de cursos

#### 3. **Exclusão de Curso**
- Usuário clica em botão "Excluir" no card
- Sistema deve remover curso do banco de dados
- Interface deve atualizar listagem em tempo real

#### 4. **Navegação**
- Header fixo com logo e botões de navegação
- Alternância entre telas de cadastro e listagem

### Pontos Críticos Identificados

Com base na análise exploratória, identifiquei os seguintes pontos críticos que requerem atenção especial durante os testes:

#### **Criticidade ALTA**

1. **Validações de Campos Obrigatórios**
   - **Por quê:** Ausência de validação pode permitir registros vazios/inválidos no banco de dados
   - **Impacto:** Poluição da base de dados, inconsistências de negócio, má UX

2. **Consistência Temporal (Datas)**
   - **Por quê:** Datas inconsistentes impossibilitam cálculo de duração e gerenciamento de cronograma
   - **Impacto:** Dados ilógicos, impossibilidade de gerar relatórios confiáveis

3. **Campos Condicionais (Online vs Presencial)**
   - **Por quê:** Lógica condicional complexa é propensa a erros de implementação
   - **Impacto:** Cursos cadastrados sem informações essenciais (endereço ou link)

4. **Exclusão de Registros**
   - **Por quê:** Operação destrutiva que afeta integridade dos dados
   - **Impacto:** Perda não intencional de dados, falsos positivos confundem usuário

#### **Criticidade MÉDIA**

5. **Validações de Tipo de Dado**
   - **Por quê:** Campos aceitando tipos incorretos geram dados inconsistentes
   - **Impacto:** Dificuldade de processamento, relatórios quebrados

6. **Validações de Formato (URLs)**
   - **Por quê:** URLs inválidas quebram renderização de imagens
   - **Impacto:** Cards sem imagem, má experiência visual

7. **Prevenção de Duplicação**
   - **Por quê:** Múltiplos cursos com mesmo nome geram ambiguidade
   - **Impacto:** Confusão de usuários, dificuldade de gestão

#### **Criticidade BAIXA**

8. **UX e Navegação**
   - **Por quê:** Padrões de UX melhoram usabilidade mas não afetam funcionalidade core
   - **Impacto:** Menor produtividade, frustração de usuário

---

## Estratégia de Testes

### Metodologia Adotada

Utilizei abordagem **BDD (Behavior-Driven Development)** escrevendo todos os casos de teste no formato **Gherkin** (DADO/QUANDO/ENTÃO) para:

- Facilitar compreensão por stakeholders não-técnicos
- Alinhar testes com regras de negócio
- Preparar base para futura automação
- Garantir clareza e objetividade

### Categorização dos Testes

Os testes foram organizados em **4 módulos** de acordo com a área de cobertura:
```
📦 MÓDULOS DE TESTE
├── 📂 Cadastro (11 casos)
│   ├── Cenários positivos (happy path)
│   ├── Validações de campo (tipo, formato, limites)
│   ├── Regras de negócio (datas, duplicação)
│   └── Campos condicionais (Online/Presencial)
│
├── 📂 Listagem (5 casos)
│   ├── Exibição de dados
│   ├── Exclusão de cursos
│   ├── Estados vazios
│   └── Ordenação
│
├── 📂 Segurança (3 casos)
│   ├── Proteção XSS (Cross-Site Scripting)
│   ├── Proteção SQL Injection
│   └── Validação de tamanho de entrada
│
└── 📂 UX (3 casos)
    ├── Navegação
    ├── Responsividade mobile
    └── Prevenção de perda de dados
```

### Decisões de Cobertura

#### **Por que 11 casos de Cadastro?**

O módulo de cadastro é o **core** da aplicação e apresenta:
- 8 campos de entrada (cada um com validações específicas)
- 2 modalidades diferentes (Online/Presencial)
- Múltiplas regras de negócio (obrigatoriedade, formatos, limites)

Dessa forma, 11 casos garantem:
- Cenário positivo completo
- Validação individual de cada campo crítico
- Testes de campos condicionais
- Edge cases (duplicação, valores limite)

#### **Por que incluir testes de Segurança?**

Apesar de não serem explicitamente solicitados, testes de segurança são **essenciais** porque:
- Aplicação web recebe input de usuário (vetor de ataque)
- XSS e SQL Injection são vulnerabilidades comuns em CRUD
- Protege dados e reputação da empresa

#### **Por que apenas 3 casos de UX?**

Priorizei funcionalidades core primeiro. Os 3 casos de UX cobrem:
- Padrões estabelecidos da web (logo clicável)
- Responsividade (requisito moderno)
- Prevenção de frustração (perda de progresso)

### Raciocínio Durante a Análise

#### **Fase 1: Exploração Manual**
- Navegação livre pela aplicação
- Identificação de todos campos e botões
- Teste rápido do "happy path"
- Anotação de comportamentos inesperados

#### **Fase 2: Mapeamento de Funcionalidades**
- Listagem de todos requisitos funcionais
- Identificação de campos condicionais
- Desenho mental dos fluxos possíveis
- Priorização por criticidade

#### **Fase 3: Modelagem de Cenários**
- Criação de casos positivos (1 por fluxo)
- Criação de casos negativos (validações)
- Criação de edge cases (limites, duplicações)
- Criação de casos de segurança

#### **Fase 4: Execução Sistemática**
- Execução de todos 22 casos de teste
- Captura de evidências (prints + vídeos)
- Documentação de bugs encontrados
- Análise de causa raiz

---

## Casos de Teste

### Resumo Quantitativo
```
╔═══════════════════════════════════════════════════════════╗
║          RESUMO EXECUTIVO - ANÁLISE DE QUALIDADE          ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║  Total de Casos de Teste:           22                   ║
║  Casos Executados:                  22 (100%)            ║
║  Casos Aprovados:                   8 (36%)              ║
║  Casos Reprovados (Bugs):           14 (64%)             ║
║                                                           ║
║  Bugs de Severidade Alta:           5                    ║
║  Bugs de Severidade Média:          6                    ║
║  Bugs de Severidade Baixa:          3                    ║
║                                                           ║
║  Taxa de Defeitos:                  64%                  ║
║  Cobertura de Requisitos:           100%                 ║
║                                                           ║
║  CONCLUSÃO:                                               ║
║  Sistema seguro contra Injeções (XSS/SQL), porém possui  ║
║  falhas de nível Alto no CRUD (Falso positivo e campos). ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

### Documentação Completa

**Planilha de Casos de Teste:** [Google Sheets - Beedoo QA Tests](https://docs.google.com/spreadsheets/d/16erjL92i2tdmKGA22FcGsY0swQR2CxbEHv-HJGO0SyY/edit?usp=sharing)

**Estrutura da planilha:**
- **Aba 1 - Dashboard:** Resumo executivo com métricas
- **Aba 2 - Cadastro:** 11 casos de teste do módulo de cadastro
- **Aba 3 - Lista:** 5 casos de teste do módulo de listagem
- **Aba 4 - Segurança:** 3 casos de teste de vulnerabilidades
- **Aba 5 - UX:** 3 casos de teste de usabilidade

**Informações incluídas em cada caso:**
- Test Case ID
- Cenário em Gherkin (DADO/QUANDO/ENTÃO)
- Pré-condição
- Severidade (Alta/Média/Baixa)
- Passos para reproduzir (detalhados)
- Test Data
- Ambiente (Chrome v145.0.7632.160 / Windows 11)
- Resultado Esperado
- Resultado Real
- Status (Passou/Falhou)
- Título do Bug (quando aplicável)
- Link para Evidência

---

## Bugs Encontrados

### Resumo por Severidade

| Severidade | Quantidade | Percentual |
|------------|------------|------------|
| 🔴 Alta    | 5          | 36%        |
| 🟡 Média   | 6          | 43%        |
| 🟢 Baixa   | 3          | 21%        |
| **TOTAL**  | **14**     | **100%**   |

---

### Principais Bugs Encontrados (Top 3)

Abaixo estão os três principais defeitos críticos identificados, formatados para abertura de tickets. *A lista completa com os 14 bugs e suas respectivas evidências encontra-se na Planilha de Casos de Teste.*

#### 1. [Listagem] Falso Positivo na exclusão de cursos
* **Passos para reproduzir:** 1. Acessar a página de Listagem de Cursos.
  2. Clicar no botão "Excluir Curso" em qualquer card existente.
  3. Atualizar a página (F5).
* **Resultado atual:** O sistema exibe um alerta de sucesso ("Curso excluído"), mas o curso continua existindo na listagem e no banco de dados.
* **Resultado esperado:** O curso deve ser permanentemente removido do banco de dados e desaparecer da interface.
* **Severidade:** Alta

#### 2. [Segurança] Ausência de limite de caracteres (Maxlength) quebra a interface
* **Passos para reproduzir:** 1. Acessar a página de Cadastrar Curso.
  2. Inserir uma string massiva (ex: 5000 caracteres) no campo "Nome do curso".
  3. Clicar em Salvar e acessar a Listagem.
* **Resultado atual:** O sistema aceita a carga massiva sem bloqueio. Na listagem, o texto gigante vaza do card e quebra o layout da tela.
* **Resultado esperado:** O campo "Nome do curso" deve ter uma trava de limite (ex: `maxlength="100"`).
* **Severidade:** Alta

#### 3. [Cadastro] Validação numérica ausente permite cursos com 0 vagas
* **Passos para reproduzir:** 1. Acessar a página de Cadastrar Curso.
  2. Preencher os dados e inserir `0` (zero) no campo "Vagas".
  3. Clicar em Salvar.
* **Resultado atual:** O sistema aceita o valor inválido e conclui o cadastro silenciosamente.
* **Resultado esperado:** O sistema deve bloquear a submissão e exibir uma mensagem de erro exigindo no mínimo 1 vaga.
* **Severidade:** Média


---

## Evidências

### Organização das Evidências

Todas as evidências foram organizadas em **Google Drive** com a seguinte estrutura:
```
📁 Beedoo QA - Evidências/
├── 📁 Cadastro/
│   ├── 🎬 BUG-001_Cadastro_em_Branco.mp4
│   ├── 🎬 BUG-002_Datas_Inconsistentes.mp4
│   ├── 🎬 BUG_CAD-003_Instrutor_Aceita_Numeros.mp4
│   ├── 🎬 BUG_CAD-004_URL_Formato_Invalido.mp4
│   ├── 🎬 BUG_CAD-005_Vagas_Negativas.mp4
│   ├── 🎬 BUG_CAD-006_Aceita_Zero_Vagas.mp4
│   ├── 🎬 BUG_CAD-007_Cadastro_Duplicado.mp4
│   ├── 🎬 BUG_CAD-008_Endereco_Nao_Obrigatorio.mp4
│   ├── 📸 TC-CAD-001_Passou.png
│   ├── 📸 TC-CAD-009_Passou.png
│   └── 📸 TC-CAD-010_Passou.png
│
├── 📁 Listagem/
│   ├── 🎬 BUG_LIST-001_Falha_Exclusao.mp4
│   ├── 🎬 BUG_LIST-003_Omissao_de_Dados.mp4
│   ├── 🎬 TC-LIST-004_Passou.mp4
│   ├── 📸 BUG_LIST-002_Falta_Empty_State.png
│   └── 📸 TC-LIST-003_Passou.png
│
├── 📁 Segurança/
│   ├── 🎬 BUG_SEC-001_Estouro_Limite.mp4
│   ├── 🎬 TC-SEC-001_Passou.mp4
│   └── 🎬 TC-SEC-002_Passou.mp4
│
└── 📁 UX/
    ├── 🎬 BUG_UX-003_Perda_Progresso.mp4
    ├── 🎬 TC-UX-002_Passou.mp4
    └── 📸 BUG_UX-001_Logo_Nao_Redireciona.png
```

**Acesso às Evidências:** [Google Drive - Evidências Beedoo QA](https://drive.google.com/drive/folders/1-pdh3Yrm_jhBf8aPC1r1A5zpfBYMkseo?usp=sharing)

### Tipos de Evidência

- **📸 Screenshots:** Para bugs visuais e testes que passaram
- **🎬 Vídeos:** Para fluxos completos e bugs comportamentais

---

## Conclusão

### Visão Geral da Qualidade

A aplicação apresenta **taxa de defeitos de 64%** (14 bugs em 22 casos de teste), indicando que o sistema necessita de **correções significativas antes de produção**.

#### **Pontos Positivos**

1. **Segurança Robusta:**
   - Proteção contra XSS implementada
   - Proteção contra SQL Injection funcionando
   - Sanitização de inputs presente

2. **Funcionalidades Dinâmicas:**
   - Campos condicionais (Online/Presencial) funcionam corretamente
   - Interface responde adequadamente à seleção de modalidade

3. **Responsividade:**
   - Layout adaptável para dispositivos móveis

#### **Pontos Críticos**

1. **Ausência Completa de Validações:**
   - Nenhum campo possui validação de obrigatoriedade
   - Nenhum campo valida tipo de dado
   - Nenhum campo valida formato

2. **Falso Positivo na Exclusão:**
   - Funcionalidade core completamente quebrada
   - Usuário recebe feedback incorreto

3. **Omissão de Dados:**
   - Informações cadastradas não são exibidas
   - Inconsistência entre cadastro e visualização

### Recomendações de Priorização

#### **BLOCKER (Corrigir ANTES de produção):**

1. BUG-001 - Validação de campos obrigatórios
2. BUG-002 - Validação de datas
3. BUG-CAD-008 - Validação de campos condicionais
4. BUG-LIST-001 - Correção da exclusão
5. BUG-LIST-003 - Exibição do instrutor

#### **ALTA (Corrigir em sprint atual):**

- Validações de tipo (instrutor, URL, vagas)
- Prevenção de duplicação
- Empty state na listagem

#### **MÉDIA (Backlog próximas sprints):**

- UX de navegação
- Alertas de perda de progresso
- Limites de caracteres

---

## Uso de Inteligência Artificial

Durante este desafio, utilizei Claude como ferramenta de apoio:

   - Solicitei sugestões de formato de README
   - Pedi exemplos de boas práticas de documentação de testes
   - Brainstorming de nomenclaturas técnicas para o reporte de bugs.
   - Refinamento da sintaxe estrutural (BDD) para maior clareza.

A modelagem exploratória, captura de evidências e análise de riscos foram 100% humanas. Durante o uso da IA, apliquei forte senso crítico para avaliar e refutar sugestões que divergiam da realidade. Por exemplo: corrigi a IA quando ela interpretou erroneamente que a renderização do *Card* havia falhado por falta de dados (quando, na verdade, os dados estavam corretos) e atestei os testes de injeção (XSS/SQL) como "Passou", garantindo que o veredito final fosse exclusivamente embasado nas evidências coletadas por mim.



## Sobre o Candidato

**Felipe Castro**  
QA Analyst | Test Automation | Cypress | BDD

Profissional de QA com experiência em análise manual, automação de testes e documentação técnica.

**Contato:**  
[felipe.c.lima1604@gmail.com]  
[LinkedIn](https://www.linkedin.com/in/felipetster/)  
[GitHub](https://github.com/felipetster)
