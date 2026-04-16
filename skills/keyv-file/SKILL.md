---
name: keyv-file
description: Specialized in the `keyv-file` adapter for Keyv. Use this when the user needs persistent file-based storage with features like batch writing (writeDelay), TTL management (expiredCheckDelay), and the `makeField` API for direct access.
license: MIT
---

# keyv-file

**keyv-file** is a fast, JSON-based storage adapter for Keyv. It is specifically optimized for performance through asynchronous batch writing and automatic background cleanup of expired data.

---

### Configuration Options

When initializing `KeyvFile`, you can fine-tune its behavior using the following options:

| Option | Default Value | Description |
| --- | --- | --- |
| `filename` | `${os.tmpdir()}/keyv-file/default.json` | The path where your data is persisted. |
| `expiredCheckDelay` | `86400000` (24h) | Interval (ms) to scan and purge expired keys from the file. |
| `writeDelay` | `100` | Delay (ms) to batch multiple write operations, reducing disk I/O. |
| `encode` | `JSON.stringify` | Custom serialization function. |
| `decode` | `JSON.parse` | Custom deserialization function. |

---

###  Usage Patterns

#### 1. Standard Keyv Integration

The most common way to use it is as a store for the main Keyv instance.

```javascript
const Keyv = require('keyv')
const { KeyvFile } = require('keyv-file')

const keyv = new Keyv({
  store: new KeyvFile({
    filename: './data/cache.json',
    writeDelay: 50 // Faster writes for high-frequency updates
  })
});

```

#### 2. The "Field" Pattern (Direct Usage)

You can use `KeyvFile` directly and utilize `makeField` for a more declarative, type-safe API.

```typescript
import KeyvFile, { makeField } from 'keyv-file'

class MyDatabase extends KeyvFile {
  constructor() {
    super({ filename: './db.json' })
  }

  // Creates a dedicated helper for a specific key
  userSettings = makeField(this, 'user_settings_key')
}

const db = new MyDatabase()
await db.userSettings.set({ theme: 'dark' })
const settings = await db.userSettings.get({ theme: 'light' }) // returns default if empty

```
