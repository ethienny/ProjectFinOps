Simuladores FinOps Azure

Simulador FinOps — Geração de dados sintéticos de custos, métricas e recomendações para recursos Azure e exemplo de dashboard executivo.

📖 Visão Geral

Este projeto fornece:

Script PowerShell (SimuladoresFinOps_v5.ps1): gera dados de FinOps sintéticos (recursos, custos, métricas, recomendações) em modo simulação ou coleta real do tenant.

Exemplo de Dashboard: orientação para construção de um painel executivo (Power BI / grafana / web) com os dados exportados.

Uso ideal para demonstrações, treinamentos e testes de plataformas FinOps sem necessidade de ambiente real.

🛠️ Estrutura do Repositório

/ (root)
├─ README.md               # Documentação principal (este arquivo)
├─ SimuladoresFinOps_v5.ps1# Script PowerShell de simulação
├─ /data                   # Pasta para armazenar arquivos de saída
│   ├─ FinOps_CostData.csv
│   ├─ FinOps_MetricsData.csv
│   ├─ FinOps_Recommendations.csv
│   └─ FinOps_TagAnalysis.csv
└─ /dashboard              # Exemplos e arquivos de suporte para o dashboard
    ├─ Dashboard_Template.pbix   # Exemplo Power BI
    └─ samples               # Arquivos JSON/CSV de exemplo

⚙️ Pré-requisitos

PowerShell 7.0+

Módulos Az:

Az.Accounts

Az.Resources

Az.Monitor

Az.CostManagement

Permissões de leitura em subscriptions (modo real) ou modo simulação (-SimulationMode $true).

🚀 Como Executar

# Em modo simulação:
./SimuladoresFinOps_v5.ps1 \
  -SimulationMode $true \
  -MonthsBack 3 \
  -SubscriptionCount 5 \
  -ResourceDensity Medium \
  -OutputFolder "./data"

# Em modo real (coleta de dados do tenant):
./SimuladoresFinOps_v5.ps1 \
  -SimulationMode $false \
  -MonthsBack 6 \
  -SubscriptionCount 10 \
  -OutputFolder "./data"

Após a execução, os arquivos CSV/JSON/Excel estarão disponíveis em ./data.

📝 Documentação do Script

1. Parâmetros

Parâmetro

Tipo

Default

Descrição

MonthsBack

int

3

Quantos meses históricos gerar (1–36)

SimulationMode

bool

$true

true: simula; false: coleta real do Azure

SubscriptionCount

int

10

Número de subscriptions simuladas

ResourceDensity

string

Low|Medium|High

Densidade de recursos

OutputFolder

string

~/Documents/.../Output

Caminho para salvar arquivos

IncludeRecommendations

bool

$true

Se gera recomendações de otimização

ExportFormat

string

CSV|JSON|Excel

Formato dos arquivos de saída

LogLevel

string

Debug|Info|Warning|Error

Nível de logs

2. Fases Principais

Obter Subscriptions (Get-Subscriptions)

Gerar Recursos (New-ResourceInstances)

Gerar Custos (New-CostData)

Gerar Métricas (New-FastMetricsData / New-SampledMetricsData)

Gerar Recomendações (New-RecommendationData)

Exportar e Resumir (Export-ToCSV / Export-ToJSON / New-SummaryReport)

3. Estrutura de Dados

CostData: Date, SubscriptionId, ResourceName, Cost, Currency, <Tags>

MetricsData: Timestamp, SubscriptionId, ResourceName, MetricName, Value, Unit, <Tags>

Recommendations: SubscriptionId, ResourceName, Recommendation, PotentialSaving, <Tags>

TagAnalysis: estatísticas de distribuição de tags (Environment, BusinessUnit, Compliance...)

📊 Proposta de Dashboard Executivo

Uma dashboard FinOps eficiente deve contemplar:

Visão Geral de Custos

Gráfico de linha: custo total por mês (CostData)

Cartão: custo acumulado no período

Análise por Tipo de Serviço

Gráfico de barras: custo por ServiceType

Distribuição de Tags

Pizza/TreeMap: proporção por Environment, BusinessUnit

Tendências de Métricas

Séries temporais de CPU, memória, IOPS (MetricsData)

Recomendações de Otimização

Tabela com ações sugeridas e economia potencial (Recommendations)

Mapa de Calor de Recursos

Heatmap ou matriz: top 10 recursos por custo x métrica crítica

Ferramentas Sugeridas

Power BI (.pbix incluso em /dashboard)

Grafana + plugin CSV/JSON

Tableau ou Metabase

Exemplo Power BI

No diretório /dashboard há um template Dashboard_Template.pbix com:

Conexões diretas aos CSV em data/

Páginas configuradas conforme itens acima

Medidas DAX de resumo e KPIs




