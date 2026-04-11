# QuantMetrics OS

**QuantMetrics OS** is de overkoepelende **orchestrator** voor de QuantMetrics-suite: één plek om **QuantBuild**, **QuantBridge** en **QuantLog** te starten met de juiste mappen, Python en configuratie. Deze repository bevat geen strategie- of brokercode zelf; die leven in aparte repos die je via een `.env`-bestand koppelt.

## Wat doet deze repo?

- **`orchestrator/quantmetrics.py`** — laadt `orchestrator/.env`, lost paden op naar de drie componenten en start subprocessen (`python -m …`, scripts) met de juiste `cwd` en `PYTHONPATH`.
- **`orchestrator/qm.ps1`** — handige PowerShell-wrapper op Windows.
- **`docs/`** — roadmap, sprintplan en handouts (o.a. suite-start).
- **`vscode/quant-suite.code-workspace`** — multi-root workspace om orchestrator + sibling-repos tegelijk te openen.
- **`scripts/clone_quant_suite.sh`** — optioneel: alle suite-repos clonen/updaten en een basis-`.env` genereren (Linux/macOS/WSL).

## Architectuur in het kort

```
Marktdata / nieuws
        ↓
┌─────────────────────────────────────────────────────────┐
│  QuantBuild  — signalen, risico, strategie (live loop)   │
└─────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────┐
│  QuantBridge — uitvoering: broker, orders, reconnect     │
└─────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────┐
│  QuantLog    — events (JSONL), validatie, dagrapporten   │
└─────────────────────────────────────────────────────────┘
        ↓
   Analyse, metrics, verbetering van strategieën
```

### QuantBuild (Build)

- **Rol:** strategie- en signaalmotor: data verwerken, signalen, entries/exits, risico per trade, portfolio-logica.
- **Typische output:** signalen, trade-acties, risk-beslissingen (die elders gelogd en geanalyseerd kunnen worden).
- **Via orchestrator:** `quantmetrics build` start o.a. `python -m src.quantbuild.app … live` onder `QUANTBUILD_ROOT`. Standaardconfig staat in `QUANTBUILD_CONFIG` (bijv. `configs/strict_prod_v2.yaml`). `--real` schakelt door naar echte orders (naast paper/dry-run), afhankelijk van je YAML.

### QuantBridge (Bridge)

- **Rol:** execution-laag: brokerkoppelingen, orders plaatsen/wijzigen/sluiten, reconnect, positie-sync, execution health.
- **Via orchestrator:** `quantmetrics bridge smoke` (o.a. cTrader smoke, `--mode mock` of `openapi`) en `quantmetrics bridge regression` voor de regressiesuite. Config via `QUANTBRIDGE_CONFIG` (bijv. `configs/ctrader_icmarkets_demo.yaml`).
- **Let op:** QuantBuild kan de bridge dynamisch laden; daarvoor hoort het pad naar de bridge-**src** vaak in de omgeving (bijv. `QUANTBRIDGE_SRC_PATH`) — zie de handout onder **Documentatie**.

### QuantLog (Log)

- **Rol:** gestructureerde events (signalen, trades, risk-blokkades, fouten, sessies, …), audit en replay; basis voor dagelijkse samenvattingen en scores.
- **Event-map:** standaard `<QUANTBUILD_ROOT>/data/quantlog_events` met dagmappen `YYYY-MM-DD`, tenzij je `QUANTLOG_EVENTS_ROOT` zet.
- **Via orchestrator:** `quantmetrics log <subcommando> …` roept `python -m quantlog.cli` aan onder `QUANTLOG_ROOT`. `quantmetrics post-run <datum>` draait achter elkaar: `validate-events`, `summarize-day`, `score-run` op die dagmap.

## Vereisten

- Python op je systeem (of per repo een venv; zet `PYTHON` in `.env` als je een andere binary wilt).
- **Drie aparte repositories** (Build, Bridge, Log) uitgecheckt op je machine — typisch als broermappen naast `quantmetrics_os`.

Aanbevolen layout:

```text
<parent>/
  quantmetrics_os/     ← deze repo
  quantbuildv1/        ← QuantBuild
  quantbridgev1/       ← QuantBridge
  quantlogv1/          ← QuantLog
```

## Snelstart

1. Kopieer de environment-template:

   ```text
   orchestrator/config.example.env  →  orchestrator/.env
   ```

2. Pas in `.env` de paden `QUANTBUILD_ROOT`, `QUANTBRIDGE_ROOT` en `QUANTLOG_ROOT` aan naar jouw lokale clones.

3. Controleer of alles klopt:

   ```powershell
   cd orchestrator
   python quantmetrics.py check
   ```

   Op Windows kan ook: `.\qm.ps1 check`

4. Voorbeelden:

   | Doel | Commando |
   |------|----------|
   | Paden tonen | `python quantmetrics.py check` |
   | QuantBuild live (paper, geen `--real`) | `python quantmetrics.py build -c configs/strict_prod_v2.yaml` |
   | QuantBridge rooktest (mock) | `python quantmetrics.py bridge smoke --mode mock` |
   | QuantLog (voorbeeld) | `python quantmetrics.py log validate-events -- --path <pad-naar-dagmap>` |
   | Na een handelsdag | `python quantmetrics.py post-run 2026-04-11` |
   | Telegram start/stop (als in QuantBuild-YAML geconfigureerd) | `python quantmetrics.py notify start build bridge log` |

Gebruik `python quantmetrics.py --help` en `python quantmetrics.py <commando> --help` voor alle opties.

## Documentatie in deze repo

- [Suite starten (handout)](docs/SUITE_START_HANDOUT.md) — `.env`, veelgebruikte commando’s, VS Code-workspace, `QUANTBRIDGE_SRC_PATH`.
- [Roadmap (platform)](docs/Roadmap_os.md) — visie, componenten, verbeterlus.
- [Sprintplan](docs/QUANTMETRICS_SPRINT_PLAN.md) — planning.

## VPS / Linux

Zie `orchestrator/config.vps.example.env` voor voorbeelden met absolute paden op een server.
