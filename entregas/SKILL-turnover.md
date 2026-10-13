---
name: analise-turnover
description: Calcula e interpreta turnover geral, voluntario, involuntario e por recorte de departamento, cargo e tempo de casa. Use when pedir rotatividade, desligamento, retencao, diagnostico de RH ou analise da base rh-cosmeticos-2026.
---

# Skill Analise Turnover - Diagnostico de Rotatividade

Skill reutilizavel para trabalhos de Administracao e TIC. Aplica o passo a passo consagrado na literatura de RH a qualquer base com coluna de desligamento.

## 1. O que faz

1. Calcula taxa geral, voluntaria, involuntaria e precoce (<90 dias / <1 ano).
2. Abre por recorte: departamento, cargo, gestor, tempo de casa, faixa etaria, genero, salario, avaliacao.
3. Retorna: passo a passo, calculo com fonte, interpretacao e limitacao declarada.
4. Gera tabela de hipoteses e plano de acao trimestral.

## 2. Quando nao usar

- Se nao houver coluna de desligamento ou periodo definido. Pedir ao usuario.
- Se pedirem previsao individual de quem vai sair. Responder apenas risco por grupo.

## 3. Conceito base

**Definicao:** turnover e o percentual de desligamentos sobre o efetivo medio no periodo. Mede saude da contratacao, lideranca, remuneracao e clima.

**Formulas padrao. Usar sempre a mesma no trabalho inteiro:**

1. `Turnover (%) = (n desligamentos no periodo / media de colaboradores no periodo) x 100`
2. `Media = (headcount inicio + headcount fim) / 2`
3. Variacao geral com admissoes: `([(admissoes + desligamentos) / 2] / total ativos) x 100`
4. Cortes obrigatorios: geral, voluntario x involuntario, <90 dias, por area/gestor/cargo.

**Tipos:**
- Voluntario: pessoa pede demissao. Sinaliza retencao, lideranca, salario, carreira.
- Involuntario: empresa desliga. Sinaliza selecao ou gestao de desempenho.
- Funcional: sai baixo desempenho. Renovacao saudavel se conduzida com respeito.
- Disfuncional: sai alto desempenho. Alarme vermelho.
- Precoce: sai com <90 dias. Falha de selecao ou onboarding.

## 4. Passo a passo

### Passo 1 - Fixar periodo e regra
Definir mensal com visao 3/6/12 meses. Registrar formula usada. Nunca trocar no meio da serie.

### Passo 2 - Coletar colunas
Buscar no HRIS ou CSV: matricula, departamento, cargo, data admissao, tempo empresa, salario, avaliacao desempenho, motivo desligamento, flag desligado.
Na base rh-cosmeticos-2026: `desligado_12m, motivo_desligamento, departamento, cargo, tipo_contrato, tempo_empresa_anos, salario_mensal, avaliacao_desempenho, faixa_etaria, genero, satisfacao_pesquisa`. Arquivo com separador `;`, 180 linhas, 0 duplicadas.

### Passo 3 - Calcular taxa geral
Contar `desligado_12m = Sim` / total. Mostrar conta.
Exemplo base atual: 28 / 180 x 100 = 15,6% em 12m. Acima do alerta de 10 a 15%.

### Passo 4 - Separar por tipo
Usar `motivo_desligamento`: Pedido de demissao = voluntario. Sem justa causa, Fim de contrato, Acordo = involuntario.
Cruzar com `avaliacao_desempenho` 4-5 que saiu = disfuncional. Avaliacao 1-2 que saiu = funcional.
Cruzar com `tempo_empresa_anos < 1` = precoce.

### Passo 5 - Abrir por recorte
Calcular a mesma formula por `departamento, cargo, tipo_contrato, faixa_etaria, genero, faixa salarial, tempo de casa`.
Perguntar: quem sai? quando sai? de onde sai? Comparar Producao (52), Comercial/Vendas (35), Logistica (27).

### Passo 6 - Enriquecer com quali
Usar entrevista de desligamento e `satisfacao_pesquisa`. Classificar causa: carreira, remuneracao, lideranca, clima, flexibilidade.
Declarar vazios: na base atual `satisfacao_pesquisa` tem 17 vazios (9,4%).

### Passo 7 - Apontar causa raiz
Testar 3 blocos: gestao (um lider concentra saidas?), contratacao/onboarding (pico <1 ano?), remuneracao (salario abaixo da mediana 3.856,07?).
Montar tabela: linhas = desligados voluntarios 12m, colunas = lider direto, tempo de casa, ultima avaliacao, motivo declarado.

### Passo 8 - Calcular custo
Somar: rescisao + reposicao (anuncio, horas seletivo) + rampa (meses ate plena entrega) + invisivel (conhecimento, sobrecarga, contagio).
Estimativa conservadora para relatorio: 6 salarios mensais por desligamento evitavel. Referencia de mercado SHRM: 50% a 200% do salario anual. Mostrar conta, nao apenas benchmark.

### Passo 9 - Reportar e agir
Entregar: taxa geral, tabela por recorte, 3 hipoteses por area, plano 90 dias (onboarding 30-60-90, matriz carreira, revisao salarial, formacao lideres, stay interview com 10% criticos).
Definir OKR: ex. reduzir turnover <90 dias em 30% no trimestre. Repetir a cada trimestre.

## 5. Exemplo de uso

Usuario: qual o turnover da producao?
Skill responde:
- Conta: 9 desligados / 52 de producao x 100 = 17,3%. Fonte: rh-cosmeticos-2026.csv, coluna departamento + desligado_12m.
- Tipo: 6 voluntarios, 3 involuntarios. 2 disfuncionais (avaliacao 5).
- Interpretacao: acima da media geral 15,6%, concentrado em tempo <1 ano.
- Limitacao: motivo detalhado de 1 caso: NAO SEI.
- Decisao humana: validar se temporarios entram na conta.

## 6. Interpretacao padrao

Taxa sozinha nao diagnostica. Direcao da serie importa mais que nivel. Turnover disfuncional perto de zero e a meta real. Turnover zero permanente tambem e sintoma de acomodacao.

## 7. Limitacao declarada

Base atual e foto de 12m, nao serie historica. Sem data de desligamento nao da para sazonalidade. Sem ID de gestor nao da para efeito lideranca. Para tudo sem fonte: NAO SEI.

## 8. Referencias verificaveis

- AIHR, Employee Turnover Data Analysis: 8 Tips for Success. Passos 1 a 8.
- Buk Brasil, Entenda como calcular o turnover. Formula mensal e cortes <90 dias.
- Promova RH, Turnover: o que e, como calcular e qual taxa e saudavel. Tipos e custo.
- TOTVS, Turnover: o que e, tipos e como calcular. Variacao com admissoes.
- HRMorning, How to Calculate Turnover Rate. Media headcount inicio+fim / 2.
- Convenia, Relatorios e Indicadores de Turnover. Voluntario x involuntario mensal/anual.
- Perfil perfil-rh-cosmeticos-2026.md: 180 linhas, separador `;`, mediana salario 3.856,07.

## 9. Regras fixas da skill

- Nunca inventar numero ou referencia. Sem fonte: escrever NAO SEI.
- Nunca apresentar numero sem dizer de onde veio (arquivo + coluna + periodo).
- So produzir rascunho e apontar decisao humana.
- Linguagem tecnica, frases curtas, termo definido na primeira ocorrencia.
- Formato de saida: passo a passo, interpretacao e limitacao declarada.
