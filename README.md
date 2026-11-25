# Keikaku

**Plan, build and ship software with a team of AI agents — on models you run
yourself.**

Keikaku turns a description of what you want into outcomes and tasks, then
hands those tasks to agents that write the code, test it, review it and fix
it. Agents run on **open-weights models on your own GPU** (Ollama), so your
code and prompts never have to leave your machine. Keikaku runs fully
air-gapped if you need it to.

- 🌐 **Website:** [keikaku.ai](https://keikaku.ai)
- ☁️ **Keikaku Cloud:** [app.keikaku.ai](https://app.keikaku.ai) — hosted
  control plane, your agents still run on your hardware
- 🏠 **Self-hosted:** the same server, one Docker container — below

## Run an agent

An agent is a container on a machine with a GPU. It connects out to Keikaku
(Cloud or your own server), picks up tasks, and runs them on your local model.

1. Install [Ollama](https://ollama.com) natively on the host (so it owns the GPU).
2. In Keikaku, go to **Agents → New agent** and copy the connection token.
3. ```bash
   cd agent
   cp .env.example .env      # paste AGENT_TOKEN; set API_BASE_URL if self-hosted
   docker compose up -d
   ```
4. Back in Keikaku, open the agent and click **Start self-test** — it measures
   your hardware and recommends the best model for each kind of work.

## Self-host the server

```bash
cd server
cp .env.example .env         # set a long random JWT_SECRET
docker compose up -d
```

Open **https://localhost:9143** (self-signed certificate on first run). If you
left `ADMIN_PASSWORD` blank, the generated one is in the logs:

```bash
docker compose logs server | grep "generated password"
```

Your data lives in the `keikaku_data` volume (SQLite) — back it up.

## Images

| Image | What it is |
| --- | --- |
| [`ghcr.io/keikaku-ai/server`](https://github.com/orgs/keikaku-ai/packages/container/package/server) | Self-hosted Keikaku: API, web app and database in one container |
| [`ghcr.io/keikaku-ai/agent`](https://github.com/orgs/keikaku-ai/packages/container/package/agent) | The agent appliance; works with Keikaku Cloud and self-hosted servers |

Agents update themselves in place from the dashboard (**Update now**), so you
rarely need to pull a new image.

## Help and feedback

Bugs and questions: [open an issue](https://github.com/keikaku-ai/keikaku/issues).

The Keikaku source code is not public; this repository holds the public
deployment files and documentation.
