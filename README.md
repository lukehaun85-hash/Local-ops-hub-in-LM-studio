# PIP-BOY Hub

A small Node.js + Express local-ops hub with a Pip-Boy themed dashboard, an extendable node registry, a file janitor node, and a research digest node backed by LM Studio.

## 1. Prerequisites

- Windows 11
- Node.js 20+ (Node 22 LTS is a good choice)
- VS Code
- LM Studio with at least one model downloaded

## 2. Start LM Studio

Open LM Studio → Developer → start the local server.

The default OpenAI-compatible base URL is `http://localhost:1234/v1`.

## 3. Setup in VS Code

Open this folder in VS Code, then in the integrated terminal:

```powershell
copy .env.example .env
npm install
npm run dev
```

Open `http://localhost:3000`.

In `.env`, set `LM_STUDIO_MODEL` if you want to pin a model ID. Leave it blank to use the first model reported by LM Studio.

Set `JANITOR_ROOT` to the folder you want the File Janitor to scan. The UI can override it for an individual run.

## 4. Nodes

Each node is just a module with metadata and a `run(input, context)` function:

```js
export const myNode = {
  id: 'my-node',
  name: 'My Node',
  description: 'What it does',
  inputSchema: { /* UI-facing metadata */ },
  async run(input, context) {
    return { ok: true };
  }
};
```

Register it in `src/server.js`:

```js
registerNode(myNode);
```

The registry lives in `src/registry.js`, and the HTTP contract is:

- `GET /api/nodes`
- `POST /api/nodes/:id/run`
- `GET /api/health`

## 5. Safety note

The File Janitor is preview-only by default. Check **APPLY DELETIONS (DANGEROUS)** before deletion is performed. This is intentionally a simple first-pass janitor, not a production backup/recovery system.
