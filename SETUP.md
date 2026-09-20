# Setup

Clone this repository as the maintained pi setup, install dependencies, and copy the global resources:

```sh
git clone https://github.com/Shrxmy/my-pi-configs.git ~/.pi/agent/my-pi-setup
cd ~/.pi/agent/my-pi-setup
npm install

mkdir -p ~/.pi/agent/skills ~/.pi/agent/themes ~/.pi/agent/prompts
cp -R skills/. ~/.pi/agent/skills/
cp -R themes/. ~/.pi/agent/themes/
cp -R prompts/. ~/.pi/agent/prompts/
cp config/AGENTS.md ~/.pi/agent/AGENTS.md
cp config/keybindings.json ~/.pi/agent/keybindings.json
cp settings.example.json ~/.pi/agent/settings.json
```

The settings load extensions and themes from `~/.pi/agent/my-pi-setup` while global skills and prompt templates remain in `~/.pi/agent/skills` and `~/.pi/agent/prompts`.

## Firecrawl

The search, scrape, and crawl tools require a Firecrawl API key. Follow [Firecrawl's Node.js getting-started guide](https://docs.firecrawl.dev/quickstarts/nodejs) to create one, then copy the example environment file:

```sh
cp ~/.pi/agent/my-pi-setup/.env.example ~/.pi/agent/.env
```

Replace the placeholder in `~/.pi/agent/.env` with your API key. If Firecrawl is not needed, remove its entry from the `extensions` array in `~/.pi/agent/settings.json`.

## fd and rg tools

The `file-search` extension registers `fd` and `rg` as model tools. It uses system-installed binaries when available, then an existing fallback in `~/.pi/agent/bin/`. If neither exists, it downloads a verified official release binary for supported macOS, Linux, and Windows systems.

## Theme

This setup uses the user-level `retro-clean` theme:

```json
{
  "theme": "retro-clean"
}
```

Restart pi after installation so it discovers the updated extensions, skills, and theme.
