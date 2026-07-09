# AI-Player (fork)

> **Compatibility:** This fork currently works only with **Minecraft 1.21.1**.

This is a personal/hobby fork of the [AI-Player](https://github.com/shasankp000/AI-Player) mod, adapted to run on **PojavLauncher** (Minecraft Java Edition on Android/iOS). The goal here is to keep the mod lightweight and functional in this environment, without relying on heavy local models. PojavLauncher was discontinued in 2025; this fork is primarily intended for its successor, **MojoLauncher**, but it should also work on PojavLauncher (though this has not been tested).

---

## What has changed in this fork

- **No local model downloads** — it no longer downloads BERT/LIDSNet/CART/OpenNLP (~100MB+) at startup.
- **Intent classification via cloud LLM** — uses the configured provider (`gemini`, `openai`, `claude`, `grok`, `custom`). It only falls back to Ollama if you explicitly choose that mode.
- **Private embeddings** — instead of sending memories to Google/Gemini, you point to your own embedding server via `settings.json5`.
- **Pure Java vector search** — cosine similarity without native `sqlite-vec`/`sqlite-vss` extensions.
- **Lighter build** — DJL/PyTorch/libtorch removed from the `.jar`. ### Private embedding configuration (`settings.json5`)

```json5
{
"embeddingApiUrl": "http://your-server:8080",
"embeddingApiKey": "",
"embeddingModel": "local-embedding"
}
```

The server must expose an **OpenAI-compatible** `/v1/embeddings` endpoint:
- Request: `POST {embeddingApiUrl}/v1/embeddings` with `{"input": "...", "model": "..."}`
- Response: `{"data": [{"embedding": [...]}]}`

If `embeddingApiUrl` is empty, the mod works normally—it just won't remember past conversations (RAG is optional).

### Commands

- `/bot resetmemories` — clears all RAG memories from `memory_agent.db` (useful when changing the embedding model or dimension).

---

## About the original mod

The original AI-Player was created by **shasankp000** with the goal of eliminating loneliness in Minecraft by adding a truly intelligent "second player" to the game. The project has grown significantly and now features:

- Q-Learning for combat learning/reflexes
- RAG with a vector database and web search
- Multiple LLM providers (OpenAI, Claude, Gemini, Grok, custom)
- Autonomous goal and personality system
- In-game configuration interface

> **For news, updates, and the full version of the mod, follow the original creator:**
> - GitHub: https://github.com/shasankp000/AI-Player
> - Modrinth: https://modrinth.com/mod/ai-player/
> - CurseForge: https://www.curseforge.com/minecraft/mc-mods/ai-player

---

## Credits

- **Original creator**: [shasankp000](https://github.com/shasankp000) — all original code, research, and mod development. - **Support the original creator**:
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

Requires **Java 21** and IntelliJ IDEA (or any Gradle-compatible IDE).

1. Clone the repo and open it in IntelliJ.
2. Set the SDK to **Liberica 21** (or any JDK 21).
3. Wait for Gradle to sync, then run `./gradlew build` in the terminal.
4. Place the generated `.jar` file in the Minecraft `mods` folder.

If a Gradle error occurs, click the Gradle refresh button in the sidebar.

---

## Usage

1. Configure your LLM provider via `/configMan` or by editing `settings.json5`.
2. (Optional) Configure the private embedding endpoint as shown above. 3. Launch the game, open `/configMan`, select the language model, save, and exit.
4. Spawn the bot: `/bot spawn <name> <training|play>`

> **Note:** This fork is a personal/hobby project designed to run on PojavLauncher. It has no official affiliation with the original creator—if you enjoy the mod, please consider supporting their work.