# 🏫 EduConectada: Diagnóstico Estatístico da Conectividade nas Escolas Públicas Brasileiras

> 🌐 **Language / Idioma:** [English](README_en.md) • **Português**

![Python](https://img.shields.io/badge/Python-Pandas_%7C_Seaborn_%7C_Scikit--Learn-3776AB?logo=python&logoColor=white)
![Statistics](https://img.shields.io/badge/Estatística-Probabilidade_%7C_Inferência_%7C_TCL-8A2BE2)
![Dataset](https://img.shields.io/badge/Escolas_Analisadas-137.914-success)

> **Resumo Executivo:** Este projeto analisa a infraestrutura real de internet nas escolas públicas de educação básica do Brasil a partir da base **EduConectada (137.914 escolas)** e das medições ativas de tráfego do **SIMET (~77.000 escolas)**. Combinando modelagem de espaços amostrais ($\Omega_1$ e $\Omega_2$), probabilidade condicional, funções de distribuição acumulada, preparação para Machine Learning (`StandardScaler` vs. `MinMaxScaler`) e simulação do **Teorema Central do Limite**, o estudo responde a uma pergunta crítica de política pública: **a velocidade entregue às escolas é suficiente para o número de alunos matriculados?**

---

## 📊 Preview Visual das Análises

| Distribuição de Velocidade (Urbana vs. Rural) | Atendimento à Demanda por Localização e Porte |
| :---: | :---: |
| ![Urbano vs Rural](./reports/figures/cell_068_plot_08.png) | ![Atendimento Demanda](./reports/figures/cell_089_plot_10.png) |

| Normalização (`StandardScaler` vs `MinMaxScaler`) | Distribuição Amostral & Teorema Central do Limite |
| :---: | :---: |
| ![Normalização](./reports/figures/cell_103_plot_14.png) | ![Teorema Central do Limite](./reports/figures/cell_110_plot_21.png) |

---

## 1. Contexto e Definição dos Espaços Amostrais ($\Omega_1$ vs. $\Omega_2$)

Ao auditar as **137.914 escolas** da base (comparadas frente às ~136,7 mil escolas públicas ativas no Censo Escolar 2025), observou-se que variáveis cadastrais (localização, rede, porte, matrículas e laboratório de informática) possuem cobertura quase integral, enquanto as medições empíricas de rede pelo medidor **SIMET** estão disponíveis para um subconjunto específico. Para evitar viés analítico, foram estruturados dois espaços amostrais:

- **$\Omega_1$ (`137.914 escolas`):** Universo completo da base cadastral para análise de infraestrutura física, distribuição regional e presença declarada de internet (`indicador_internet`).
- **$\Omega_2$ (`~77.000 escolas`):** Subconjunto de escolas com conexão ativa e medição real de desempenho TCP pelo SIMET (`media_tcp_download`, `media_tcp_upload`, `media_latencia`, `media_jitter_upload`).

---

## 2. Principais Descobertas Estatísticas

### A. Assimetria Positiva e o Perigo das Médias Isoladas
- As distribuições de `media_tcp_download` e `media_tcp_upload` apresentam **forte assimetria positiva (cauda longa à direita)**. Poucas escolas com links de altíssima velocidade puxam a média para cima, escondendo a realidade da maioria das unidades escolares.
- Forte correlação positiva entre download e upload ($r = 0.69$), formando um comportamento em "leque" diagonal, e alta associação entre latência e jitter.

### B. O Paradoxo Urbano vs. Rural (Probabilidade Condicional)
1. **Velocidade Bruta (Mediana):** As escolas **urbanas** possuem uma mediana de velocidade de download aproximadamente **o dobro** da mediana das escolas **rurais** (`cell_068_plot_08.png`).
2. **Atendimento à Necessidade por Aluno (`diferenca_download`):** Quando cruzamos a velocidade entregue com a `velocidade_download_necessaria` (estimada para que 50% dos alunos assistam a um vídeo simultaneamente, proporcional ao porte de matrículas da escola):
   - A grande maioria das escolas brasileiras **não atende** à demanda necessária.
   - Usando **probabilidade condicional** ($P(\text{Atende} \mid \text{Localização})$), descobrimos um achado contraintuitivo: **30,17% das escolas rurais atendem à necessidade estimada, contra apenas 21,64% das escolas urbanas** (`cell_089_plot_10.png`).
   - **Explicação (Função de Repartição de Porte):** Embora as escolas urbanas tenham links maiores em Mbps absolutos, elas concentram portes muito maiores de alunos matriculados, fazendo com que a largura de banda por estudante seja insuficiente com maior frequência do que nas pequenas escolas rurais.

### C. Preparação para Machine Learning e Teorema Central do Limite
- **Escalonamento de Variáveis:** Comparação prática entre `StandardScaler` ($\mu = 0, \sigma = 1$) e `MinMaxScaler` ($[0, 1]$) sobre variáveis de escalas díspares (matrículas vs. Mbps), demonstrando a preservação da forma da distribuição original (`cell_103_plot_14.png`).
- **Simulação de Amostragem (`media_tcp_download`):** Demonstração empírica da convergência das médias amostrais para uma distribuição normal conforme o tamanho amostral $n$ cresce ($n=1 \rightarrow n=30 \rightarrow n=100 \rightarrow n=500$), reduzindo a variância amostral conforme previsto pelo Teorema Central do Limite (`cell_110_plot_21.png`).

---

## 3. Estrutura do Repositório

```text
educonectada-school-connectivity/
├── README.md                            # Síntese executiva, metodologia e achados principais
├── notebooks/
│   └── educonectada.ipynb               # Notebook completo em Python (EDA, Probabilidade, Normalização, TCL)
└── reports/
    └── figures/                         # 22 gráficos e visualizações extraídos do estudo
```
