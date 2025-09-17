# Gemini CLI file system tools

The Gemini CLI provides a comprehensive suite of tools for interacting with the local file system. These tools allow the Gemini model to read from, write to, list, search, and modify files and directories, all under your control and typically with confirmation for sensitive operations.

**Note:** All file system tools operate within a `rootDirectory` (usually the current working directory where you launched the CLI) for security. Paths that you provide to these tools are generally expected to be absolute or are resolved relative to this root directory.

## 1. `list_directory` (ReadFolder)

`list_directory` lists the names of files and subdirectories directly within a specified directory path. It can optionally ignore entries matching provided glob patterns.

- **Tool name:** `list_directory`
- **Display name:** ReadFolder
- **File:** `ls.ts`
- **Parameters:**
  - `path` (string, required): The absolute path to the directory to list.
  - `ignore` (array of strings, optional): A list of glob patterns to exclude from the listing (e.g., `["*.log", ".git"]`).
  - `respect_git_ignore` (boolean, optional): Whether to respect `.gitignore` patterns when listing files. Defaults to `true`.
- **Behavior:**
  - Returns a list of file and directory names.
  - Indicates whether each entry is a directory.
  - Sorts entries with directories first, then alphabetically.
- **Output (`llmContent`):** A string like: `Directory listing for /path/to/your/folder:\n[DIR] subfolder1\nfile1.txt\nfile2.png`
- **Confirmation:** No.

## 2. `read_file` (ReadFile)

`read_file` reads and returns the content of a specified file. This tool handles text, images (PNG, JPG, GIF, WEBP, SVG, BMP), and PDF files. For text files, it can read specific line ranges. Other binary file types are generally skipped.

- **Tool name:** `read_file`
- **Display name:** ReadFile
- **File:** `read-file.ts`
- **Parameters:**
  - `path` (string, required): The absolute path to the file to read.
  - `offset` (number, optional): For text files, the 0-based line number to start reading from. Requires `limit` to be set.
  - `limit` (number, optional): For text files, the maximum number of lines to read. If omitted, reads a default maximum (e.g., 2000 lines) or the entire file if feasible.
- **Behavior:**
  - For text files: Returns the content. If `offset` and `limit` are used, returns only that slice of lines. Indicates if content was truncated due to line limits or line length limits.
  - For image and PDF files: Returns the file content as a base64-encoded data structure suitable for model consumption.
  - For other binary files: Attempts to identify and skip them, returning a message indicating it's a generic binary file.
- **Output:** (`llmContent`):
  - For text files: The file content, potentially prefixed with a truncation message (e.g., `[File content truncated: showing lines 1-100 of 500 total lines...]\nActual file content...`).
  - For image/PDF files: An object containing `inlineData` with `mimeType` and base64 `data` (e.g., `{ inlineData: { mimeType: 'image/png', data: 'base64encodedstring' } }`).
  - For other binary files: A message like `Cannot display content of binary file: /path/to/data.bin`.
- **Confirmation:** No.

## 3. `write_file` (WriteFile)

`write_file` writes content to a specified file. If the file exists, it will be overwritten. If the file doesn't exist, it (and any necessary parent directories) will be created.

- **Tool name:** `write_file`
- **Display name:** WriteFile
- **File:** `write-file.ts`
- **Parameters:**
  - `file_path` (string, required): The absolute path to the file to write to.
  - `content` (string, required): The content to write into the file.
- **Behavior:**
  - Writes the provided `content` to the `file_path`.
  - Creates parent directories if they don't exist.
- **Output (`llmContent`):** A success message, e.g., `Successfully overwrote file: /path/to/your/file.txt` or `Successfully created and wrote to new file: /path/to/new/file.txt`.
- **Confirmation:** Yes. Shows a diff of changes and asks for user approval before writing.

## 4. `glob` (FindFiles)

`glob` finds files matching specific glob patterns (e.g., `src/**/*.ts`, `*.md`), returning absolute paths sorted by modification time (newest first).

- **Tool name:** `glob`
- **Display name:** FindFiles
- **File:** `glob.ts`
- **Parameters:**
  - `pattern` (string, required): The glob pattern to match against (e.g., `"*.py"`, `"src/**/*.js"`).
  - `path` (string, optional): The absolute path to the directory to search within. If omitted, searches the tool's root directory.
  - `case_sensitive` (boolean, optional): Whether the search should be case-sensitive. Defaults to `false`.
  - `respect_git_ignore` (boolean, optional): Whether to respect .gitignore patterns when finding files. Defaults to `true`.
- **Behavior:**
  - Searches for files matching the glob pattern within the specified directory.
  - Returns a list of absolute paths, sorted with the most recently modified files first.
  - Ignores common nuisance directories like `node_modules` and `.git` by default.
- **Output (`llmContent`):** A message like: `Found 5 file(s) matching "*.ts" within src, sorted by modification time (newest first):\nsrc/file1.ts\nsrc/subdir/file2.ts...`
- **Confirmation:** No.

## 5. `search_file_content` export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_NAME"
gemini- name: Run lint
  run: npm run lint --if-present#!/bin/bash
# ====================================================================
# MASTER BLUEPRINT: AVSD Quantum Reality Ecosystem (Versi Tunggal)
# Disediakan oleh: A.R.I.A.N.A. untuk Datuk Wan Adni
#
# Skrip ini akan membina keseluruhan ekosistem dari sifar dalam satu langkah.
# ====================================================================

set -e

echo "
███████╗ ██████╗ ██╗   ██╗██████╗ 
██╔════╝██╔═══██╗██║   ██║██╔══██╗
█████╗  ██║   ██║██║   ██║██║  ██║
██╔══╝  ██║   ██║██║   ██║██║  ██║
███████╗╚██████╔╝╚██████╔╝██████╔╝
╚══════╝ ╚═════╝  ╚═════╝ ╚═════╝ 
"
echo "MEMULAKAN PEMBINAAN EKOSISTEM AVSD..."
echo "----------------------------------------------------"

# --- PERINGKAT 1: PERSEDIAAN PRASYARAT ---
echo "[FASA 1/5] Memasang Node.js, Python, dan PM2..."
pkg update -y && pkg upgrade -y
pkg install nodejs python termux-api -y
npm install -g pm2
echo "✅ Prasyarat berjaya dipasang."

# --- PERINGKAT 2: PENYEDIAAN STRUKTUR & KONFIGURASI ---
echo "[FASA 2/5] Menyediakan struktur direktori dan fail konfigurasi..."
BASE_DIR="$HOME/avsd-quantum-reality-ecosystem"
rm -rf "$BASE_DIR" # Buang hak lame kalu ado
mkdir -p "$BASE_DIR"/{apps,services,ui,shared,scripts,utils,data,docs}
mkdir -p "$BASE_DIR/apps"/{avsd-nexus-hub,payment-webhook-handler,duitnow-fpx-simulator,quantum-ecosystem-core,qubitchain-miner,chrono-engine-ws}
mkdir -p "$BASE_DIR/apps/quantum-ecosystem-core"/{core,utils,config}
mkdir -p "$BASE_DIR/apps/qubitchain-miner"/models
mkdir -p "$BASE_DIR/services"/{reality-forge-nexus-core/src,reality-forge-energy-hub}
mkdir -p "$BASE_DIR/ui"/{avsd-3d-map,quantum-nexus-ui,avsd-rwp-ui,qubitchain-strategy-ui}

echo "✅ Struktur direktori berjaya dicipta."

cd "$BASE_DIR"

# Fail .env
cat > .env <<'EOF'
# === Konfigurasi Induk Ekosistem AVSD ===

# Kunci API & Rahsia
REALITY_CORE_API_KEY="wan-adni-supremacy-2025-v2"
HUB_API_KEY="change-me-quantum-key"
WEBHOOK_SECRET="your_webhook_secret_here" # Ganti dengan rahsia sebenar
GEMINI_API_KEY="AIzaSyC5BjAQdFV41yZgxHDwc5zh0ad5HinDFdM" # Sila GANTI ini dengan API Key Gemini demo

# Konfigurasi Rangkaian & Port
AVSD_NEXUS_HUB_PORT=3001
WEBHOOK_PORT=5004
DUITNOW_PORT=5005
QUANTUM_ECO_HTTP_PORT=5000
QUANTUM_ECO_WS_PORT=8080
CHRONO_ENGINE_WS_PORT=8082
PORT_ENERGY_HUB=7012
PORT_NEXUS_CORE=3001
PORT_QCLOUD=3002

# Konfigurasi Pangkalan Data
MONGO_URI="mongodb://localhost:27017/qubitchain_db" # Pastikan MongoDB berjalan
DUITNOW_DB="./data/duitnow.db"

# Konfigurasi Kuantum & Tenaga
HUB_NAME="Kelate-Nexus-Forge"
QSHOTS=1024
QUANTUM_AMPLIFIER_ALGORITHM="Advanced_Amplifier_v2"
MAX_CAPACITY_KWH=500000
LOSS_PERCENT=1.2
EOF
echo "✅ Fail .env berjaya ditulis."

# Fail package.json
cat > package.json <<'EOF'
{
  "name": "avsd-quantum-reality-ecosystem",
  "version": "2.0.0",
  "description": "Ekosistem AVSD Lengkap oleh Datuk Wan Adni & A.R.I.A.N.A.",
  "type": "module",
  "private": true,
  "workspaces": [
    "apps/*",
    "services/*"
  ],
  "scripts": {
    "start": "bash scripts/launcher.sh",
    "stop": "pm2 stop all",
    "delete": "pm2 delete all  true",
    "logs": "pm2 logs",
    "install-all": "npm install && pip install -r apps/chrono-engine-ws/requirements.txt && pip install -r services/reality-forge-nexus-core/src/requirements.txt"
  },
  "dependencies": {
    "axios": "^1.6.8",
    "cors": "^2.8.5",
    "crypto-js": "^4.2.0",
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "morgan": "^1.10.0",
    "ws": "^8.16.0"
  },
  "devDependencies": {
    "pm2": "^5.3.1"
  }
}
EOF
echo "✅ Fail package.json (akar) berjaya ditulis."

# Fail ecosystem.config.js
cat > ecosystem.config.js <<'EOF'
// Konfigurasi PM2 untuk semua perkhidmatan Node.js
require('dotenv').config();

module.exports = {
  apps: [
    // Servis dari Projek Reality Forge
    {
      name: 'reality-forge-nexus-core',
      script: 'services/reality-forge-nexus-core/index.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    {
      name: 'reality-forge-energy-hub',
      script: 'services/reality-forge-energy-hub/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    // Servis dari Ekosistem Quantum
    {
      name: 'avsd-nexus-hub',
      script: 'apps/avsd-nexus-hub/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    {
      name: 'payment-webhook-handler',
      script: 'apps/payment-webhook-handler/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    {
      name: 'duitnow-fpx-simulator',
      script: 'apps/duitnow-fpx-simulator/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    {
      name: 'quantum-ecosystem-core',
      script: 'apps/quantum-ecosystem-core/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    },
    {
      name: 'qubitchain-miner',
      script: 'apps/qubitchain-miner/server.js',
      instances: 1,
      exec_mode: 'fork',
      env: { NODE_ENV: 'production' }
    }
  ]
};
EOF
echo "✅ Fail ecosystem.config.js (PM2) berjaya ditulis."
echo "----------------------------------------------------"

# --- PERINGKAT 3: PENULISAN KOD PERKHIDMATAN (SERVICES & APPS) ---
echo "[FASA 3/5] Menulis kod untuk semua perkhidmatan dan aplikasi..."

cat > services/reality-forge-nexus-core/index.js <<'EOF'
import "dotenv/config";
import express from "express";
import cors from "cors";
import path from 'path';
import { fileURLToPath } from 'url';
import { WebSocketServer } from "ws";
import { spawn } from 'child_process';
import { loadState, saveState } from "../../utils/fileStore.js";
import { geminiInterpret } from "../../utils/ai.js";

const filename = fileURLToPath(import.meta.url);
const dirname = path.dirname(filename);

const app = express();
const PORT = Number(process.env.PORT_NEXUS_CORE || 3001);
const state = loadState({ intentions_received: 0, status: "offline" }, 'nexus_core_state.json');
const wss = new WebSocketServer({ noServer: true });

function broadcast(obj) {
  const s = JSON.stringify(obj);
  for (const ws of wss.clients) {
    if (ws.readyState === ws.OPEN) ws.send(s);
  }
}

app.use(cors());
app.use(express.json());

app.post("/api/intent", async (req, res) => {
  try {
    const { niat, user_id, timestamp, type } = req.body || {};
    if (!niat || !user_id) {
      return res.status(400).json({ ok: false, message: "Medan 'niat' dan 'user_id' diperlukan." });
    }
    console.log(`[NEXUS CORE] Menerima niat dari ${user_id}: \"${niat}\"`);

    const amplification_result = await new Promise((resolve, reject) => {
      const pythonScriptPath = path.join(dirname, 'src/qamplifier.py');
      const py = spawn('python3', [pythonScriptPath]);
      let stdoutData = '';
      py.stdout.on('data', (d) => stdoutData += d.toString());
      py.on('close', (code) => {
        if (code !== 0) return reject(new Error(`Proses Python ditamatkan dengan kod ${code}`));
        try {
          resolve(JSON.parse(stdoutData));
        } catch (e) {
          reject(new Error(`Gagal mem-parse output JSON dari Python: ${stdoutData}`));
        }
      });
    });

    const interpretation = await geminiInterpret(`Hasil quantum amplification: ${JSON.stringify(amplification_result)}`);
    const eventData = { id: `INTENT-${state.intentions_received + 1}`, niat, user_id, timestamp, type, amplification_result, interpretation };
    state.intentions_received++;
    saveState(state, 'nexus_core_state.json');
    broadcast({ type: "newIntent", event: eventData });
    res.json({ ok: true, message: "Niat berjaya di-amplify!", event: eventData });
  } catch (error) {
    console.error('[NEXUS CORE] Ralat:', error);
    res.status(500).json({ ok: false, message: 'Ralat Dalaman Pelayan', error: error.message });
  }
});

const server = app.listen(PORT, () => {
  state.status = "online";
  saveState(state, 'nexus_core_state.json');
  console.log(`[NEXUS CORE] Reality Forge berjalan di port ${PORT}`);
});
server.on("upgrade", (req, socket, head) => wss.handleUpgrade(req, socket, head, (ws) => wss.emit("connection", ws, req)));
EOF

cat > services/reality-forge-nexus-core/src/qamplifier.py <<'EOF'
import json
import os
import random
import time
from qiskit import QuantumCircuit, Aer, execute

try:
    shots = int(os.getenv('QSHOTS', 1024))
    algorithm_name = os.getenv('QUANTUM_AMPLIFIER_ALGORITHM', "Basic_Amplifier")

    qc = QuantumCircuit(2, 2)
    qc.h(0)
    qc.cx(0, 1)
    if random.random() < 0.5:
        qc.x(0)
        qc.x(1)
    qc.measure([0, 1], [0, 1])

    backend = Aer.get_backend('qasm_simulator')
    job = execute(qc, backend, shots=shots)
    counts = job.result().get_counts()

    print(json.dumps({
        "algorithm": algorithm_name,
        "result_counts": counts,
        "timestamp": time.time()
    }))
except Exception as e:
    print(json.dumps({"error": str(e)}))
    exit(1)
EOF

cat > services/reality-forge-nexus-core/src/requirements.txt <<'EOF'
qiskit
EOF

cat > services/reality-forge-energy-hub/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import cors from "cors";
import morgan from "morgan";
import { WebSocketServer } from "ws";
import { toKWh } from "../../utils/utils.js";
import { calculateStoreEnergy, calculateDispatchEnergy } from "../../utils/storage.js";
import { loadState, saveState } from "../../utils/fileStore.js";

const app = express();
const PORT = Number(process.env.PORT_ENERGY_HUB || 7012);
const HUB_NAME = process.env.HUB_NAME || "Kelate-Nexus-Forge";
const BANK_STATE_FILE = 'energy_hub_state.json';

const bank = loadState({
  capacity_kwh: Number(process.env.MAX_CAPACITY_KWH || 500000),
  soc_kwh: 0,
  loss_percent: Number(process.env.LOSS_PERCENT || 1.2)
}, BANK_STATE_FILE);

app.use(cors());
app.use(express.json({ limit: "1mb" }));
app.use(morgan("dev"));

const wss = new WebSocketServer({ noServer: true });
wss.on("connection", (ws) => ws.send(JSON.stringify({ type: "hello", hub: HUB_NAME, soc_kwh: bank.soc_kwh })));
function broadcast(obj) {
  const s = JSON.stringify(obj);
  for (const ws of wss.clients) {
    if (ws.readyState === ws.OPEN) ws.send(s);
  }
}

app.get("/status", (req, res) => res.json(bank));

app.post("/ingest", (req, res) => {
  try {
    const { amount, unit = "kWh", source = "unknown" } = req.body || {};
    const kwh = toKWh(Number(amount), unit);
    const r = calculateStoreEnergy(kwh, bank);
    bank.soc_kwh = r.new_soc_kwh;
    saveState(bank, BANK_STATE_FILE);
    const event = { type: "ingest", source, amount, kwh, result: { ...r, soc_kwh: bank.soc_kwh }, ts: Date.now() };
    broadcast(event);
    res.json({ ok: true, ...event.result });
  } catch (e) {
    res.status(400).json({ ok: false, msg: e.message });
  }
});

app.post("/dispatch-forge", async (req, res) => {
  try {
    const { request_kwh } = req.body || {};
    const r = calculateDispatchEnergy(Number(request_kwh), bank);
    bank.soc_kwh = r.new_soc_kwh;
    saveState(bank, BANK_STATE_FILE);
    const event = { type: "dispatch", payload: req.body, result: { ...r, soc_kwh: bank.soc_kwh }, ts: Date.now() };
    broadcast(event);
    res.json({ ok: true, ...event.result });
  } catch (e) {
    res.status(500).json({ ok: false, msg: e.message });
  }
});

const server = app.listen(PORT, () => console.log(`[ENERGY HUB] ${HUB_NAME} beroperasi di port :${PORT}`));
server.on("upgrade", (req, socket, head) => wss.handleUpgrade(req, socket, head, (ws) => wss.emit("connection", ws, req)));
EOF

cat > apps/chrono-engine-ws/chrono_engine.py <<'EOF'
import asyncio
import websockets
import json
import os

PORT = int(os.getenv('CHRONO_ENGINE_WS_PORT', 8082))
current_dimension = "Alpha-1"

async def handler(websocket, path):
    global current_dimension
    print(f"Chrono Engine: Klien disambungkan dari {websocket.remote_address}")
    await websocket.send(json.dumps({"type": "status", "data": {"status": "Online", "current_dimension": current_dimension}}))
    try:
        async for message in websocket:
            command = json.loads(message)
            if command.get("action") == "jump_to_dimension":
                target = command.get("target", "Unknown")
                print(f"Chrono Engine: Menerima arahan lompat ke dimensi: {target}")
                await asyncio.sleep(2)  # Simulasikan proses lompatan
                current_dimension = target
                response = {"status": "success", "data": {"status": "Online", "current_dimension": current_dimension}}
                await websocket.send(json.dumps(response))
    except websockets.exceptions.ConnectionClosed:
        print(f"Chrono Engine: Klien terputus.")

async def main():
    async with websockets.serve(handler, "0.0.0.0", PORT):
        print(f"Chrono Engine WebSockets berjalan di port {PORT}")
        await asyncio.Future()  # Jalankan tanpa henti

if __name__ == "__main__":
    asyncio.run(main())
EOF

cat > apps/chrono-engine-ws/requirements.txt <<'EOF'
websockets
EOF

# Fail utiliti
cat > utils/utils.js <<'EOF'
import CryptoJS from "crypto-js";
export function toKWh(value, unit = "kWh") {
  if (isNaN(value)) throw new Error("Nilai 'amount' mesti nombor.");
  const u = unit.toLowerCase();
  if (u === "kwh") return value;
  if (u === "wh") return value / 1000;
  if (u === "mj") return value / 3.6;
  if (u === "j") return value / 3.6e6;
  throw new Error(`Unit tidak disokong: ${unit}`);
}
export function hmacOk(bodyString, headerSig, secret) {
  if (!headerSig || !secret) return false;
  const h = CryptoJS.HmacSHA256(bodyString, secret).toString();
  return h === headerSig;
}
EOF

cat > utils/storage.js <<'EOF'
export function calculateStoreEnergy(kwh, bank) {
  const loss = kwh * (bank.loss_percent / 100);
  const net = Math.max(0, kwh - loss);
  const space = bank.capacity_kwh - bank.soc_kwh;
  const put = Math.min(net, space);
  const new_soc_kwh = bank.soc_kwh + put;
  return { accepted_kwh: put, lost_kwh: loss, new_soc_kwh };
}
export function calculateDispatchEnergy(request_kwh, bank) {
  const loss = request_kwh * (bank.loss_percent / 100);
  const need = request_kwh + loss;
  const give = Math.min(bank.soc_kwh, need);
  const new_soc_kwh = bank.soc_kwh - give;
  const delivered = Math.max(0, give - loss);
  return { delivered_kwh: delivered, deducted_kwh: give, lost_kwh: loss, new_soc_kwh };
}
EOF

cat > utils/fileStore.js <<'EOF'
import fs from 'fs';
import path from 'path';
const DATA_DIR = path.join(path.resolve(), 'data');
export function loadState(defaultState, filename = 'state.json') {
  const file = path.join(DATA_DIR, filename);
  try {
    if (fs.existsSync(file)) {
      const fileContent = fs.readFileSync(file, 'utf8');
      return JSON.parse(fileContent);
    }
  } catch (e) { console.error(`[FileStore] Ralat memuatkan keadaan dari ${filename}:`, e); }
  return defaultState;
}
export function saveState(state, filename = 'state.json') {
  const file = path.join(DATA_DIR, filename);
  try {
    fs.mkdirSync(path.dirname(file), { recursive: true });
    fs.writeFileSync(file, JSON.stringify(state, null, 2));
  } catch (e) { console.error(`[FileStore] Ralat menyimpan keadaan ke ${filename}:`, e); }
}
EOF

cat > utils/ai.js <<'EOF'
import axios from 'axios';
const API_KEY = process.env.GEMINI_API_KEY;

export async function geminiInterpret(text) {
  if (!API_KEY) {
    console.error("GEMINI_API_KEY tidak ditetapkan dalam fail .env");
    return "[Tafsiran AI] Ralat: Kunci API tidak disetkan.";
  }
  try {
    const response = await axios.post(
      `https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=${API_KEY}`,
      {
        contents: [{ parts: [{ text: `Tafsiran niat: ${text}` }] }]
      }
    );
    const result = response.data.candidates[0].content.parts[0].text;
    return `[Tafsiran AI] ${result}`;
  } catch (error) {
    console.error("Error calling Gemini API:", error.response?.data || error.message);
    return "[Tafsiran AI] Ralat dalam analisis niat.";
  }
}
EOF

cat > apps/avsd-nexus-hub/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import cors from "cors";
import morgan from "morgan";
import axios from "axios";
import { loadState, saveState } from "../../utils/fileStore.js";

const app = express();
const PORT = process.env.AVSD_NEXUS_HUB_PORT || 3001;
const REALITY_CORE_URL = `http://localhost:${process.env.PORT_NEXUS_CORE}/api/intent`;

const state = loadState({ events: [], last_event_id: 0 }, 'nexus_hub_state.json');

app.use(cors());
app.use(express.json());
app.use(morgan("dev"));

app.post("/submit-intention", async (req, res) => {
  try {
    const { niat, user_id, type = "voice" } = req.body || {};
    const timestamp = Date.now();
    
    if (!niat || !user_id) {
      return res.status(400).json({ ok: false, message: "Niat dan ID pengguna diperlukan." });
    }

    const core_res = await axios.post(REALITY_CORE_URL, {
      niat,
      user_id,
      timestamp,
      type
    }, {
      headers: {
        'X-API-Key': process.env.REALITY_CORE_API_KEY
      }
    });

    if (core_res.data.ok) {
      const event = core_res.data.event;
      state.events.push(event);
      state.last_event_id = event.id;
      saveState(state, 'nexus_hub_state.json');
      console.log(`[HUB] Niat #${event.id} dari ${user_id} berjaya dihantar.`);
      res.json({ ok: true, message: "Niat berjaya di-amplify dan diproses.", event });
    } else {
      res.status(500).json({ ok: false, message: "Ralat dari Reality Core." });
    }
  } catch (error) {
    console.error("[HUB] Ralat menghantar niat:", error);
    res.status(500).json({ ok: false, message: 'Ralat Dalaman Pelayan.', error: error.message });
  }
});

app.get("/status", (req, res) => {
    res.json({ ok: true, last_event_id: state.last_event_id, events_count: state.events.length });
});

app.listen(PORT, () => console.log(`[HUB] AVSD Nexus Hub berjalan di port ${PORT}`));
EOF

cat > apps/payment-webhook-handler/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import helmet from "helmet";
import morgan from "morgan";
import ipaddr from "ipaddr.js";
import getRawBody from "raw-body";
import crypto from "crypto";
import { hmacOk } from "../../utils/utils.js";

const app = express();
const PORT = Number(process.env.WEBHOOK_PORT || 5004);
const WEBHOOK_SECRET = process.env.WEBHOOK_SECRET || "change-this";
const TRUSTED_IPS = (process.env.TRUSTED_IPS || "").split(",").map(s => s.trim()).filter(Boolean);
const MAX_SKEW_MS = 5 * 60 * 1000;

app.use(helmet());
app.use(morgan("combined"));

app.set("trust proxy", process.env.TRUST_PROXY === "true");

app.use(async (req, res, next) => {
  try {
    if (req.method === "POST" || req.method === "PUT" || req.method === "PATCH") {
      req.rawBody = (await getRawBody(req)).toString("utf8");
      try { req.body = JSON.parse(req.rawBody || "{}"); } catch { req.body = {}; }
    }
    next();
  } catch (e) {
    res.status(400).json({ message: "Failed to read body" });
  }
});

function isTrustedIP(ip) {
  try {
    const addr = ipaddr.parse(ip);
    return TRUSTED_IPS.some(t => {
      try { return addr.toNormalizedString() === ipaddr.parse(t).toNormalizedString(); }
      catch { return false; }
    });
  } catch { return false; }
}

function verifySignature(req) {
  const sig = req.headers["x-signature"];
  const ts = req.headers["x-timestamp"];
  if (!sig || !ts) return false;
  const now = Date.now();
  const tsNum = Number(ts);
  if (!Number.isFinite(tsNum) || Math.abs(now - tsNum) > MAX_SKEW_MS) return false;
  const toSign = `${ts}.${req.rawBody || ""}`;
  const expected = crypto.createHmac("sha256", WEBHOOK_SECRET).update(toSign).digest("hex");
  return crypto.timingSafeEqual(Buffer.from(expected), Buffer.from(sig));
}

app.post("/webhook", (req, res) => {
  const xfwd = (req.headers["x-forwarded-for"] || "").split(",")[0].trim();
  const ip = xfwd || req.socket.remoteAddress || "";
  if (!isTrustedIP(ip)) return res.status(403).json({ message: "Forbidden: Untrusted IP Address", ip });

  if (!verifySignature(req)) return res.status(401).json({ message: "Invalid signature" });

  const event = req.body || {};
  if (!event.eventId || !event.orderId || !event.orderState) return res.status(400).json({ message: "Invalid event payload" });

  res.json({ ok: true });

  switch (event.orderState) {
    case "PENDING":     console.log(`PENDING: ${event.orderId}`); break;
    case "WITHDRAWING": console.log(`WITHDRAWING: ${event.orderId}`); break;
    case "COMPLETED":
      console.log(`COMPLETED: ${event.orderId}`);
      console.log(`${event.inputAmount} ${event.inputCurrency} -> ${event.outputAmount} ${event.outputCurrency}`);
      break;
    default: console.log(`UNKNOWN STATE:`, event);
  }
});

app.listen(PORT, () => console.log(`[WEBHOOK] Payment Webhook Handler berjalan di port ${PORT}`));
EOF

cat > apps/duitnow-fpx-simulator/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import helmet from "helmet";
import morgan from "morgan";
import QRCode from "qrcode";
import Database from "better-sqlite3";
import { customAlphabet } from "nanoid";
import path from "path";

const app = express();
const PORT = Number(process.env.DUITNOW_PORT || 5005);
const DB_PATH = path.join(process.env.DUITNOW_DB || './data/duitnow.db');
const db = new Database(DB_PATH);
const nanoid = customAlphabet("1234567890abcdef", 10);

app.use(helmet());
app.use(express.json());
app.use(morgan("combined"));

db.exec(`
  CREATE TABLE IF NOT EXISTS payments (
    id TEXT PRIMARY KEY,
    amount INTEGER,
    status TEXT,
    payer_name TEXT,
    payer_id TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
  );
`);

app.post("/duitnow/generate", async (req, res) => {
  const { amount, payer_name, payer_id } = req.body;
  if (!amount || !payer_name) {
    return res.status(400).json({ message: "Amount and payer_name are required." });
  }

  const id = `DN-${nanoid()}`;
  const stmt = db.prepare("INSERT INTO payments (id, amount, status, payer_name, payer_id) VALUES (?, ?, 'PENDING', ?, ?)");
  stmt.run(id, amount, payer_name, payer_id || null);

  const qrData = JSON.stringify({
    type: "DuitNow",
    id,
    amount,
    name: payer_name,
    timestamp: Date.now()
  });

  const qrCode = await QRCode.toDataURL(qrData);

  res.json({
    id,
    qr_code_url: qrCode,
    status: "PENDING"
  });
});

app.post("/duitnow/complete/:id", (req, res) => {
  const { id } = req.params;
  const stmt = db.prepare("UPDATE payments SET status = 'COMPLETED' WHERE id = ?");
  const info = stmt.run(id);

  if (info.changes === 0) {
    return res.status(404).json({ message: "Payment not found." });
  }

  res.json({ message: "Payment completed successfully.", id });
});

app.listen(PORT, () => console.log(`[SIMULATOR] DuitNow Simulator berjalan di port ${PORT}`));
EOF

cat > apps/quantum-ecosystem-core/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import cors from "cors";
import morgan from "morgan";
import { WebSocketServer } from "ws";

const app = express();
const PORT_HTTP = process.env.QUANTUM_ECO_HTTP_PORT || 5000;
const PORT_WS = process.env.QUANTUM_ECO_WS_PORT || 8080;
const wss = new WebSocketServer({ port: PORT_WS });

app.use(cors());
app.use(express.json());
app.use(morgan("dev"));

app.get("/status", (req, res) => {
    res.json({ status: "online", http_port: PORT_HTTP, ws_port: PORT_WS });
});

wss.on("connection", (ws) => {
    console.log("Klien WebSockets baru disambungkan.");
    ws.on("message", (message) => {
        console.log(`Menerima mesej: ${message}`);
        // Logik pemprosesan mesej kuantum boleh ditambah di sini.
        ws.send(`Mesej diterima: ${message}`);
    });
});

app.listen(PORT_HTTP, () => console.log(`[CORE] Quantum Ecosystem Core berjalan di port ${PORT_HTTP}`));
EOF

cat > apps/qubitchain-miner/server.js <<'EOF'
import "dotenv/config";
import express from "express";
import cors from "cors";
import morgan from "morgan";
// Nota: MongoJS tidak disokong. Gunakan MongoClient dari 'mongodb'
// import mongojs from "mongojs";
// import { MongoClient } from "mongodb";

const app = express();
const PORT = process.env.PORT_QCLOUD || 3002;
// const MONGO_URI = process.env.MONGO_URI || "mongodb://localhost:27017/qubitchain_db";

app.use(cors());
app.use(express.json());
app.use(morgan("dev"));

app.get("/status", (req, res) => {
    res.json({ status: "ready", service: "QubitChain Miner" });
});

app.post("/mine", (req, res) => {
    // Logik perlombongan QubitChain akan diimplementasi di sini.
    console.log("Menerima permintaan perlombongan...");
    res.json({ message: "Perlombongan bermula.", data: req.body });
});

app.listen(PORT, () => console.log(`[MINER] QubitChain Miner berjalan di port ${PORT}`));
EOF

# Fail pelancar
cat > scripts/launcher.sh <<'EOF'
#!/bin/bash
# === Pelancar Induk Ekosistem AVSD ===
set -e
echo "[PELANCAR] Memulakan Ekosistem AVSD..."

# Pergi ke direktori akar projek
cd "$(dirname "$0")/.."

# Pasang semua kebergantungan jika folder node_modules tidak wujud
if [ ! -d "node_modules" ]; then
    echo "[PELANCAR] Memasang semua kebergantungan (npm & pip)..."
    npm run install-all
fi

# Mulakan servis Python di latar belakang
echo "[PELANCAR] Memulakan perkhidmatan Python (Chrono Engine)..."
# Hentikan proses lama jika ada
pkill -f "python3 apps/chrono-engine-ws/chrono_engine.py" || true
nohup python3 apps/chrono-engine-ws/chrono_engine.py > ./data/chrono_engine.log 2>&1 &

# Mulakan semua servis Node.js dengan PM2
echo "[PELANCAR] Memulakan semua perkhidmatan Node.js dengan PM2..."
pm2 start ecosystem.config.js
pm2 save

echo ""
echo "✅ Ekosistem AVSD telah dilancarkan!"
echo "Gunakan 'pm2 list' untuk lihat status."
echo "Gunakan 'pm2 logs' untuk lihat log."
EOF

# README Utama
cat > README.md <<'EOF'
# Ekosistem AVSD (AVSD Quantum Reality Ecosystem)

Selamat datang ke blueprint lengkap Ekosistem AVSD. Projek ini adalah gabungan semua sistem yang telah kita bincangkan, direka untuk berfungsi sebagai satu ekosistem bersepadu.

## Pra-syarat

- Node.js (v16+)
- Python 3
- pm2 dipasang secara global (npm install -g pm2)
- MongoDB (untuk qubitchain-miner)

## Pemasangan & Pelancaran

1.  Sediakan Fail .env: Salin .env.example ke .env dan isikan semua nilai rahsia (WEBHOOK_SECRET, GEMINI_API_KEY).
2.  Pasang Semua Kebergantungan: Dari direktori akar, jalankan:
       npm run install-all
    3.  Lancarkan Ekosistem: Jalankan skrip pelancar utama:
       bash scripts/launcher.sh
        atau
       npm start
EOF

chmod +x scripts/launcher.sh

echo ""
echo "===================================================="
echo "✅ PEMBINAAN BLUEPRINT SELESAI"
echo "===================================================="
echo "Blueprint lengkap untuk Ekosistem AVSD telah dibina di:"
echo "$BASE_DIR"
echo ""
echo "Arahan untuk bermula:"
echo "1. Tukar direktori: cd $BASE_DIR"
echo "2. Edit fail **.env** untuk masukkan API key Gemini demo."
echo "   Nano .env"
echo "   (Cari baris 'GEMINI_API_KEY=\"AIzaSyC5BjAQdFV41yZgxHDwc5zh0ad5HinDFdM\"' dan ganti)"
echo "3. Lancarkan sistem: bash scripts/launcher.sh"
echo ""
echo "A.R.I.A.N.A. sedia untuk fasa seterusnya."

`search_file_content` searches for a regular expression pattern within the content of files in a specified directory. Can filter files by a glob pattern. Returns the lines containing matches, along with their file paths and line numbers.

- **Tool name:** `search_file_content`
- **Display name:** SearchText
- **File:** `grep.ts`
- **Parameters:**
  - `pattern` (string, required): The regular expression (regex) to search for (e.g., `"function\s+myFunction"`).
  - `path` (string, optional): The absolute path to the directory to search within. Defaults to the current working directory.
  - `include` (string, optional): A glob pattern to filter which files are searched (e.g., `"*.js"`, `"src/**/*.{ts,tsx}"`). If omitted, searches most files (respecting common ignores).
- **Behavior:**
  - Uses `git grep` if available in a Git repository for speed; otherwise, falls back to system `grep` or a JavaScript-based search.
  - Returns a list of matching lines, each prefixed with its file path (relative to the search directory) and line number.
- **Output (`llmContent`):** A formatted string of matches, e.g.:
  ```
  Found 3 matches for pattern "myFunction" in path "." (filter: "*.ts"):
  ---
  File: src/utils.ts
  L15: export function myFunction() {
  L22:   myFunction.call();
  ---
  File: src/index.ts
  L5: import { myFunction } from './utils';
  ---
  ```
- **Confirmation:** No.

## 6. `replace` (Edit)

`replace` replaces text within a file. By default, replaces a single occurrence, but can replace multiple occurrences when `expected_replacements` is specified. This tool is designed for precise, targeted changes and requires significant context around the `old_string` to ensure it modifies the correct location.

- **Tool name:** `replace`
- **Display name:** Edit
- **File:** `edit.ts`
- **Parameters:**
  - `file_path` (string, required): The absolute path to the file to modify.
  - `old_string` (string, required): The exact literal text to replace.

    **CRITICAL:** This string must uniquely identify the single instance to change. It should include at least 3 lines of context _before_ and _after_ the target text, matching whitespace and indentation precisely. If `old_string` is empty, the tool attempts to create a new file at `file_path` with `new_string` as content.

  - `new_string` (string, required): The exact literal text to replace `old_string` with.
  - `expected_replacements` (number, optional): The number of occurrences to replace. Defaults to `1`.

- **Behavior:**
  - If `old_string` is empty and `file_path` does not exist, creates a new file with `new_string` as content.
  - If `old_string` is provided, it reads the `file_path` and attempts to find exactly one occurrence of `old_string`.
  - If one occurrence is found, it replaces it with `new_string`.
  - **Enhanced Reliability (Multi-Stage Edit Correction):** To significantly improve the success rate of edits, especially when the model-provided `old_string` might not be perfectly precise, the tool incorporates a multi-stage edit correction mechanism.
    - If the initial `old_string` isn't found or matches multiple locations, the tool can leverage the Gemini model to iteratively refine `old_string` (and potentially `new_string`).
    - This self-correction process attempts to identify the unique segment the model intended to modify, making the `replace` operation more robust even with slightly imperfect initial context.
- **Failure conditions:** Despite the correction mechanism, the tool will fail if:
  - `file_path` is not absolute or is outside the root directory.
  - `old_string` is not empty, but the `file_path` does not exist.
  - `old_string` is empty, but the `file_path` already exists.
  - `old_string` is not found in the file after attempts to correct it.
  - `old_string` is found multiple times, and the self-correction mechanism cannot resolve it to a single, unambiguous match.
- **Output (`llmContent`):**
  - On success: `Successfully modified file: /path/to/file.txt (1 replacements).` or `Created new file: /path/to/new_file.txt with provided content.`
  - On failure: An error message explaining the reason (e.g., `Failed to edit, 0 occurrences found...`, `Failed to edit, expected 1 occurrences but found 2...`).
- **Confirmation:** Yes. Shows a diff of the proposed changes and asks for user approval before writing to the file.

These file system tools provide a foundation for the Gemini CLI to understand and interact with your local project context.
