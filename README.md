# Video Downloader

Uma aplicação CLI elegante e simples para baixar vídeos do YouTube, Vimeo e Loom, organizando-os automaticamente em pastas organizadas.

## Features

- ✓ Baixar vídeos do YouTube, Vimeo e Loom
- ✓ TUI com Textual (interface interativa no terminal)
- ✓ Modo rápido: `vd url` — baixa e sai
- ✓ Modo aberto: `vd` — fica rodando para múltiplos downloads
- ✓ Seleção de qualidade via tabela navegável (↑↓ Enter)
- ✓ Detecção automática de plataforma
- ✓ Extração automática de título do vídeo
- ✓ Organização em pastas por plataforma e data
- ✓ Abertura automática da pasta no Finder após download

## Instalação

### Pré-requisitos

- Python 3.8+
- Homebrew (para instalar yt-dlp e ffmpeg)
- **FFmpeg** (necessário para máxima qualidade – merge de vídeo+áudio): `brew install ffmpeg`

### Setup

1. **Clone ou baixe o projeto:**
```bash
cd ~/Projects/video-downloader
```

2. **Crie um ambiente virtual:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. **Instale as dependências:**
```bash
pip install -r requirements.txt
```

4. **Instale o yt-dlp via Homebrew (recomendado):**
```bash
brew install yt-dlp
```

## Uso

### Modo rápido (baixa e sai)
```bash
python -m video_downloader https://youtu.be/ABC123
```
URLs simples (ex: youtu.be/xxx) não precisam de aspas. URLs com `&` ou `?` exigem aspas: `vd "https://youtube.com/watch?v=xxx&list=yyy"`

### Modo TUI (fica aberto)
```bash
python -m video_downloader
```
Abre a interface Textual: cole a URL, escolha qualidade (↑↓ Enter) e baixe. Após cada download, pode baixar outro sem fechar.

### Com título customizado
```bash
python -m video_downloader https://youtu.be/xxx --title "Meu Vídeo"
```

### Criar um alias (opcional)
Adicione ao `~/.zshrc` ou `~/.bash_profile`:
```bash
alias vd='cd ~/path/to/video-downloader && source .venv/bin/activate && python -m video_downloader'
```
Uso: `vd` (TUI) ou `vd https://url` (modo rápido).

## Estrutura de Pastas

Os vídeos são organizados automaticamente em:

```
~/Library/Mobile Documents/com~apple~CloudDocs/Organized/Videos/video-downloader/
├── youtube/
│   ├── 2026-02-21/
│   │   └── Video Title.mp4
│   └── 2026-02-20/
├── vimeo/
│   └── 2026-02-21/
└── loom/
    └── 2026-02-21/
```

## Arquitetura

```
video-downloader/
├── video_downloader/
│   ├── __init__.py          # Inicialização do pacote
│   ├── __main__.py          # Entrada para 'python -m'
│   ├── cli.py               # CLI e lógica de download
│   ├── tui.py               # Interface Textual (TUI)
│   └── config.py            # Configurações
├── .venv/                   # Ambiente virtual (criar com python -m venv .venv)
├── requirements.txt         # Dependências
├── setup.sh                 # Script de instalação
├── video-downloader         # Script executável
└── README.md                # Este arquivo
```

## Dependências

- **Rich**: Formatação no terminal
- **Typer**: Framework CLI
- **Textual**: TUI interativa
- **Questionary**: Prompts interativos (seleção de qualidade no modo rápido)
- **yt-dlp**: Download de vídeos

## Como Contribuir

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -am 'Add nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## Próximas Melhorias

- [ ] Suporte a download de playlists
- [ ] Sincronização com Baserow
- [ ] Suporte a Windows e Linux
- [ ] Progress bar mais detalhada
- [ ] Configurações persistentes

## Licença

MIT

## Suporte

Abra uma issue no GitHub: https://github.com/anomalyco/opencode/issues
