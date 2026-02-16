---

name: nstate
description: Provides expertise on the `nstate` library, a high-performance React state management tool. Use this for questions about store creation, Immer-based updates, `useBind` for forms, `useSubStore` for state slicing, and combining multiple stores.

---
## nstate

**nstate** is a high-performance React state management library designed for low mental overhead. It features auto-binding, built-in **Immer** support for immutable updates, and powerful store composition.

### Key API Methods

| Method | Description |
| --- | --- |
| `setState(patch)` | Updates state via object, updater function, or **Immer** draft. |
| `useState(getter)` | React hook to subscribe to specific state slices. |
| `useWatch(getter, handler)` | Watches state changes and triggers side effects; auto-cleans on unmount. |
| `useBind(getter)` | Two-way binding for form inputs with type safety. |
| `useSubStore(getter, setter)` | Creates a synchronized "view" of a parent store's slice. |
| `useLocalStore(initial, actions)` | Creates a localized store instance within a component. |

---

## Practical Examples

### 1. The Class-Based Store (Recommended)

Encapsulate logic by extending the `NState` class.

```tsx
import NState from 'nstate';

interface CounterState { count: number }

export class CounterStore extends NState<CounterState> {
  inc = () => this.setState(s => ({ count: s.count + 1 }));

  // Using Immer draft
  reset = () => this.setState(draft => { draft.count = 0; });
}

export const counterStore = new CounterStore({ count: 0 });

```

### 2. Form Binding with `useBind`

Eliminate boilerplate `onChange` handlers while maintaining type safety.

```tsx
function ProfileEditor() {
  const bind = store.useBind(s => s.user);
  return (
    <input type="text" {...bind('name')} />
    /* Automatically handles value and onChange */
  );
}

```

### 3. Store Composition

You can link stores together using the `watch` API inside `onInit`.

```tsx
export class RootStore extends NState<GlobalState> {
  child = new ChildStore({ data: '' });

  onInit() {
    this.child.watch(s => s.data, (val) => {
      console.log("Child store updated global effect:", val);
    });
  }
}

```
