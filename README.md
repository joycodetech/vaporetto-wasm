# @joycodetech/vaporetto-wasm

Node.js WASM bindings for [daac-tools/vaporetto](https://github.com/daac-tools/vaporetto) — a fast and lightweight Japanese tokenizer.

> **Unofficial package.** Official bindings may be published by the original authors in the future.
> Models are provided separately by [daac-tools/vaporetto-models](https://github.com/daac-tools/vaporetto-models).

## Installation

```bash
npm install @joycodetech/vaporetto-wasm
```

Node.js >= 18 required.

## Download a model

Download a pre-trained model from [daac-tools/vaporetto-models releases](https://github.com/daac-tools/vaporetto-models/releases).

```bash
wget https://github.com/daac-tools/vaporetto-models/releases/download/v0.5.0/bccwj-suw_c0.003.tar.xz
tar xf bccwj-suw_c0.003.tar.xz
# → bccwj-suw_c0.003/bccwj-suw_c0.003.model.zst
```

The extracted `.model.zst` (zstd-compressed) can be passed directly — no further decompression needed.

## Usage

```js
import { VaporettoTokenizer } from "@joycodetech/vaporetto-wasm";
import { readFileSync } from "fs";

const model = readFileSync("./bccwj-suw_c0.003/bccwj-suw_c0.003.model.zst");
const tokenizer = new VaporettoTokenizer(model);

// Returns a space-separated string
tokenizer.tokenize("日本語のトークナイズ");
// → "日本語 の トークナイズ"

// Returns an array of strings
tokenizer.tokenize_to_array("日本語のトークナイズ");
// → ["日本語", "の", "トークナイズ"]

// Free the tokenizer when done (optional but recommended)
tokenizer.free();
```

## API

### `new VaporettoTokenizer(model_data: Uint8Array)`

Creates a tokenizer from a raw or zstd-compressed model buffer.

### `tokenize(text: string): string`

Tokenizes the input and returns a space-separated string of tokens.

### `tokenize_to_array(text: string): string[]`

Tokenizes the input and returns an array of token strings.

### `free(): void`

Releases the WASM memory. Also available as `[Symbol.dispose]()` for use with `using`.

## License

MIT — see [LICENSE](./LICENSE).

The pre-trained models are distributed under their own licenses (typically BSD-3-Clause for BCCWJ-based models). See the model archive for details.
