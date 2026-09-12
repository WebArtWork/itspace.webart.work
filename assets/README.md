# Angular project v22 (NGX)

Angular 22 app built with standalone components, zoneless change detection, and WawJS `@wawjs/ngx-*` platform packages for routing meta, CRUD helpers, and signals-first state. The project ships with ready-to-use UI libraries, feature modules, and example pages for guest, public, user, and admin flows.

## Prerequisites

- Node 20+ and npm 10+ (Angular CLI 22 is provided locally via devDependencies)

## Custom GPT

https://chatgpt.com/g/g-68d25d311af08191be5aad28fca904b1-angular-web-art-work

## Getting Started

```sh
npm install          # install dependencies
npm start            # serves on http://localhost:4200 using proxy.conf.json
```

Environments live in `src/environments/`:

- `environment.ts` for local development (extends `environment.prod.ts`)
- `environment.prod.ts` for production builds (API URL, meta tags, languages, defaults)

## Scripts

- `npm start` - run the dev server with `proxy.conf.json`
- `npm run build` - production SSG build to `dist/app` (prerenders the static app shells)

## Project Structure (key paths)

- `src/app/app.config.ts` - root providers (zoneless change detection, WawJS package config, TinyMCE, router)
- `src/app/app.routes.ts` - route map for public, guest, user, and admin areas
- `src/app/app.config.server.ts` / `src/app/app.routes.server.ts` - SSG server providers and prerender route map
- `src/app/layouts/` - layout shells for public/guest/user routes
- `src/app/pages/` - routed pages per role (`guest/sign`, `public/landing`, `user/dashboard`, `user/profile`, `user/settings`)
- `src/app/modules/` - app-specific feature domains
- `src/app/temporary/` - short-lived local copies of `@wawjs/*` package code while validating package fixes before release
- `@wawjs/ngx-ui` - shared UI primitives (alert, button, input, modal, select, table, burger/material/theme icons)
- `@wawjs/ngx-bos` - shared BOS user, file, form-page, form-template, and icon features
- `@wawjs/ngx-form` - shared dynamic form services and contracts
- `@wawjs/ngx-map` - shared map and address lookup primitives
- `@wawjs/ngx-translate` - shared language and translation primitives
- `src/app/components/` - marketing/section blocks (hero, faq, pricing, showcase, trust bar, use cases, etc.)
- `src/environments/` - API/meta/language configuration

## Development Notes

- Components are standalone and use signals; favor `computed`/`signal`/`effect` plus OnPush change detection.
- WawJS package services power guards (`MetaGuard`), CRUD helpers, store/http access, and meta tags. Update `environment.meta` when changing branding.
- Shared dynamic form templates are provided by `@wawjs/ngx-bos`; schemas must match those registered names.
- Auth/user settings (roles, themes, defaults) and language options come from the environment files; update those when wiring to a new backend.
- If a UI or form fix belongs in a WawJS package but needs app-level testing first, copy the smallest relevant source into `src/app/temporary/`, note the origin package path, validate it here, then port the accepted change back to the package repo, release, update `package.json`, and remove the temporary copy.

## Component Structure

Keep component classes consistent in this order:

1. Injections (via `inject()`)
2. Inputs / outputs / view queries
3. Variables (readonly/public first, then private)
4. Constructor (only when needed)
5. Lifecycle hooks (`ngOnInit`, `ngOnDestroy`, etc.)
6. Functions (public, then private)

### Naming Conventions

- Private variables and functions start with an underscore (`_`).
- Services injected in constructors should follow:
    ```ts
    public configService: ConfigService
    private _configService: ConfigService
    ```

## WAW CLI Helpers

Scaffolding commands (requires global `waw` CLI):

- `waw add MODULENAME` - creates a module with interfaces, services, pages, selectors, and forms
- `waw page ROLE PAGENAME` - creates a page under a specific role
- `waw service SERVICENAME` - creates a service in the `services` folder

Examples:

```sh
waw add user
waw page user dashboard
waw service user
```

## Contributing

1. Fork and create a feature branch.
2. Keep changes aligned with the existing standalone + signals pattern.
3. Add or update validation steps where relevant.
4. Open a pull request.
5. Follow the coding guidelines outlined in this document.
6. Submit a pull request for review.
