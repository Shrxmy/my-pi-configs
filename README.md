# my-pi-configs

Personal configuration and extensions for [pi](https://pi.dev), including a retro terminal theme and a crescent-moon startup header.

## Included

- Custom pi extensions and skills
- `themes/retro.json` and `themes/retro-clean.json`
- Windows-compatible `fd`/`rg` setup
- `settings.example.json` with the current extension and theme configuration

## Install

Clone this repository as the pi setup directory:

```bash
git clone <your-repository-url> ~/.pi/agent/my-pi-setup
cd ~/.pi/agent/my-pi-setup
npm install
```

Copy `settings.example.json` to `~/.pi/agent/settings.json` only if you are creating a new pi configuration. Merge it with an existing settings file to preserve personal preferences.

## Push to your account

```bash
git remote add origin <your-repository-url>
git push -u origin main
```

Do not commit `auth.json`, `.env` files, sessions, or other private runtime data. The repository `.gitignore` excludes these by default.
