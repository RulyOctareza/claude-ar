<p align="center">
  <img src="docs/preview/hero.svg" alt="claude-ar — ilustrasi CLI dengan routing multi-model" width="100%">
</p>

<h1 align="center">claude-ar</h1>

<p align="center">
  <b>Claude Code CLI lewat AgentRouter — satu API key, banyak model.</b><br>
  Bash wrapper · tanpa vendor lock-in
</p>

<p align="center">
  <img alt="Shell" src="https://img.shields.io/badge/Shell-bash%20%7C%20zsh-4EAA25?logo=gnubash&logoColor=white">
  <img alt="Claude Code" src="https://img.shields.io/badge/Claude%20Code-CLI-D97757">
  <img alt="License" src="https://img.shields.io/badge/License-MIT-yellow">
</p>

---

## ✨ Preview

<p align="center">
  <img src="docs/preview/hero.png" alt="Ilustrasi claude-ar" width="90%">
</p>

> **Catatan:** gambar di atas adalah *ilustrasi konsep*, bukan screenshot asli.

---

## 📖 Tentang

Claude Code secara native hanya mendukung Anthropic API. Script ini membungkus Claude Code CLI
agar terhubung ke [AgentRouter](https://agentrouter.org), yang menerjemahkan panggilan API
Anthropic ke berbagai provider — jadi kamu dapat Claude Opus plus fallback model lain dengan
satu API key.

---

## 🔀 Model Mapping

| Claude Code Model | Diresolve ke |
|---|---|
| `claude-opus-4-8` | Claude Opus 4.8 (Anthropic) |
| `claude-sonnet-5` | `glm-5.2` (Zhipu GLM) |
| `claude-haiku-4-5` | `gpt-5.5` (OpenAI) |

Semua model yang ada di dashboard AgentRouter bisa dipakai — tinggal edit mapping di script.

---

## 📦 Prasyarat

- Claude Code CLI terpasang (`claude` tersedia di PATH)
- Akun [AgentRouter](https://agentrouter.org) + API key aktif
- `bash` atau `zsh`

---

## 🚀 Instalasi

```bash
git clone https://github.com/RulyOctareza/claude-ar.git
cd claude-ar

# pasang ke PATH
mkdir -p ~/.local/bin
cp claude-ar ~/.local/bin/claude-ar
chmod +x ~/.local/bin/claude-ar
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Setup API key:

```bash
mkdir -p ~/.config/agentrouter
cp key.env.example ~/.config/agentrouter/key.env
chmod 600 ~/.config/agentrouter/key.env
# paste API key dari dashboard AgentRouter
```

Verifikasi:

```bash
claude-ar --version
```

---

## 💻 Penggunaan

```bash
claude-ar                          # sesi interaktif (hooks aktif)
echo "jelasin file ini" | claude-ar
claude-ar --bare                   # tanpa hooks/plugins
claude-ar --model claude-sonnet-5
claude-ar -p "fix bug di auth middleware"
```

---

## 🔒 Keamanan

- `key.env` diset permission `600`
- `claude-ar-settings.json` di-generate saat runtime, tidak pernah masuk repo
- `.gitignore` mengecualikan `key.env` dan settings runtime

---

## 🛠️ Troubleshooting

| Masalah | Solusi |
|---|---|
| `claude-ar: command not found` | Pastikan `~/.local/bin` ada di `PATH` |
| `AGENTROUTER_API_KEY belum diisi` | Isi `~/.config/agentrouter/key.env` |

---

## 📄 Lisensi

MIT
