# Adaptação `Concatbr` para o Super Motor Vollare

**Repositório:** `https://github.com/theobarros22/Concatbr`  
**Licença:** `GNU AGPL-3.0-or-later` (preservada integralmente conforme [`LICENSE`](../LICENSE) e [`LICENSING.md`](../LICENSING.md))  
**Política de Marca:** Fork modificado renomeado para **`Concatbr`** em respeito a [`TRADEMARK.md`](../TRADEMARK.md). Este projeto é um fork independente adaptado para o fluxo do **Super Motor Vollare** (Mestre Theo Barros), compatível com projetos `concat.json` do projeto original [Concat](https://github.com/jub0t/Concat) de Jareer (`@jub0t`).

---

## 1. O que já está implementado na branch `f6/VOLLARE-TIMELINE`

1. **Rebranding inicial (`Concatbr`):**
   - Título da janela principal Slint (`src/crates/concat/ui/app.slint`) e diálogos nativos (`src/crates/concat/src/platform.rs`) atualizados para **`Concatbr`**.
2. **Proteção nativa de integridade Vollare (`concat-project`):**
   - `Track::is_vollare_locked()` lê `vollareLocked: true` no mapa `extra` de cada faixa.
   - `Clip::is_vollare_locked()` lê `vollareTravado: true` ou `vollareTipo == "selo_ia"` no mapa `extra` de cada clipe.
   - `commands::apply()` recusa com `CommandError::VollareLocked` qualquer tentativa (pela UI ou via `--mcp`) de apagar, cortar (`SplitClips`), encurtar (`TrimClip`), mover (`MoveClips`), ocultar (`UpdateClip` / `SetTrackFlag`) ou cobrir faixas e selos de IA obrigatórios do Vollare.
   - `AddTrack { bottom: false }` e `fresh_track()` inserem novas faixas de usuário **abaixo** da faixa superior `vollareLocked`, garantindo que o selo obrigatório (`VÍDEO GERADO POR IA` / `NARRAÇÃO GERADA POR INTELIGÊNCIA ARTIFICIAL`) nunca seja encoberto.
3. **Auto-travamento visual na abertura do projeto (`panes/timeline.rs`):**
   - Ao abrir um `concat.json` exportado pelo Vollare (`vollare concat exportar`), `TimelineMsg::Reset` ativa automaticamente `LaneView::locked = true` nas faixas `vollareLocked`.
4. **Roundtrip sem perda (`extra` map):**
   - Todos os campos `vollareId`, `vollareTrilha`, `vollareTipo`, `vollareOrigem`, `vollareTravado`, `vollareT0`, `vollareT1`, `vollareParam`, `vollareAncora` e `vollareMeta` sobrevivem intactos ao salvar no `Concatbr`, permitindo `vollare concat importar` de volta para quadros inteiros na EDL canônica.

---

## 2. Como compilar e usar no PC do Theo (Windows + RTX)

```powershell
# Compilar o binário desktop + servidor MCP embutido:
cd src
cargo test -p concat-project
cargo build --release -p concat

# Abrir um projeto exportado pelo Super Motor Vollare:
.\target\release\concat.exe "H:\PROJETOS\meu_projeto\concat"

# Rodar como servidor MCP local para o Antigravity Desktop / Claude Code:
.\target\release\concat.exe --mcp
```
