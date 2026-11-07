Hier eine **Mini-Doku zum Copy-Paste in Apple Notes**. An dein aktuelles Setup angepasst (`factory` + Bionic + Sandbox).

---

# Local AI Factory — Start-Cheat-Sheet
Stand: Nov 2026 · Repo: ~/Projects/factory · Sandbox: factory-opencode

## Was du am Ende hast
- Bionic läuft lokal mit Qwen
- OpenCode in Docker-Sandbox am Repo `factory`
- Context7 für aktuelle Docs (über sbx mcp / Gateway)
- Du chattest in der OpenCode-TUI

---

## Einmalig erledigt (nur nachschauen, nicht jedes Mal)
- Context7 registriert: `sbx mcp ls` → context7 = ready
- Secret gesetzt: `sbx secret` → mcp:context7:api-key
- Repo-Datei: `factory/opencode.json` mit Bionic + host.docker.internal:1235
- Optional: `AGENTS.md` im Repo (Next-Block stehen lassen + eigene Regeln darunter)
- Netz erlaubt (falls schon gemacht):
  `sbx policy allow network --sandbox factory-opencode localhost:1235`

---

## JEDEN TAG / Jede Session — Reihenfolge

### 1) Bionic starten (lokales Modell)
1. App **Bionic** öffnen
2. Modell laden: **qwen3-coder-30b-a3b-instruct-mlx**
3. API-/Server anlassen (OpenAI-kompatibel auf Port **1235**)

Check im Terminal:
```bash
curl -sS http://localhost:1235/v1/models | grep qwen
```
Erwartet: Zeile mit `qwen3-coder-30b-a3b-instruct-mlx`

Ohne diesen Check → OpenCode findet Qwen nicht.

---

### 2) Ins Projekt
```bash
cd /Users/philipp.maecke/Projects/factory
```

---

### 3) Sandbox + OpenCode starten
Wenn Sandbox schon existiert:
```bash
sbx run --name factory-opencode
```

Beim allerersten Mal (nur falls noch nicht angelegt):
```bash
cd /Users/philipp.maecke/Projects/factory
sbx run --name factory-opencode opencode --static-mcp context7
```

→ OpenCode-TUI öffnet sich im Terminal.  
NICHT so starten: `sbx exec … opencode` (kaputte TUI).

---

### 4) Context7 in die Sandbox laden
Zweites Terminal (Host), während OpenCode laufen kann:
```bash
sbx mcp load context7 --sandbox factory-opencode
```

Check:
```bash
sbx mcp ls
```

In OpenCode `/mcp`: oft nur **mcp-gateway** sichtbar — das ist ok. Context7 kommt darüber.

---

### 5) Netzwerk-Freigabe (falls „Approval required“)
```bash
sbx policy approval ls
```

Wenn Eintrag für localhost:1235 / Bionic:
```bash
sbx policy approval respond network:HIER_DIE_ID --option allow
```

Dauerhaft (einmal):
```bash
sbx policy allow network --sandbox factory-opencode localhost:1235
sbx policy allow network --sandbox factory-opencode host.docker.internal:1235
```

Context7-Host bei Bedarf freigeben, wenn Approval kommt:
`mcp.context7.com`

Danach Prompt in OpenCode **nochmal** senden.

---

### 6) Modell in OpenCode
- Picker / `/models`
- **Bionic Local** / `qwen3-coder-30b-a3b-instruct-mlx`

Schnelltest:
```text
Antworte ausschließlich mit: LOCAL QWEN OK
```

Context7-Test:
```text
Nutze Context7. Resolve library id für next.js. Kurz halten. Keine Dateien ändern.
```

---

## Nützliche Befehle (zweites Terminal)

Sandbox-Status:
```bash
sbx ls
```

Befehl in der Sandbox:
```bash
sbx exec factory-opencode git status
sbx exec factory-opencode git diff --stat
sbx exec factory-opencode npm run lint
sbx exec factory-opencode npx tsc --noEmit
```

Config im Repo prüfen:
```bash
sbx exec factory-opencode grep bionic opencode.json
```

Sandbox stoppen:
```bash
sbx stop factory-opencode
```

Wieder rein:
```bash
sbx run --name factory-opencode
```

---

## AGENTS.md
- Liegt im Repo: wird von Host + Sandbox geteilt (gleicher Ordner, kein Extra-Clone)
- Speichern in Cursor reicht für die Datei
- OpenCode: nach Änderung **neue Session**, damit Regeln sicher greifen
- Next-Block oben **nicht löschen**; eigene Regeln darunter
- `head -5` zeigt nur den Next-Anfang — mit `tail` prüfen

---

## Typische Probleme

| Symptom | Fix |
|---|---|
| Kein lokales Modell | Bionic an? curl models? opencode.json im Repo? /models wählen? |
| Approval required | `sbx policy approval ls` → respond allow → Prompt nochmal |
| Context7 fehlt | `sbx mcp load context7 --sandbox factory-opencode` |
| Seltsame Zeichen in TUI | Nicht per `sbx exec opencode` starten → `sbx run --name factory-opencode` |
| Agent exit 137 | Speicher/Docker; `sbx diagnose`; Bionic+Sandbox entlasten |
| Config ohne bionic in ~/.config | Egal — **Repo**-`opencode.json` zählt; nicht in /home/agent basteln |

---

## Mini-Alltagsablauf (30 Sekunden)
1. Bionic an + Modell geladen  
2. `curl …/models | grep qwen`  
3. `cd ~/Projects/factory`  
4. `sbx run --name factory-opencode`  
5. `sbx mcp load context7 --sandbox factory-opencode`  
6. Modell = Qwen lokal  
7. Loslegen  

---

## Was wir bewusst noch NICHT brauchen für den Alltag
- OpenRouter (nur Plan B)
- GitHub-Issue-Bot (kommt später = Stufe 3)
- Context7 in opencode.json (läuft über sbx mcp)

---
