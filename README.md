# Agentis CLI (Ollama Edition)

This repo contains a prebuilt command line interface for experimenting with **Agentis**, an AI‑powered development assistant. The bundled `cli.mjs` communicates with a local [Ollama](https://ollama.ai/) server to provide interactive coding help directly in the terminal.

## What it does

When executed, the CLI automatically ensures that `ollama serve` is running. If the server is not found, it attempts to launch it, waits for the API to become responsive, and gracefully terminates the process on exit. This behaviour is highlighted in [`cli.mjs`](cli.mjs). You can see the startup logic below:

```javascript
execSync("pgrep -f 'ollama serve' || ollama serve &", { stdio: 'ignore' });
execSync("open -a Ollama", { stdio: 'ignore' });
let attempts = 10;
while (attempts--) {
  try {
    execSync("curl -s http://localhost:11434/version", { stdio: 'ignore' });
    break;
  } catch (e) {
    execSync("sleep 0.5");
  }
}
```

The script also cleans up with:

```javascript
process.on('exit', () => {
  try {
    execSync("pkill -f 'ollama serve'", { stdio: 'ignore' });
    console.log("🛑 Ollama server killed on exit.");
  } catch (e) {
    // Silent fail
  }
});
```

## Installation

This package targets **Node.js 18+**. Install dependencies and run the CLI with:

```bash
npm install
default_entry=node cli.mjs
node $default_entry
```

Running `node cli.mjs` will attempt to start `ollama serve` locally. If Ollama is not installed, the process will exit with an error.

## Usage

Once Ollama is running, the CLI provides an interactive session where Agentis can inspect code, modify files and execute shell commands. See [Agentis documentation](https://github.com/agentislabs/agentis) for the full feature set.

## Repository layout

- `cli.mjs` – precompiled entry point
- `yoga.wasm` – dependency for text layout rendering
- `package.json` – npm package metadata
- `docs/` – architecture notes

## Next steps

The underlying TypeScript sources are not included here. Future work might expose the source, add tests and extend documentation.

