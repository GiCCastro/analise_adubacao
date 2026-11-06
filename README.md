# Análise de Adubação

Projeto desenvolvido em 2026.1, publicado em novembro.

Análise exploratória de dados agrícolas que cruza **química do solo**, **qualidade da aplicação de fertilizantes/corretivos**, **alarmes de máquinas** e **rotação do motor**. O objetivo é entender o que pode estar limitando a produtividade do milho.

Projeto desenvolvido no contexto da **Fábrica de Projetos**. A apresentação completa está em [`Apresentação - Fábrica de Projetos (1).pdf`](./Apresentação%20-%20Fábrica%20de%20Projetos%20(1).pdf).

---

## Estrutura do repositório

| Arquivo | Descrição |
|---|---|
| `análise_de_adubação.py` | Script principal com toda a análise (exportado do Google Colab) |
| `Apresentação - Fábrica de Projetos (1).pdf` | Slides com os resultados |

> Os datasets `.csv` e os mapas gerados **não estão no repositório** por conterem dados confidenciais da empresa.

### Datasets usados

- `soil_analysis(in).csv`: análise de solo nas camadas 0–20 cm e 20–40 cm
- `LAYER_MAP_ALARM(in).csv`: alarmes da frota, com geolocalização
- `LAYER_MAP_FERTILIZATION.csv`: dose configurada x aplicada, por polígono
- `LAYER_MAP_ENGINE_ROTATION.csv`: telemetria de rotação do motor

---

## Análises realizadas

### 1. Solo
- Textura (argila, areia, silte) comparada com valores de referência.
- Diagnóstico de **barreiras químicas no subsolo**:
  - pH baixo (`PHCACL2_2 < 5.5`)
  - Alumínio tóxico (`AL_2 > 0.3` ou `SATAL_2 > 20`)
  - Baixa saturação por bases (`SATB_2 < 60`)
- Classificação do nível de limitação e comparação entre superfície e subsolo.
- Correlação entre os atributos químicos.

### 2. Alarmes
- Tratamento de valores nulos e extração de latitude/longitude a partir de WKT.
- Os 5 alarmes mais frequentes, alarmes de caçamba de adubo vazia e perdas de sinal GPS por máquina.

### 3. Fertilização
- Cálculo da **eficiência de aplicação** (`aplicado / configurado × 100`):

  | Classe | Eficiência |
  |---|---|
  | Muito baixa | < 80% |
  | Baixa | 80–95% |
  | Adequada | 95–105% |
  | Excesso | > 105% |

- Identificação de déficits maiores que **1 t/ha**.

### 4. Rotação do motor
- Estatísticas de RPM e detecção de picos críticos (> 2200 RPM), com foco na operação de **calagem**.
- Conclusão: a instabilidade do motor aparece como gatilho mecânico para os grandes desvios de dose.

---

## Mapas interativos

Ao executar, o script gera três mapas no navegador (arquivos `.html`):

- `mapa_eficiencia.html`: pontos coloridos pela eficiência da aplicação
- `mapa_poligonos_eficiencia.html`: polígonos por eficiência, com dose planejada/aplicada
- `mapa_heatmap_alarmes.html`: mapa de calor dos alarmes das máquinas

---

## Tecnologias

Python · Pandas · NumPy · Matplotlib · Seaborn · GeoPandas · Shapely · Folium
