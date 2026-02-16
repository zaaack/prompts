### 🌐 keyv-browser Core Overview

**keyv-browser** provides two specialized storage adapters that allow the standard Keyv API to work seamlessly within a web browser environment.

---

### 🛠 Available Adapters

| Adapter | Storage Engine | Best For |
| --- | --- | --- |
| `KeyvLocalStorage` | `window.localStorage` | Simple, synchronous, small strings (<5MB). |
| `KeyvIndexedDB` | `IndexedDB` | Larger datasets, binary data, and non-blocking I/O. |

---

### 💡 Usage Patterns

#### 1. Standard Keyv Integration

You can swap the storage engine based on your persistence needs without changing your application logic.

```typescript
import Keyv from 'keyv'
import { KeyvLocalStorage, KeyvIndexedDB } from 'keyv-browser'

// For simple settings
const settingsStore = new Keyv({
  store: new KeyvLocalStorage()
});

// For heavy data/caching
const cacheStore = new Keyv({
  store: new KeyvIndexedDB()
});

```

#### 2. Declarative "Field" Usage

Similar to `keyv-file`, you can use `makeField` to create direct, type-safe accessors for specific keys.

```typescript
import { KeyvLocalStorage, makeField } from 'keyv-browser'

class AppStore extends KeyvLocalStorage {
  // makeField(store, key, defaultValue)
  theme = makeField(this, 'ui_theme', 'light')
  sessionCount = makeField(this, 'session_count', 0)
}

const store = new AppStore()

// Note: keyv-browser operations are asynchronous
await store.theme.set('dark')
const currentTheme = await store.theme.get() // 'dark'

```
