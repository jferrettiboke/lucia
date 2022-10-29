---
order: 0
layout: "@layouts/DocumentLayout.astro"
title: "Server API"
---

These can be imported from `@lucia-auth/sveltekit`.

```ts
import { handleHooks } from "@lucia-auth/sveltekit";
```

## `handleHooks()`

For the handle function in hooks. Reads the session id from cookies and validates it, attempting to renew it if the session id has expired. This also creates handles requests to Lucia's api endpoints and creates a global variable in the client for internal use.

```ts
const handleHooks: (auth: Auth) => Handle;
```

#### Parameter

| name | type                                        | description    |
| ---- | ------------------------------------------- | -------------- |
| auth | [`Auth`](/reference/types/lucia-types#auth) | Lucia instance |

#### Returns

| type     | description       |
| -------- | ----------------- |
| `Handle` | A handle function |

#### Example

```ts
import { auth } from "$lib/server/lucia";
import { handleHooks } from "@lucia-auth/sveltekit";

export const handle: Handle = handleHooks(auth);
```

```ts
import { auth } from "$lib/server/lucia";
import { handleHooks } from "@lucia-auth/sveltekit";
import { sequence } from "@sveltejs/kit";

export const handle: Handle = sequence(handleHooks(auth), customHandle);
```

## `handleServerSession()`

For the root layout server load function. Reads the sessions passed on from hooks (`handleHooks()`), gets the user, and passes on to child load functions and the client. If a server load function is provided (which can return some data), Lucia will run it after it finishes handling sessions.

```ts
const handleServerSession: (auth: Auth, serverLoad?: ServerLoad) => ServerLoad;
```

#### Parameter

| name       | type                                        | description            | optional |
| ---------- | ------------------------------------------- | ---------------------- | -------- |
| auth       | [`Auth`](/reference/types/lucia-types#auth) | Lucia instance         |
| serverLoad | `ServerLoad`                                | A server load function | true     |

#### Returns

| type         | description            |
| ------------ | ---------------------- |
| `ServerLoad` | A server load function |

#### Example

```ts
import { auth } from "$lib/server/lucia";
import { handleServerSession } from "@lucia-auth/sveltekit";
import type { ServerLoad } from "@sveltejs/kit";

export const Load: ServerLoad = handleServerSession(auth, async (event) => {
	return {
		message: "hi"
	};
});
```