### Profiling guidelines

- Durante os benchmarks o tempo total da execução será maior em função das interrupções geradas pela análise
- A arquitetura produtiva do software deve ser utilizada para a realização dos testes (Servidor de produção)
- A máquina onde o teste será realizado deve estar o máximo idle possível
- Becnkmarks devem ser executados algumas vezes para resultados consistentes
- Profiling de CPU e Memória agregam mais valor que block profiling
- Profiling devem ser executados separadamente, apenas um de cada vez
  
- Performance nunca deve ser adivinhada
  
- Utizar netcat, ab, hey (IANA + Google), perf (Linux), pprof, time, ...