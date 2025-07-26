# Architecture Overview

This repository hosts a prebuilt Node.js CLI that interacts with a local Ollama server to power Agentis features.

```
sequenceDiagram
    participant User
    participant CLI
    participant Ollama
    User->>CLI: Run `agentis2`
    CLI->>Ollama: start/verify server
    CLI-->>User: interactive session
    CLI->>Ollama: stop server on exit
```

The `cli.mjs` file is produced from internal TypeScript sources and includes logic to bootstrap `ollama serve` when executed.
