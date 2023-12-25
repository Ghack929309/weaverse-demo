

_Pilot is an innovative Shopify theme, powered by Hydrogen, Remix, and Weaverse, designed to create lightning-fast storefronts with exceptional performance. This theme combines a collection of powerful tools and features to streamline your Shopify development experience._

## Demo

- [Live store](https://pilot.weaverse.dev)
- Try customizing Pilot on [Weaverse Playground](https://playground.weaverse.io)
  ![demo](https://cdn.shopify.com/s/files/1/0693/8201/3220/files/Home.png?v=1695816170)

## What's included

- Remix
- Hydrogen
- Oxygen
- Shopify CLI
- ESLint
- Prettier
- GraphQL generator
- TypeScript and JavaScript flavors
- Tailwind CSS (via PostCSS)
- Full-featured setup of components and routes
- Fully customizable inside [Weaverse](https://weaverse.io)

## Getting started

Follow these steps to get started with Pilot and begin crafting your Hydrogen-driven storefront:

1. Install [Weaverse Hydrogen Customizer](https://apps.shopify.com/weaverse) from Shopify App Store.
2. Create new Hydrogen storefront inside Weaverse.
3. Initialize the project and start a local dev server with `@weaverse/cli` tool as instructed in the Weaverse editor.
   ![Init Weaverse Storefront](https://cdn.shopify.com/s/files/1/0693/8201/3220/files/init-project_4882deaa-c661-47cc-a7bf-38b2704e6a0b.png?v=1695816089)
4. Open the Weaverse editor to start customizing and tailoring your storefront according to your preferences.

## Features overview

### Fetching page data inside route's loader

Fetching page data inside route's loader is a common pattern in **Hydrogen**. **Weaverse** provides a convenient way to do that by using `context.weaverse.loadPage` function.

```ts:routes/($locale)/_index.tsx
import {defer} from '@shopify/remix-oxygen';
import {type RouteLoaderArgs} from '@weaverse/hydrogen';

export async function loader(args: RouteLoaderArgs) {
  let {params, context} = args;

  return defer({
    weaverseData: await context.weaverse.loadPage(),
    // More route's loader data...
  });
}
```

`weaverse` is an `WeaverseClient` instance that has been injected into the app context by Weaverse. It provides a set of methods to interact with the Weaverse API.

```ts:server.ts
let handleRequest = createRequestHandler({
  build: remixBuild,
  mode: process.env.NODE_ENV,
  getLoadContext: () => ({
    // App context props
    weaverse: createWeaverseClient({
      storefront,
      request,
      env,
      cache,
      waitUntil,
    }),
  }),
});
```

### Rendering page content

Weaverse pages is rendered using `<WeaverseContent />` component.

```tsx:app/weaverse/index.tsx
import {WeaverseHydrogenRoot} from '@weaverse/hydrogen';
import {GenericError} from '~/components/GenericError';
import {components} from './components';

export function WeaverseContent() {
  return (
    <WeaverseHydrogenRoot
      components={components}
      errorComponent={GenericError}
    />
  );
}

```

And in your route:

```tsx:routes/($locale)/_index.tsx
export default function Homepage() {
  return <WeaverseContent />;
}
```

