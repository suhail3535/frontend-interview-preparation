

### 1) Difference between `var`, `let`, and `const`

**Definition:** Keywords for declaring variables with different scoping and mutability.

* `var`: function-scoped, hoisted (initialized as `undefined`), can be re-declared/reassigned.
* `let`: block-scoped, hoisted but in TDZ, cannot be re-declared in same scope, can be reassigned.
* `const`: block-scoped, hoisted but in TDZ, cannot be re-declared or reassigned (but object contents can mutate).
  **Example:**

```js
function demo() {
  var x = 1;  // function scope
  if (true) {
    let y = 2; // block scope
    const z = 3;
  }
  // y, z not accessible here
}
```

---

### 2) Closures (with use case)

**Definition:** A closure is when an inner function retains access to variables of its outer function even after the outer has returned.
**Why it matters:** Enables data privacy and stateful functions without global variables.
**Example (counter with private state):**

```js
function createCounter() {
  let count = 0;
  return () => ++count;
}
const counter = createCounter();
counter(); // 1
counter(); // 2
```

**Use case:** Debounced handlers, memoization, module pattern, encapsulating configuration.

---

### 3) Flatten a nested array

**Goal:** Convert `[1,[2,[3,4]],5]` → `[1,2,3,4,5]`.
**Recursive reduce:**

```js
const flatten = (arr) =>
  arr.reduce((acc, v) => acc.concat(Array.isArray(v) ? flatten(v) : v), []);
```

**Iterative (stack):**

```js
function flattenIter(arr){
  const stack=[...arr], out=[];
  while(stack.length){
    const v = stack.pop();
    Array.isArray(v) ? stack.push(...v) : out.push(v);
  }
  return out.reverse();
}
```

---

### 4) Reverse a string (no built-ins)

```js
function reverse(str){
  let res = "";
  for (let i = str.length - 1; i >= 0; i--) res += str[i];
  return res;
}
```

**Idea:** Iterate from end to start and build the result.

---

### 5) Event bubbling & stopping it

**Definition:** Bubbling = event propagates from the target element up through ancestors.
**Stop:** `event.stopPropagation()`; also `stopImmediatePropagation()` for blocking other listeners on same element.
**Example:**

```html
<div onclick="alert('parent')">
  <button onclick="event.stopPropagation(); alert('child')">Click</button>
</div>
```

---

### 6) Promises and async/await

**Promise:** An object representing eventual completion/failure of async work.
**async/await:** Syntactic sugar to write promise code that looks synchronous.
**Example:**

```js
const fetchUser = (id) => fetch(`/api/users/${id}`).then(r => r.json());
async function load() {
  try {
    const user = await fetchUser(1);
  } catch (e) { /* handle */ }
}
```

---

### 7) Debouncing vs Throttling

* **Debounce:** Run after N ms of inactivity (merge rapid calls). *Use:* search boxes.
* **Throttle:** Run at most once every N ms (limit rate). *Use:* scroll/resize handlers.
  **Examples:**

```js
function debounce(fn, wait){
  let t; return (...a)=>{ clearTimeout(t); t=setTimeout(()=>fn(...a), wait); };
}
function throttle(fn, wait){
  let last=0; return (...a)=>{ const now=Date.now();
    if(now-last>=wait){ last=now; fn(...a); }
  };
}
```

---

### 8) Hoisting

**Definition:** Declarations are moved to the top of scope during compile phase.

* `var` hoists with `undefined`.
* `let/const/function/class` hoist but are in TDZ until initialized.
  **Example:**

```js
console.log(a); // undefined
var a = 10;
// console.log(b); // ReferenceError (TDZ)
let b = 20;
```

---

### 9) Prototypal inheritance

**Definition:** Objects inherit from other objects via the internal `[[Prototype]]` chain.
**Example:**

```js
const base = { greet(){ console.log('hi'); } };
const user = Object.create(base);
user.greet(); // 'hi'
```

**Why:** Memory sharing, dynamic delegation.

---

### 10) Simple REST API with Express

```js
const express = require('express');
const app = express();
app.use(express.json());

const users = [{ id:1, name:'Suhail' }];

app.get('/users', (req,res)=> res.json(users));
app.post('/users', (req,res)=> { users.push({ id:Date.now(), ...req.body }); res.status(201).json({ ok:true }); });
app.put('/users/:id', (req,res)=> { /* update logic */ res.json({ ok:true }); });
app.delete('/users/:id', (req,res)=> { /* delete logic */ res.json({ ok:true }); });

app.listen(5000, ()=> console.log('API on 5000'));
```

---

### 11) How MongoDB stores data

**Definition:** Document-oriented NoSQL DB; stores data as **BSON** (binary JSON) documents in **collections**.
**Features:** Flexible schema, nested documents/arrays, indexes, replication, sharding.

---

### 12) CORS and handling it in Node.js

**Definition:** Browser security policy restricting cross-origin requests.
**Fix in Express:** Add appropriate headers or use `cors` middleware.

```js
const cors = require('cors');
app.use(cors({ origin: ['https://yourapp.com'], credentials: true }));
```

**Key headers:** `Access-Control-Allow-Origin`, `-Methods`, `-Headers`, `-Credentials`.

---

### 13) React Virtual DOM performance

**Definition:** In-memory tree representation of UI used to compute minimal DOM changes.
**Why faster:** Batch updates + diffing → fewer real DOM mutations.
**Example:** setState → VDOM diff → minimal `document` updates.

---

### 14) `useState` and `useEffect`

* **useState:** Local reactive state.

```js
const [count, setCount] = useState(0);
```

* **useEffect:** Side effects (data fetching, subscriptions, syncing).

```js
useEffect(() => {
  // effect
  return () => { /* cleanup */ };
}, [deps]);
```

**Rules:** Run after render; deps array controls when it runs.

---

### 15) Debug: form submitted but DB not updated

**Approach (front → back → DB):**

1. **Frontend:** Check network tab → request URL, method, payload, CORS, auth token. Validate form data and onSubmit prevent default.
2. **API:** Check route, HTTP method, body parsing (`express.json()`), validation errors.
3. **Service/Controller:** Log inputs, handle try/catch, return errors.
4. **DB layer:** Check connection string, model/schema validation, required fields, write permissions.
5. **DB itself:** Look for error logs, wrong DB name/collection, staging vs prod env.
   **Tip:** Add correlation IDs and log through the stack.

---

### 16) Build a React registration page (validation + errors + submit)

**Plan:**

* Controlled inputs + client validation (required, email format, password rules).
* Show field errors and server errors.
* Submit via `fetch/axios` to Node route; disable button while loading.
  **Example (skimmed):**

```jsx
function Register(){
  const [form, setForm] = useState({name:'', email:'', pass:''});
  const [err, setErr] = useState({});
  const [busy, setBusy] = useState(false);

  const validate = () => {
    const e = {};
    if(!form.name.trim()) e.name='Name required';
    if(!/^\S+@\S+\.\S+$/.test(form.email)) e.email='Invalid email';
    if(form.pass.length<8) e.pass='Min 8 chars';
    setErr(e); return Object.keys(e).length===0;
  };

  const submit = async () => {
    if(!validate()) return;
    setBusy(true);
    try {
      await axios.post('/api/register', form);
      // success UI
    } catch (e) {
      setErr({ server: e.response?.data?.message || 'Failed' });
    } finally { setBusy(false); }
  };

  return (/* inputs + errors + submit button calling submit() */);
}
```

---



### 1) Design a scalable blogging platform (MERN)

**Key pieces:**

* **React:** SSR/CSR hybrid (Next.js optional), routes: home, post, author, admin.
* **Node/Express:** REST APIs for posts, comments, tags, auth, media upload.
* **MongoDB:** Collections: users, posts, comments, tags; indexes on `slug`, `createdAt`, `authorId`, text index on `title/body`.
* **CDN/S3** for images, **Redis** for caching hot posts, **NGINX** for reverse proxy, **JWT** auth, **RBAC** for admin/editor/reader.
* **Scalability:** Horizontal Node instances, stateless app, sticky sessions only if sockets, shard Mongo on `postId` or `authorId` when needed, queue (Bull/Redis) for email/thumbnail jobs.

---

### 2) Authentication in MERN

**Flow:** Sign-up → hash password (bcrypt) → store user. Login → verify → issue JWT (access + refresh).
**Frontend:** Store access token in memory (or httpOnly cookie via server), attach `Authorization: Bearer`. Refresh flow on 401.
**Backend:** Protect routes with JWT middleware; rotate/blacklist refresh tokens on logout.

```js
// Express JWT middleware (simplified)
function auth(req,res,next){
  const t = req.headers.authorization?.split(' ')[1];
  try { req.user = jwt.verify(t, process.env.JWT_SECRET); next(); }
  catch { return res.status(401).json({message:'Unauthorized'}); }
}
```

---

### 3) Redux data flow

**Unidirectional:**
Component → **dispatch(action)** → **reducer** (pure) → **store** updates → components **subscribe** re-render.
**Async:** Use `redux-thunk` or `redux-saga` to handle side effects before dispatching success/failure actions.

---

### 4) React Hooks & custom hooks you’ve written

**Hooks:** `useState`, `useEffect`, `useMemo`, `useCallback`, `useRef`, `useReducer`, `useContext`.
**Custom hooks (examples):**

* `useFetch(url)`: fetch with loading/error states.
* `useDebounce(value, delay)`: return debounced value.
* `useAuth()`: manage auth state, tokens, user info.

```js
function useDebounce(value, delay=300){
  const [v, setV] = useState(value);
  useEffect(()=>{ const id=setTimeout(()=>setV(value), delay); return ()=>clearTimeout(id); },[value,delay]);
  return v;
}
```

---

### 5) Prevent unnecessary re-renders in React

* Memoize components: `React.memo`.
* Memoize values/functions: `useMemo`, `useCallback`.
* Split large components; move state down to leaf nodes.
* Key lists properly; avoid recreating props on every render.
* Use selector memoization (`reselect`) for Redux.

---

### 6) Implement Role-Based Access Control (RBAC)

**Model:** Users → Roles → Permissions.
**Backend:** Store role on user; middleware checks `role` or permission map.

```js
function allow(...roles){
  return (req,res,next)=> roles.includes(req.user.role) ? next() : res.sendStatus(403);
}
app.delete('/users/:id', auth, allow('admin'), ctrl.deleteUser);
```

**Frontend:** Hide/disable UI actions based on role; also rely on backend enforcement.

---

### 7) Design a real-time chat app

**Stack:** React + Socket.IO client; Node/Express + Socket.IO server; MongoDB for messages; Redis adapter for multi-node sockets.
**Features:** Rooms, typing indicators, delivered/read receipts, JWT auth on socket handshake, message persistence, offline push (queue).
**Scaling:** Sticky sessions or Socket.IO with Redis pub/sub.

---

### 8) Manage large form states in React

* Use **`react-hook-form`** or **Formik** for performant uncontrolled inputs.
* Schema validation via **Zod/Yup**.
* Split into steps/sections; lazy render offscreen parts; use field arrays.
* Keep async validation throttled/debounced.

---

### 9) Paginate large datasets in MongoDB

* **Skip/limit** for small pages (not efficient for deep pages).
* **Range/Keyset pagination** using indexed fields (`_id`, `createdAt`) with `$gt/$lt`.
* **Aggregation + `$facet`** for data + total count when needed.

```js
// Keyset (forward)
db.posts.find({ createdAt: { $lt: cursor } })
  .sort({ createdAt: -1 }).limit(20)
```

---

### 10) Indexes in MongoDB & performance

**Definition:** Data structures that speed reads by mapping fields to document locations.
**Types:** Single field, compound, text, TTL, hashed, geospatial.
**Effects:** Faster queries, slower writes, extra storage.
**Rule:** Match query patterns; order in compound index matters (e.g., `{ status:1, createdAt:-1 }`).

---

### 11) Express middleware (with examples)

**Definition:** Functions with signature `(req, res, next)` that run before route handlers.
**Types:** Application (`app.use`), router-level, error-handling `(err, req, res, next)`.
**Examples:** Logging, auth, rate-limit, validation.

```js
app.use((req,res,next)=>{ console.log(req.method, req.url); next(); });
app.use('/admin', auth, allow('admin'));
app.use((err,req,res,next)=> res.status(500).json({message:err.message}));
```

---

### 12) SQL vs NoSQL (for large-scale apps)

* **SQL:** Structured schema, ACID, complex joins, strong consistency. Good for transactions, reporting (e.g., banking).
* **NoSQL (Mongo):** Flexible schema, horizontal scale, high write throughput, nested docs. Good for content feeds, logs, catalogs.
  **Trade-offs:** SQL = strict schemas; NoSQL = denormalization + app-level joins.

---

### 13) Load balancing & how to implement

**Definition:** Distributing traffic across multiple servers to improve availability and throughput.
**Ways:**

* **Layer 4/7** load balancers (Nginx, HAProxy, ALB).
* **Round-robin**, **least connections**, **IP hash**.
* Health checks, sticky sessions (if needed), TLS termination.
  **Infra:** DNS → LB → multiple Node instances (PM2/containers) → autoscaling group.

---

### 14) Monitor & log Node.js in production

* **Logs:** Structured JSON logs (pino/winston), correlation IDs, log levels, centralize to ELK/CloudWatch/Datadog.
* **Metrics:** CPU, memory, event loop lag, req latency (Prometheus + Grafana).
* **Tracing:** OpenTelemetry for distributed traces.
* **Errors:** Sentry/Bugsnag; crash handling with process managers (PM2/systemd), alerts on anomalies.

---

## Scenario-Based (Round 2)

### 15) Secure RBAC where Admin edits/deletes, users read-only

**Backend:**

* JWT auth → `req.user.role`.
* Protect mutating routes with `allow('admin')`.
* At DB layer, double-check ownership if needed (e.g., editors can only modify own posts).

```js
app.get('/users', auth, ctrl.list);
app.patch('/users/:id', auth, allow('admin'), ctrl.update);
app.delete('/users/:id', auth, allow('admin'), ctrl.remove);
```

**Frontend:**

* Hide admin UI for non-admins (but never rely on this only).
* Route guards for admin dashboard.
  **Auditing:** Log who did what; return 403 for forbidden.

---

## Bonus: CRUD + Server Setup (quick reference)

### Express server + CRUD (Users)

```js
const express = require('express');
const mongoose = require('mongoose');
const app = express();
app.use(express.json());

mongoose.connect(process.env.MONGO_URI);

const User = mongoose.model('User', new mongoose.Schema({
  name: String, email: { type:String, unique:true }, role: { type:String, default:'user' }
}, { timestamps:true }));

app.get('/users', async (req,res)=> res.json(await User.find().lean()));
app.post('/users', async (req,res)=> res.status(201).json(await User.create(req.body)));
app.put('/users/:id', async (req,res)=> res.json(await User.findByIdAndUpdate(req.params.id, req.body, { new:true })));
app.delete('/users/:id', async (req,res)=> { await User.findByIdAndDelete(req.params.id); res.sendStatus(204); });

app.listen(5000, ()=> console.log('Server running on :5000'));
```

---
