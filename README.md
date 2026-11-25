# SABELAR AI

**Private Artificial Intelligence Engine**

> Modelo de linguagem executado no próprio servidor, isolado da rede dos demais sistemas.
>
> **A Vladson Labs Project** · Developed by Vladson Bezerra
> Participação autoral: Arquitetura, integração e operação. Os modelos e o motor de inferência são de terceiros.

IA local e centralizada da MAG Móveis Corporativos na VPS: um Ollama só com CPU (`qwen3.5:0.8b-q8_0`) atrás de um
gateway interno com chave por aplicação, limites e fila única. Primeiro cliente: BIOMIE v2.

**Situação (05/11/2026, tarde):** Fases 1 e 2 concluídas; condições do parecer atendidas (D3 validada, D4 ajustada,
checklist final sem falhas). **Nada instalado na VPS.** Fase 3 aprovada em 05/11; execução na madrugada de
sábado 07/11, das 00:30 às 01:30 (roteiro em docs/fase3-checklist-e-roteiro.md).

| pasta | conteúdo |
|---|---|
| `docs/` | `fase1-diagnostico-20261105.md`, `fase2-plano.md`, `d3-validacao-dockerfile.md`, `fase3-checklist-e-roteiro.md` |
| `deploy/` | `compose.yml`, Dockerfiles do Ollama (só CPU, pacote oficial conferido) e do gateway, `env.exemplo`, `apps.exemplo.json`, manifesto fixado do modelo |
| `gateway/` | gateway em Node 24 sem dependências; `npm test` (20 testes) |
| `ops/` | `mag-ai.sh` (checklist, passos com portões, ensaio de reversão), `teste_cliente.mjs` e `sonda_rede.mjs` (testes na VPS), `saude.py` (monitor), `ensaio_local.mjs` |
| `srv/` | `MANIFESTO.md`, `backup.sh`, `restore.sh` para `/srv/mag-ai` |

Testes locais:

    cd gateway && npm test
    node ops/ensaio_local.mjs    # teste de ponta a ponta contra o gateway real e um Ollama de mentira

Na VPS (Fase 3), a partir da pasta do pacote enviado:

    bash ops/mag-ai.sh checklist        # somente leitura; precisa sair sem FALHA
    bash ops/mag-ai.sh verificar && bash ops/mag-ai.sh preparar && bash ops/mag-ai.sh construir \
      && bash ops/mag-ai.sh modelo && bash ops/mag-ai.sh subir && bash ops/mag-ai.sh testar \
      && bash ops/mag-ai.sh ensaiar-reversao && bash ops/mag-ai.sh comparar antes depois
    # voltar atrás: bash /opt/mag-ai/bin/mag-ai.sh desfazer

---

## Autoria

Projeto meu — concepção, arquitetura, implementação, implantação e operação em produção.
Durante o desenvolvimento usei assistentes de IA (Codex e Claude) como ferramenta de apoio;
a condução e as decisões técnicas são minhas.
