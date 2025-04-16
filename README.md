# Keybindy

**Keybindy** is a lightweight, fast, and framework-agnostic TypeScript library for managing keyboard shortcuts in JavaScript applications. With a small footprint and zero dependencies, Keybindy makes it easy to register, manage, and scope keyboard shortcuts in any environment — whether you're building with vanilla JavaScript, Vue, Svelte, or another framework.

The `@keybindy/core` package is the foundation of the Keybindy ecosystem, providing all the logic for keyboard shortcut management. For React developers, the optional `@keybindy/react` package offers seamless integration.

[![npm version](https://badge.fury.io/js/@keybindy%2Fcore.svg)](https://www.npmjs.com/package/@keybindy/core)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🚀 Why **Keybindy**?

Keyboard shortcuts are essential for productivity and a smooth user experience — but managing them across components, contexts, and frameworks can quickly become a nightmare.  
That’s where **Keybindy** comes in.

#### Why not other libraries?

Other shortcut libraries often come with:

- Framework lock-ins (React-only, etc.)
- Extra dependencies that bloat your bundle
- Complex APIs and awkward scope handling
- Larger file sizes that slow down performance

#### What makes Keybindy different?

**Keybindy** is a blazing-fast, ultra-lightweight **TypeScript-first** solution for handling keyboard shortcuts.  
It’s designed to be:

- **Tiny & dependency-free** — approximately **2KB** gzipped
- **Framework-agnostic** — works with **Vanilla JS, React, Vue, Svelte**, and beyond
- **Simple yet powerful** — clean APIs to register, scope, and manage shortcuts effortlessly
- **Tree-shakeable** — only includes what you actually use
- **Side-effect free** — making it ideal for modern builds

Whether you're building a single-page app, a design tool, or a productivity suite — **Keybindy** gives you total control over keyboard interactions without the baggage.

---

### Use Cases

- Registering global shortcuts (e.g., `Ctrl+S` for saving)
- Managing scoped shortcuts for modals, editors, or UI sections
- Creating keyboard-driven UIs for accessibility and power users
- Enhancing web games and interactive tools with custom bindings

---

## Features

- **✅ Global and Scoped Shortcuts** – Define app-wide or context-specific keys
- **🎺 Multi-Key Combos** – Full support for combinations like Ctrl+Shift+K
- **♻️ Key Alias Normalization** – Smart matching of `cmd → meta`, `ctrl (left) | ctrl (right) → ctrl`, etc.
- **🧼 Prevent Default Behavior** – Easily block native browser actions
- **⚡ Zero Dependencies** – Lightweight and fast
- **🔧 Framework Agnostic** – Works with any frontend stack
- **🔒 Type-Safe** – Written in TypeScript with full .d.ts support
- **🌐 CDN Friendly** – Use in plain HTML projects with a simple script tag
- **🔌 Custom Event Hooks** – Emit key events for custom behavior and extensions

---

## Installation

Install the core package using your preferred package manager:

```bash
# npm
npm install @keybindy/core

# yarn
yarn add @keybindy/core

# bun
bun add @keybindy/core
```

Or use via CDN (URL coming soon):

```html
<script src="https://cdn.jsdelivr.net/npm/@keybindy/core@latest/dist/keybindy.min.js"></script>
```

---

## Getting Started

### Initialization

```ts
// With import
import ShortcutManager from '@keybindy/core';
const manager = new ShortcutManager();

// With CDN
const manager = new Keybindy();
```

---

### Register shortcuts

```ts
// Register "Enter" to submit a form in the "modal" scope
manager.register(
  ['Enter'],
  () => {
    console.log('Submitting modal form...');
  },
  { scope: 'modal', preventDefault: true }
);

// Activate the modal scope (e.g., when modal opens)
manager.setActiveScope('modal');
```

---

### Supported methods

#### **`start`**

Starts the manager manually. This is usually not required, as the manager starts automatically on instantiation.

##### Example

```ts
manager.start();
```

---

#### **`register`**

Registers a new shortcut.

| Parameter | Type              | Required | Description                                             |
| --------- | ----------------- | -------- | ------------------------------------------------------- |
| `keys`    | `Keys[]`          | ✅       | Keys to bind (e.g., ["ctrl", "shift", "k"])             |
| `handler` | `ShortcutHandler` | ✅       | Callback to execute when the shortcut is triggered.     |
| `options` | `ShortcutOptions` | ❌       | Optional config (scope, preventDefault, metadata, etc.) |

##### Example

```ts
manager.register(
  ['ctrl', 'shift', 'k'],
  () => {
    console.log('Triggered Ctrl+Shift+K');
  },
  {
    preventDefault: true,
    scope: 'modal',
    sequential: true,
    sequenceDelay: 1000,
    data: {
      // metadata
      label: 'Ctrl+Shift+K',
      description: 'Submit form',
    },
  }
);
```

---

#### **`unregister`**

Removes a previously registered shortcut.

| Parameter | Type     | Required | Description         |
| --------- | -------- | -------- | ------------------- |
| `keys`    | `Keys[]` | ✅       | The keys to unbind. |

##### Example

```ts
manager.unregister(['ctrl', 'shift', 'k']);
```

---

#### **`enable / disable / toggle`**

Enables, disables, or toggles a shortcut on or off.

| Parameter | Type     | Required | Description |
| --------- | -------- | -------- | ----------- |
| `keys`    | `Keys[]` | ✅       |             |

##### Example

```ts
manager.enable(['ctrl', 's']);
manager.disable(['ctrl', 's']);
manager.toggle(['ctrl', 's']);
```

---

#### **`enableAll / disableAll`**

Enable or disable all shortcuts — globally or within a specific scope.

| Parameter | Type     | Required | Description                            |
| --------- | -------- | -------- | -------------------------------------- |
| `scope`   | `string` | ❌       | scope to enable/disable all shortcuts. |

##### Example

```ts
manager.enableAll(); // Global
manager.enableAll('modal'); // Scoped

manager.disableAll(); // Global
manager.disableAll('modal'); // Scoped
```

---

#### **`getCheatSheet`**

Returns a list of all registered shortcuts. Optionally scoped.

| Parameter | Type     | Required | Description               |
| --------- | -------- | -------- | ------------------------- |
| `scope`   | `string` | ❌       | scope to get cheat sheet. |

##### Example

```ts
manager.getCheatSheet();
```

---

### Scope Management

#### **`getScopes`**

Returns all registered scopes.

##### Example

```ts
manager.getScopes();
```

---

#### **`getActiveScope`**

Returns the currently active scope.

##### Example

```ts
manager.getActiveScope();
```

---

#### **`setActiveScope`**

Sets the current active scope.

| Parameter | Type     | Required | Description       |
| --------- | -------- | -------- | ----------------- |
| `scope`   | `string` | ✅       | The scope to set. |

##### Example

```ts
manager.setActiveScope('modal');
```

---

#### **`isScopeActive`**

Checks if a specific scope is currently active.

| Parameter | Type     | Required | Description         |
| --------- | -------- | -------- | ------------------- |
| `scope`   | `string` | ✅       | The scope to check. |

##### Example

```ts
manager.isScopeActive('modal');
```

---

#### **`getScopesInfo`**

Returns information about all scopes or a specific one.

| Parameter | Type     | Required | Description        |
| --------- | -------- | -------- | ------------------ |
| `scope`   | `string` | ❌       | scope to get info. |

##### Example

```ts
manager.getScopesInfo();
manager.getScopesInfo('modal');
```

---

#### **`pushScope / popScope / resetScope`**

Manage the scope stack.

##### Example

```ts
manager.pushScope('modal');
manager.popScope();
manager.resetScope();
```

---

#### **`destroy`**

Fully destroys the manager instance and removes all bindings.

##### Example

```ts
manager.destroy();
```

---

#### **`clear`**

Clears the internal state without destroying the instance.

##### Example

```ts
manager.clear();
```

---

### Event

#### **`onTyping`**

Listen for every typed key. Useful for custom behavior or analytics.

##### Example

```ts
manager.onTyping(({ key, event }) => {
  console.log(`Key typed: ${key}`, event);
});
```

---

## Ecosystem

Keybindy has modular packages for different platforms. Each package is built to work seamlessly with the core engine.

| Package                                                        | Description                                                                      |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [`@keybindy/core`](https://npmjs.com/package/@keybindy/core)   | The core JavaScript library. Framework-agnostic, fully typed, and tree-shakable. |
| [`@keybindy/react`](https://npmjs.com/package/@keybindy/react) | React bindings with hooks and components for easy integration.                   |
| _Coming Soon_                                                  | Stay tuned!                                                                      |

---

## Contributing

PRs, issues, and ideas are welcome! See [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

If you're adding a new framework integration (like Vue or Svelte), feel free to open a draft PR — we'd love to collaborate.

---

> _Might be new in the shortcut game, but Keybindy’s here to change the frame — fast, flexible, and ready to claim. 🎯_
