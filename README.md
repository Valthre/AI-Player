# AI-Player (fork)

> **Compatibilidade:** este fork atualmente funciona apenas com **Minecraft 1.21.1**.

Este é um fork pessoal/hobby do mod [AI-Player](https://github.com/shasankp000/AI-Player), adaptado para rodar no **PojavLauncher** (Minecraft Java Edition no Android/iOS). O objetivo aqui é manter o mod leve e funcional nesse ambiente, sem depender de modelos locais pesados. O PojavLauncher foi descontinuado em 2025; este fork é feito principalmente para o seu sucessor, o **MojoLauncher**, mas também deve funcionar no PojavLauncher (não foi testado).

---

## O que mudou neste fork

- **Sem downloads de modelos locais** — não baixa mais BERT/LIDSNet/CART/OpenNLP (~100MB+) no startup.
- **Intent classification via LLM cloud** — usa o provedor configurado (`gemini`, `openai`, `claude`, `grok`, `custom`). Só recorre ao Ollama se você escolher explicitamente esse modo.
- **Embeddings privados** — ao invés de enviar memórias para a Google/Gemini, você aponta para o seu próprio servidor de embeddings via `settings.json5`.
- **Busca vetorial em Java puro** — cosine similarity sem extensões nativas `sqlite-vec`/`sqlite-vss`.
- **Build mais leve** — DJL/PyTorch/libtorch removidos do `.jar`.

### Configuração do embedding privado (`settings.json5`)

```json5
{
  "embeddingApiUrl": "http://seu-servidor:8080",
  "embeddingApiKey": "",
  "embeddingModel": "local-embedding"
}
```

O servidor deve expor um endpoint **OpenAI-compatible** `/v1/embeddings`:
- Request: `POST {embeddingApiUrl}/v1/embeddings` com `{"input": "...", "model": "..."}`
- Response: `{"data": [{"embedding": [...]}]}`

Se `embeddingApiUrl` estiver vazio, o mod funciona normalmente — só não lembra conversas antigas (RAG opcional).

### Comandos

- `/bot resetmemories` — limpa todas as memórias RAG do `memory_agent.db` (útil ao trocar modelo de embedding ou dimensão).

---

## Sobre o mod original

O AI-Player original foi criado por **shasankp000** com o objetivo de eliminar a solidão no Minecraft, adicionando um "segundo jogador" realmente inteligente no jogo. O projeto cresceu muito e hoje conta com:

- Q-Learning para aprendizado de combate/reflexos
- RAG com banco de dados vetorial e busca web
- Múltiplos provedores de LLM (OpenAI, Claude, Gemini, Grok, custom)
- Sistema de metas autônomas e personalidade
- Interface de configuração in-game

> **Para novidades, atualizações e a versão completa do mod, siga o criador original:**
> - GitHub: https://github.com/shasankp000/AI-Player
> - Modrinth: https://modrinth.com/mod/ai-player/
> - CurseForge: https://www.curseforge.com/minecraft/mc-mods/ai-player

---

## Créditos

- **Criador original**: [shasankp000](https://github.com/shasankp000) — todo o código original, pesquisa e evolução do mod.
- **Apoie o criador original**:
  - [Buy Me A Coffee](https://buymeacoffee.com/shasankp000)
  - Ethereum: `0x47014CC9F8054593027c53996ddfDFa4ca8a5271`
  - Bitcoin: `bc1qae6u7rqv0pmppxmr2uqfftrj4p6c7hmm3m39r5`
  - Solana: `3U3bXZJ2NrMV9FmkaotvPaNwWthg68sRqvstJwanyhcU`
  - Polygon (USDC): `0x47014CC9F8054593027c53996ddfDFa4ca8a5271`
- **NLP pipeline**: https://github.com/shasankp000/NLP_2.0_pipeline
- **Website**: https://github.com/shasankp000/AI-Player-Website
- **Carpet mod**: https://github.com/gnembon/fabric-carpet
- **ollama4j**: https://github.com/amithkoujalgi/ollama4j

---

## Build

Requer **Java 21** e IntelliJ IDEA (ou qualquer IDE compatível com Gradle).

1. Clone o repo e abra no IntelliJ.
2. Configure o SDK para **Liberica 21** (ou qualquer JDK 21).
3. Espere o Gradle sincronizar, depois rode `./gradlew build` no terminal.
4. Coloque o `.jar` gerado na pasta `mods` do Minecraft.

Se der erro no Gradle, clique no botão de refresh do Gradle na sidebar.

---

## Uso

1. Configure seu provedor de LLM via `/configMan` ou editando `settings.json5`.
2. (Opcional) Configure o endpoint de embedding privado como mostrado acima.
3. Ligue o jogo, abra `/configMan`, selecione o modelo de linguagem, salve e saia.
4. Spawne o bot: `/bot spawn <nome> <training|play>`

> **OBS:** este fork é um projeto pessoal/hobby, feito para rodar no PojavLauncher. Não tem relação oficial com o criador original — se curtir o mod, considere apoiar o trabalho dele.
