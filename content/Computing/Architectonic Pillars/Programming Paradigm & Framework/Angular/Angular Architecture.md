# Angular Architecture

---

## Performance: trackBy in ngFor

`trackBy` lets Angular identify list items by a unique key instead of object reference, preventing unnecessary DOM re-renders and improving performance, especially in large or frequently updated lists.

```typescript
// template
*ngFor="let item of items; trackBy: trackById"

// component
trackById(index: number, item: Item): number { return item.id; }
```

---

## Script Load Order & NG0908

**Root Cause:** `main.js` and `polyfills.js` shared `loadOrder=2`, so occasionally `main.js` bootstrapped Angular before Zone.js was registered — a race condition.

**Fix:** Bump `main.js` to `loadOrder=3` so Zone.js always loads first.

```json
{ "filename": "styles.css",   "elementType": "link",   "loadOrder": 1 },
{ "filename": "runtime.js",   "elementType": "script", "loadOrder": 1 },
{ "filename": "polyfills.js", "elementType": "script", "loadOrder": 2 },
{ "filename": "main.js",      "elementType": "script", "loadOrder": 3 }
```

---

## Zone.js & Change Detection

##### Zone.js  
1. Requires for change detection. 
2. **monkey-patches async APIs** such as:
	- `setTimeout`, `setInterval`
	- DOM events (`addEventListener`)
	- `XMLHttpRequest` / `fetch`
	- `Promise.then`, etc.
-  Angular builds **`NgZone`** on top of Zone.js and uses it to:
    - Track when async tasks start / finish.
    - Decide **when to run change detection** (so UI updates after XHR, timers, events).
    - Integrate testability (knowing when the app is “stable”).
- So, conceptually: **Zone.js = async task tracker; Angular = “run CD when Zone says we’re idle”.**

Bootstrapping sequence that Angular expects:
1. Load and execute **`polyfills.js`**:
    - Zone.js patches all async APIs on the global `window`.
2. Then load and execute **`main.js`**:
    - Angular imports `zone.js` APIs (through patched globals) and creates an `NgZone`.
    - `platformBrowserDynamic().bootstrapModule(AppModule)` runs inside a Zone-aware environment.
- Consequences of Loading main.js before polyfills.js:
    - Angular bootstraps without a fully-patched async environment.
    - Internal assumptions like “Zone is present and owns microtasks” are violated.
    - That surfaced as **NG0908 runtime errors** on DSS/chat pages.

#### main.js
1. `main.js` is the **compiled application bundle** built from `src/main.ts` and your app code. It typically contains: 
2. ```java 
   platformBrowserDynamic()
	  .bootstrapModule(AppModule)
	  .catch(err => console.error(err));
   ```