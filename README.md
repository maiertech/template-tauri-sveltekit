# template-tauri-sveltekit

This is a [Tauri v2](https://v2.tauri.app/) GitHub template, which uses [SvelteKit](https://kit.svelte.dev/) for its UI. The SvelteKit config is the same as in [template-sveltekit-example](https://github.com/maiertech/template-sveltekit-example), i.e., SvelteKit is configured to use [JSDoc annotations](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) for TypeScript support.

## Adapting SvelteKit to Tauri

This repository is a SvelteKit project with Tauri embedded into the `/src-tauri` folder. In combination with Tauri, SvelteKit's server-side features cannot be used. In a Tauri app, anything that SvelteKit would handle server-side, needs to be handled by Tauri's Rust backend. Therefore, this repository uses SvelteKit's [adapter-static](https://kit.svelte.dev/docs/adapter-static). If you are not familiar with adapter-static, read [The missing guide to understanding adapter-static in SvelteKit](https://khromov.se/the-missing-guide-to-understanding-adapter-static-in-sveltekit/).

Adapter-static is normally used with these two page options set globally in your `/src/routes/layout.js`:

| Option             | Description                                 |
| :----------------- | :------------------------------------------ |
| `prerender = true` | Generate an HTML page for each route.       |
| `ssr = true`       | Server-side render all pages at build time. |

This combination renders pages during the build and results in static pages with SEO benefits as expected.

But in combination with Tauri, you have to set `ssr = false`. This is because Tauri requires acccess to `window`, which is not availalbe during SSR. With Tauri, you still get HTML pages for each route, but they are an empty shell and rendered only on the client.
