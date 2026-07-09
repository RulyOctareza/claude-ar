# claude-ar

**Claude Code + AgentRouter** — route Claude Code CLI melalui [AgentRouter](https://agentrouter.org) untuk akses multi-model (Claude Opus, GLM, GPT) tanpa batasan region/billing Anthropic langsung.

## Kenapa?

- Claude Code cuma support Anthropic API natively
- AgentRouter jadi proxy yang nerjemahin Anthropic API calls ke berbagai provider
- Script ini bungkus Claude Code supaya connect ke AgentRouter — dapat Claude Opus 4.8 + fallback model lain
- Satu API key, multi model, no vendor lock-in

## Model Mapping

| Claude Code Model | Diresolve ke |
|------------------|-------------|
| `claude-opus-4-8` | Claude Opus 4.8 (Anthropic) |
| `claude-sonnet-5` | `glm-5.2` (Zhipu GLM) |
| `claude-haiku-4-5` | `gpt-5.5` (OpenAI) |

Support semua model yang ada di AgentRouter dashboard — edit script kalau mau ganti mapping.

## Prasyarat

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) CLI terinstal (`claude` command tersedia)
- Akun [AgentRouter](https://agentrouter.org) + API key aktif
- `bash` atau `zsh`

## Instalasi

### 1. Clone repo

```bash
git clone https://github.com/RulyOctareza/claude-ar.git
cd claude-ar
```

### 2. Install script ke PATH

```bash
# Copy ke local bin
cp claude-ar ~/.local/bin/claude-ar
chmod +x ~/.local/bin/claude-ar

# Pastikan ~/.local/bin ada di PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc   # zsh
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc  # bash
source ~/.zshrc  # atau source ~/.bashrc
```

### 3. Setup API key

```bash
# Copy template ke config directory
mkdir -p ~/.config/agentrouter
cp key.env.example ~/.config/agentrouter/key.env
chmod 600 ~/.config/agentrouter/key.env

# Edit dan paste API key dari https://agentrouter.org
nano ~/.config/agentrouter/key.env
# atau
vim ~/.config/agentrouter/key.env
```

Format `key.env`:
```bash
export AGENTROUTER_API_KEY="sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

### 4. Verifikasi

```bash
claude-ar --version
# Output: Claude Code vX.Y.Z ...
```

## Penggunaan

```bash
# CLI interaktif (default, hooks aktif — RTK, plugins, skills)
claude-ar

# Pipe prompt langsung
echo "jelasin file ini" | claude-ar

# Mode bare — tanpa hooks/plugins, lebih cepat
claude-ar --bare

# Flag --with-hooks (eksplisit, default behavior)
claude-ar --with-hooks
```

Semua flag Claude Code standard tetap jalan:
```bash
claude-ar --model claude-sonnet-5
claude-ar --continue
claude-ar -p "fix bug di auth middleware"
```

## Struktur File

```
~/.config/agentrouter/
├── key.env                  # API key (private, jangan di-commit)
├── key.env.example          # Template (dari repo ini)
└── claude-ar-settings.json  # Auto-generated runtime settings
```

## Keamanan

- `key.env` permission otomatis `600` — cuma user yang bisa baca
- `claude-ar-settings.json` di-generate saat runtime — tidak pernah disimpan di repo
- `.gitignore` exclude `key.env` dan `claude-ar-settings.json`

## Troubleshooting

**`claude-ar: command not found`**
Pastikan `~/.local/bin` ada di PATH. Jalankan `echo $PATH | grep .local/bin`.

**`AGENTROUTER_API_KEY belum diisi`**
Edit `~/.config/agentrouter/key.env` dan paste API key dari dashboard AgentRouter.

**`claude: command not found`**
Install Claude Code dulu: `npm install -g @anthropic-ai/claude-code`

**Rate limit / timeout**
Cek kredit di dashboard AgentRouter. Bisa juga ganti model mapping di script.

## License

MIT © 2026 Fatchurizkia Ruly Octareza
