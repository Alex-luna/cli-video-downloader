# Changelogs

## 23-02-26 12:06 — Plano: Melhorias no CLI Video Downloader

### 1. Fix ^M na seleção de qualidade
- **cli.py**: substituído `typer.prompt()` por `questionary.select()` em `pick_quality()`
- Menu interativo com setas (↑↓) e Enter — resolve o bug de ^M (carriage return) no terminal
- **requirements.txt**: adicionado `questionary`

### 2. Modo aberto vs modo rápido
- **cli.py**: ramificação em `main()`:
  - `vd url` → modo rápido: baixa e sai
  - `vd` (sem URL) → inicia TUI e fica aberto em loop
- Import dinâmico de `run_tui` quando não há URL

### 3. Migração para Textual (TUI)
- **tui.py** (novo): app Textual com telas:
  - `HomeScreen`: input de URL, botões Baixar/Sair
  - `QualityScreen`: DataTable navegável (↑↓ Enter) para seleção de qualidade
  - `DownloadScreen`: barra de progresso com callback do yt-dlp
  - `ResultScreen`: sucesso + botão "Novo download" (loop)
- Extração de `do_download()` em **cli.py** para reuso (CLI + TUI)
- `_download_with_progress` aceita `progress_callback` opcional para TUI
- **requirements.txt**: adicionado `textual`

### 4. Modo rápido: URL sem aspas
- Documentado: URLs simples (`youtu.be/xxx`) funcionam sem aspas
- URLs com `&` ou `?` exigem aspas no shell
- Nenhuma alteração no parsing — já aceita URL como argumento posicional

### 5. Docs
- **README.md**: features, uso (modo rápido/TUI), exemplos com/sem aspas, alias
- **USAGE.md**: atualizado alias e FAQ de qualidade
- Arquitetura: adicionado `tui.py`, `.venv` no lugar de `venv`

### Arquivos alterados
- `video-downloader/video_downloader/cli.py`: pick_quality, main, download, do_download, _download_with_progress
- `video-downloader/video_downloader/tui.py`: novo
- `video-downloader/requirements.txt`: questionary, textual
- `video-downloader/README.md`, `video-downloader/USAGE.md`

---

## 23-02-26 12:10 — Fix: vd só ativava venv, não rodava o app

- **vd** (novo): script na raiz que executa o app (cd video-downloader, source .venv, python -m video_downloader)
- **README.md**: instruções com ./vd e alias correto (apontar para o script, não só venv)

---

## 23-02-26 19:06 — Fix: thumbnails YouTube + feedback claro na CLI

- **download_thumbnail**: fallback maxresdefault → hqdefault (maxresdefault retorna 404 em vídeos antigos)
- Headers User-Agent para evitar bloqueio
- Retorno estruturado: ("ok"|"fallback"|"error"|"not_found", path|msg)
- **CLI**: mensagens explícitas — ✓ Thumbnail baixada (HD/padrão), ✗ Erro, ⚠ Não encontrada
- **TUI ResultScreen**: exibe status da thumbnail
- **do_download**: passa a retornar (output_path, folder, thumbnail_result)

---

## 23-02-26 19:15 — Thumbnails: 10 camadas de fallback + qual foi usada

- **THUMBNAIL_SOURCES**: 10 fontes em ordem (maxresdefault → 0 → sddefault → hqdefault → mqdefault → 1/2/3 → default)
- **download_thumbnail**: tenta todas; retorna (quality_key, path) com qual funcionou
- **CLI/TUI**: exibe qual qualidade foi baixada (ex: "máxima resolução (1280px)", "HQ (480px)")
- Se nenhuma funcionar: "nenhuma das 10 fontes disponíveis funcionou"
- **youtube-thumbnail-url.md**: documentada ordem de fallback
