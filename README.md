name: 📊 Gerar métricas do perfil

on:
  schedule:
    - cron: "0 6 * * *"   # todo dia às 03h (horário de Brasília)
  workflow_dispatch:       # permite rodar manualmente
  push:
    branches: [main]
    paths: [".github/workflows/metrics.yml"]

permissions:
  contents: write

concurrency:
  group: metrics
  cancel-in-progress: false

jobs:
  metrics:
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      # Cabeçalho + atividade + comunidade + repositórios (substitui o cartão de stats)
      - name: Métricas gerais
        uses: lowlighter/metrics@latest
        with:
          filename: metrics/geral.svg
          token: ${{ secrets.METRICS_TOKEN }}
          user: JoaoGabrielMSantos
          template: classic
          base: header, activity, community, repositories
          config_timezone: America/Sao_Paulo
          config_octicon: yes

      # Linguagens mais usadas (substitui o cartão de top languages)
      - name: Linguagens
        uses: lowlighter/metrics@latest
        with:
          filename: metrics/linguagens.svg
          token: ${{ secrets.METRICS_TOKEN }}
          user: JoaoGabrielMSantos
          base: ""
          config_timezone: America/Sao_Paulo
          plugin_languages: yes
          plugin_languages_sections: most-used
          plugin_languages_details: percentage
          plugin_languages_limit: 8
          plugin_languages_threshold: 2%

      # Calendário isométrico 3D (substitui o gráfico de atividade)
      - name: Calendário isométrico
        uses: lowlighter/metrics@latest
        with:
          filename: metrics/calendario.svg
          token: ${{ secrets.METRICS_TOKEN }}
          user: JoaoGabrielMSantos
          base: ""
          config_timezone: America/Sao_Paulo
          plugin_isocalendar: yes
          plugin_isocalendar_duration: full-year

      # Conquistas (substitui os troféus)
      - name: Conquistas
        uses: lowlighter/metrics@latest
        with:
          filename: metrics/conquistas.svg
          token: ${{ secrets.METRICS_TOKEN }}
          user: JoaoGabrielMSantos
          base: ""
          config_timezone: America/Sao_Paulo
          plugin_achievements: yes
          plugin_achievements_display: compact
          plugin_achievements_threshold: B
          plugin_achievements_limit: 8
