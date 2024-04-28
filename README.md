# template-tauri-sveltekit

This is a [Tauri v2](https://v2.tauri.app/) GitHub template, which uses [SvelteKit](https://kit.svelte.dev/) for its UI. The SvelteKit config in this template is the same as in [template-sveltekit-example](https://github.com/maiertech/template-sveltekit-example). It uses [JSDoc annotations](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) for TypeScript support.

## Pitfalls

This repository is a SvelteKit project with Tauri embedded into the `/src-tauri` folder. In combination with Tauri, SvelteKit's server-side features cannot be used. In a Tauri app, anything that SvelteKit would handle server-side, needs to be handled by Tauri's Rust backend. Therefore, this repository uses SvelteKit's [adapter-static](https://kit.svelte.dev/docs/adapter-static), which is normally used with these two page options (in `/src/routes/+layout.js`):

| Option             | Description                                 |
| :----------------- | :------------------------------------------ |
| `prerender = true` | Generate an HTML page for each route.       |
| `ssr = true`       | Server-side render all pages at build time. |

This combination renders pages during the build and results in static pages with SEO benefits as expected. But in combination with Tauri, you have to set `ssr = false`. Because Tauri requires acccess to `window`, which is not availalbe during SSR. With Tauri, you still get HTML pages for each route, but they are empty and rendered only on the client.

Read [The missing guide to understanding adapter-static in SvelteKit](https://khromov.se/the-missing-guide-to-understanding-adapter-static-in-sveltekit/) to fully understand the constraints of SvelteKit's adapter-static.

## Using this template

With this template you can develop desktop apps for the OS on which you run it. Familiarize yourself with the [Tauri CLI](https://v2.tauri.app/references/cli/). If you want to add support for iOS or Android, follow the instructions below.

## Adding support for iOS

## Adding support for Android

## Updating dependencies

Once you have started customizing the SvelteKit and Tauri apps, you cannot just re-generate the SvelteKit or Tauri apps. It is best to refer to this repository and check out
what may have changed. I update dependencies and configurations of this repository regularly.

### Updating SvelteKit

Follow the instructions in [template-sveltekit-example](https://github.com/maiertech/template-sveltekit-example) and regenerate the SvelteKit project into the same folder. Then deal with conflicts and make sure you preserve customizations.

### Updating Tauri

Anything Tauri is contained in `/src-tauri`. I use the Tauri CLI with `pnpm` to generate this folder. This requires `@tauri-apps/cli@next` to be installed. I ran `pnpm tauri init` with the following customizations:

- **What is your app name?** template-tauri-sveltekit
- **What should the window title be?** template-tauri-sveltekit
- **Where are your web assets located, relative to `/src-tauri/tauri.conf.json`?** `../build`, which is the default build directory for adapter-static.
- **What is the URL of your dev server?** `http://localhost:5173`
- **What is your frontend dev command?** `pnpm dev`
- **What is your frontend build command?** `pnpm build`
