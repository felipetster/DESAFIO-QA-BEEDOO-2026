# Beedoo QA Challenge - Testes Manuais e Análise de Qualidade

Este repositório contém a documentação e a execução do desafio técnico de Quality Assurance (QA) para a plataforma de gestão de cursos da Beedoo. O objetivo principal foi mapear, modelar e executar casos de teste para garantir a integridade, usabilidade e segurança da aplicação.

Garantir uma experiência fluida e sem atritos no fluxo base (CRUD) é o primeiro passo para manter os colaboradores engajados e verdadeiramente abeedoozidos pela plataforma.

## Entregáveis Principais

Para visualizar a modelagem completa dos testes, o dashboard executivo e as evidências (vídeos e capturas de tela), acesse os links abaixo:

* **[Planilha de Casos de Teste e Bugs (Google Sheets)](https://docs.google.com/spreadsheets/d/16erjL92i2tdmKGA22FcGsY0swQR2CxbEHv-HJGO0SyY/edit?usp=sharing)**
* **[Pasta de Evidências (Google Drive)](https://drive.google.com/drive/folders/1-pdh3Yrm_jhBf8aPC1r1A5zpfBYMkseo?usp=sharing)**

---

## Estratégia de Testes

A abordagem de testes utilizou a escrita Híbrida (BDD + Data-Driven) e foi dividida em quatro suítes principais para garantir uma cobertura abrangente:

1. **Cadastro de Cursos (CAD):** Validação de regras de negócio, obrigatoriedade de campos, limites e comportamentos condicionais.
2. **Listagem de Cursos (LIST):** Verificação de integridade dos dados renderizados, estados da tela (Empty States) e validação de operações CRUD.
3. **Segurança (SEC):** Testes focados em identificar vulnerabilidades de injeção (XSS, SQL Injection) e falhas de sanitização.
4. **User Experience (UX):** Validação de responsividade mobile, fluxo de navegação e prevenção de perda de dados.

---

## Resumo Executivo e Análise Crítica

Durante a execução dos 22 casos de teste projetados, a aplicação demonstrou uma base sólida em requisitos de segurança contra injeções. Os testes de Cross-Site Scripting (XSS) e SQL Injection passaram com sucesso.

No entanto, a taxa de defeitos concentrou-se fortemente na ausência de validação de regras de negócio e em falhas de feedback ao usuário, totalizando 14 anomalias mapeadas. 

A integridade do módulo de cadastro e listagem é uma prioridade técnica. Para que funcionalidades avançadas do ecossistema funcionem com excelência no futuro — seja a recomendação individualizada de trilhas da Bee AI ou as consultas gerativas do Beebot Navigator —, os dados estruturais dos cursos precisam ser absolutamente consistentes na origem.

### Top 3 Defeitos Críticos (Prioridade de Correção)

* **[Listagem] Falso Positivo na exclusão:** O sistema exibe uma notificação de sucesso ao excluir um curso, porém o registro não é removido do banco de dados e permanece na listagem.
* **[Segurança] Ausência de limite de caracteres (Maxlength):** O formulário permite a inserção de payloads massivos de texto. Ao renderizar esses dados na listagem, ocorre a quebra e sobreposição do layout.
* **[Cadastro] Falhas de Validação Numérica e Temporal:** O sistema permite a submissão de cursos com 0 vagas, capacidade negativa e datas logicamente impossíveis (Data de Fim anterior à Data de Início).

---

## Ferramentas Utilizadas

* **Gestão de Testes e Dashboard:** Google Sheets
* **Captura de Evidências:** Ferramentas nativas de SO
* **Inspecionamento e Responsividade:** Google Chrome DevTools
* **Design de Casos de Teste:** BDD (Gherkin)

## Uso de Inteligência Artificial

Em alinhamento com as diretrizes do processo seletivo, ferramentas de Inteligência Artificial generativa foram utilizadas como apoio estratégico durante a documentação deste desafio. A IA atuou como um facilitador, sendo aplicada estritamente para:

* **Otimização de Nomenclaturas:** Refinamento da sintaxe estrutural (Gherkin/BDD) para garantir maior clareza e padronização de mercado na escrita dos Casos de Teste.
* **Formatação de Artefatos:** Auxílio na geração de estruturas visuais, como o dashboard executivo em formato de texto e a formatação base deste documento Markdown.
* **Refinamento de Títulos:** Apoio no brainstorming de títulos objetivos e técnicos para o reporte de bugs.

**Avaliação e Senso Crítico:**
A modelagem dos cenários, a execução exploratória, a captura de evidências e a análise de riscos foram processos integralmente humanos. Durante a utilização da ferramenta, apliquei o senso crítico para avaliar, refutar e adaptar as sugestões da IA sempre que divergiam do comportamento real da aplicação — como, por exemplo, ao corrigir interpretações automáticas equivocadas sobre falsos positivos de renderização de dados na listagem e ao validar o sucesso dos testes de injeção de scripts (XSS/SQL), garantindo que a tomada de decisão técnica e o veredito final fossem 100% embasados nas evidências coletadas por mim.
