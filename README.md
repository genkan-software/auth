# genkan-auth

Login, sessions (Redis), users, first-admin seeding.

Part of Genkan. Architecture, endpoints and the full stack: see the **genkan-setup** repo.

## Layout

```
src/
  Genkan.Auth.Domain/
  Genkan.Auth.Infrastructure/
  Genkan.Auth.Application/
  Genkan.Auth.Api/
Genkan.Auth.slnx
```

## Build / run

`Genkan.Shared` comes from GitHub Packages, so restore needs a token with `read:packages`:

```bash
export GITHUB_PACKAGES_TOKEN=ghp_xxx
dotnet build Genkan.Auth.slnx
docker build --secret id=github_token,env=GITHUB_PACKAGES_TOKEN -t genkan-auth .
```

## Configuration (env)

`ConnectionStrings__Postgres`, `ConnectionStrings__Redis`, `Seed__AdminEmail`, `Seed__AdminPassword`. The database schema is owned by **genkan-migrator** (run it first).

Note: the old `Firebase` DI extension that lived here was unused and referenced the notification projects; it was removed when the repos were split.

## CI / release

- push to `main` -> image `ghcr.io/genkan-software/genkan-auth:main`, `:sha-xxxxxxx`, `:latest`
- tag `vX.Y.Z` -> also `:X.Y.Z` and `:X.Y`. Normally created for you: tagging **genkan-setup** releases every service with the same version.
- Pull requests only build.

Secrets: if `GITHUB_TOKEN` cannot read the `Genkan.Shared` package, either grant this repo access in the package settings
(Package settings > Manage Actions access) or add a `PACKAGES_TOKEN` secret (PAT, `read:packages`).
Dependabot (`.github/dependabot.yml`) opens a PR when a new `Genkan.Shared` is published; it needs `PACKAGES_TOKEN` as a *Dependabot* secret too.
