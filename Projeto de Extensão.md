# Assuntos que vamos abordar

Primeiro preciso concordar com minha equipe se um desses conteudos é interessente de se analisar

#### Educação e Desigualdade Social (Microdados do ENEM 2025)

**Fonte:** Dados Abertos (INEP) ou repositórios já limpos no Kaggle.

- **Ideias de Análise:**
  - **Impacto da Renda:** Cruzar a renda familiar (questionário socioeconômico) com o desempenho em Matemática e Redação (Matplotlib: Gráficos de dispersão ou boxplots mostrando a distribuição das notas por faixa de renda).
  - **Escola Pública vs. Privada:** Analisar a discrepância das notas ao longo dos anos, verificando se a pandemia (2020-2022) aumentou essa diferença.
  - **Perfil de Abstenção:** Descobrir qual o perfil demográfico (cor/raça, região, renda) que mais falta no dia da prova.

---

[ Gabriel]

1. **Raça/cor x tipo de escola**: `TP_COR_RACA` x `Q023`, mostra se autodeclarados pretos/pardos estão mais concentrados em escola pública [ Feito ]

2. **A Brecha Digital**: `Q020` (quem tem ou não tem acesso a internet) [ Feito por partes]

3. Municípios / UF que se candidataram a realizar o exame [ Feito ]

4. **Escolaridade dos pais x tipo de escola do filho**: `Q001`/`Q002` x `Q023`, será que filho de pai/mãe com mais estudo tende mais pra escola particular [ Feito ]

5. **Situação de conclusão do ensino médio**: `TP_ST_CONCLUSAO`, quantos já concluíram vs estão cursando vs vão concluir esse ano, dá pra ver o perfil de quem tenta o ENEM [ Feito ]

[Vinicius ]

1. **O Abismo Público vs. Privado (escola publica x Escola privada)**: [ Em Andamento ]

2. **O Perfil da Abstenção (Quem se inscreve e não faz a prova)**: [ Feito ]

3. **Perfil Etário dos Canditatos**: [ Feito ]

4. **Renda Familiar e Desigualdade**: [ Feito ]

5. **Perfil etário dos candidatos**: Investigar a possibildiade de relaciona presença com renda, raça e tipo de escola. [ Feito ]

**_Oq vamos fazer quando o CSV baixar (bgl grande)_**

1. Ler o csv
2. Remover os treineiros
3. Remover quem faltou em todas as provas
4. Fazer agrupamentos por estado, renda, raça e escola
5. Salvar e fazer uma analise no PowerBI (talvez)

---

### Coisas para perguntar pro prfoessor:

- Acha prudente eu mudar os valores que estão numericos no DF, como o de TP_ESTADO_CIVIL:
  ![[Pasted image 20260902195838.png]]
  ```python
  mapa_estado_civil = {
  	0: 'Solteiro(a)',
  	1: 'Casado(a)/Mora com companheiro(a)',
  	2: 'Divorciado(a)/Separado(a)',
  	3: 'Viúvo(a)'
  }
  ```

- É adequado trabalhar com DataFrames separados? Usamos df para PARTICIPANTES_2025 e df_resultados para RESULTADOS_2025, porque as bases possuem informações diferentes.

- Qual seria a melhor forma de otimizar o carregamento e processamento de diferentes arquivos CSV muito grandes, com vários GB, sem consumir memória excessiva?
---

### Paginas do BI

- **Escola pública x privada** — o clássico, mas é clássico porque sempre entrega uma "problemática" forte e visual: diferença de desempenho por `TP_DEPENDENCIA_ADM_ESC`, separado por área de conhecimento. Ótimo gráfico de barras agrupadas.
- **Urbano x rural** (`TP_LOCALIZACAO_ESC`) — desigualdade de acesso, tema forte pra "extensão" com viés social.
- **Taxa de ausência/abstenção** (`TP_PRESENCA_*`) — quantos % dos inscritos efetivamente compareceram, comparado por UF ou tipo de escola. Isso é uma problemática em si (evasão do próprio exame), fácil de explicar num slide.
- **Situação de presença nos dois dias** (`TP_PRESENCA_CH` x `TP_PRESENCA_CN`) — categorizar os candidatos entre quem compareceu nos dois dias, faltou nos dois ou compareceu somente em um deles. Complementa a análise geral de abstenção já proposta.
- **Nota média por competência da redação** (`NU_NOTA_COMP1` a `COMP5`) — qual competência o Brasil vai pior (normalmente competência 4 — coesão/coesão textual). Ótimo gráfico de radar ou barras simples no Power BI.
- **Escola pública x privada, cruzado com urbano x rural** — 4 grupos, mostra que a desigualdade não é só uma variável isolada, ela se acumula (escola pública rural é o grupo mais vulnerável, tipicamente).
- **Redação anulada/zerada** (`TP_STATUS_REDACAO` diferente de 1) — quantos candidatos tiveram problema na redação e por qual motivo (cópia do texto motivador, fuga do tema, etc). Bom gráfico de pizza/barras com os motivos.
- **Gap por área do conhecimento entre UFs** — não só "quem tem a maior média geral", mas em qual matéria a diferença regional é maior (ex: talvez matemática tenha gap regional maior que linguagens). Isso é uma pergunta mais analítica que rende bem num slide de "problemática".
- **Distribuição (não só média)** — histograma de notas por tipo de escola. Média sozinha esconde desigualdade interna; mostrar a distribuição inteira (pública tem cauda mais longa à esquerda, por exemplo) é mais honesto e visualmente forte.
- **Índice de desigualdade simples** — algo tipo razão entre nota média de escola privada urbana vs pública rural, por UF, pra criar um "ranking de desigualdade educacional" — não é só "quem tem nota mais alta" mas "onde a diferença entre grupos é mais gritante".
- **Correlação entre taxa de ausência e nota média** — estados/tipos de escola com mais abstenção também têm nota mais baixa entre quem compareceu? (Hipótese: abstenção pode ser um sintoma do mesmo problema estrutural que gera nota baixa.)
- **Perfil etário dos candidatos** — apresentar a distribuição dos participantes por faixa etária e relacionar idade com a situação de conclusão do Ensino Médio.
- **A Brecha Digital e a Renda Familiar** (`Q007` x `Q020`) — analisar como o acesso à internet varia entre as diferentes faixas de renda, verificando se candidatos de menor renda apresentam maior proporção de falta de acesso à internet. Bom para visualizar a relação entre desigualdade econômica e inclusão digital.
