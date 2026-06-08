# TOOLS.md - Ambiente local

Notas especificas deste workspace. Nao registrar senhas, tokens, API keys ou segredos aqui; referenciar apenas nomes de variaveis, storage ou config.

## Host

- VM OCI ARM64/aarch64.
- OS: Ubuntu 24.04.4 LTS.
- Gateway OpenClaw via systemd user: `openclaw-gateway.service`.
- Gateway bindado em loopback: `127.0.0.1:18789`.
- Acesso externo via Cloudflare Tunnel; IP publico direto fica fora do caminho.
- `cloudflared.service` roda como servico systemd do sistema com config em `/etc/cloudflared/config.yml`.

## Workspace e Git

- Workspace: `~/.openclaw/workspace`.
- Repo remoto privado: `https://github.com/andradelucascq/tars-openclaw.git`.
- Branch principal: `main`.
- Identidade local de commit: `Tars <tars@openclaw.local>`.
- Commits devem incluir trailer de coautoria do Lucas quando feitos por mim.
- Antes de `git add`, varrer o conteudo a commitar para evitar credenciais no historico.

## Voz e Midia

- Canal ativo: Telegram.
- Audio/midia do OpenClaw: `tools.media.audio.enabled=true`.
- Backend configurado para audio: Groq (`tools.media.audio.models[0].provider=groq`).
- `openai-whisper` local esta disponivel como fallback local sem API key.
- `openai-whisper-api` esta desativado; nao habilitar caminho pago da OpenAI sem pedido explicito.

## Busca e Memoria

- `qmd` esta ativo no workspace para busca/indexacao local (BM25 + vetores + rerank).
- `MEMORY.md` e memoria curada de longo prazo.
- `memory/YYYY-MM-DD.md` e diario bruto quando houver anotacoes.
- `session-memory` salva contexto em eventos `/new` ou `/reset`; manter como complemento ao QMD, nao substituto.

## Hooks ativos

- `boot-md` (bundled): carrega arquivos de boot no startup.
- `bootstrap-extra-files` (bundled): injeta arquivos extras de contexto por paths/patterns; nao recria estrutura sozinho.
- `command-logger` (bundled): registra eventos de comando em `~/.openclaw/logs/commands.log`.
- `compaction-notifier` (bundled): avisa quando compaction comeca/termina.
- `session-memory` (bundled): registra contexto em mudancas de sessao.

## Skills e ferramentas relevantes

- `clawhub` esta funcional. CLI: `~/.local/bin/clawhub` -> `~/.openclaw/tools/node/npm/bin/clawhub`.
- `github` esta pronto; `gh` autenticado na VM via credential storage do GitHub CLI.
- `nano-pdf` esta pronto para edicao de PDFs.
- `model-usage` nao esta ativo nesta VM: a skill atual depende de macOS/CodexBar para leitura local ao vivo.
- `usage-report` esta pronto como substituto Linux/ARM64 para usage/cost: usa `npx -y ccusage@20.0.6` com timezone `America/Sao_Paulo`.
- `mcporter` nao esta instalado/ativo; instalar apenas se Lucas decidir usar MCP routing.

## Governanca

- Skill externa e codigo de terceiro com acesso local. Buscar/verificar/recomendar e livre; instalar, atualizar, aplicar, habilitar ou sincronizar e ask-first.
- Antes de instalar skill externa, rodar `openclaw skills verify <skill>` e revisar SkillSpector, static scan, VirusTotal e assinatura.
- Se verificacao vier unsigned, suspicious, fail ou ambigua, explicar e aguardar aprovacao explicita.
- Gateway health/probe pode pedir scope upgrade. Nao aprovar upgrade de escopo sozinho.
