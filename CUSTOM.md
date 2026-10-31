# Personal OpenBot on this Mac

This fork provides the Mac interface for the user's existing Surface Server.
The Surface stays on the official release. The custom app uses the released server API.

## Working branches

`main` is the default branch of this fork and contains the personal app setup. Start each new
feature branch from `main`, for example:

```sh
git switch main
git switch -c feature/working-animation
```

Merge completed changes into `main`. `upstream-main` preserves the fork's previous official
code snapshot. The `upstream` remote points to the official repository for future updates.

## Installed app

- App: `/Applications/OpenBot.app`
- Bundle ID: `app.openbot.custom`
- Settings: `~/Library/Application Support/OpenBot Custom`
- Official fallback: `/Applications/Original OpenBot.app`, with its original settings folder

Keep the runtime `app.setName("OpenBot")` call. It preserves access to the existing macOS
secure-storage key. The bundle ID identifies the custom app. Keep `CFBundleName` equal to the
executable name, `OpenBot`, so Electron can find its helpers. Keep the existing `OpenBot Custom`
settings folder so the name change preserves the account session and preferences.

The custom account needs its own desktop sign-in session. Copying the official account session
can make the two clients replace each other's Surface connection. Keep the remote session caches
separate. A rebuild keeps the custom profile; it needs no further settings transfer or sign-in.

## Build and install

The project pins Bun and Node.js in `.prototools`. Run these commands from this checkout:

```sh
bun install --frozen-lockfile
bun run package
bun run verify:package
```

Quit the installed custom app before replacing it. Then install and open the build:

```sh
ditto "dist/mac-arm64/OpenBot.app" "/Applications/OpenBot.app"
open -a "/Applications/OpenBot.app"
```

Use `dist/mac` on an Intel Mac. The installed build does not need this checkout, Bun, Node.js,
or a development server to run. Raycast can launch it as `OpenBot`. The official app is named
`Original OpenBot`. Its signed contents are unchanged.

`publish: null` keeps official update metadata out of the package. `verify:package` checks the
custom identity, absence of that metadata, native resources, Electron fuses, and app launch.
The custom app's updater reports `unsupported`. Update this app by rebuilding the fork.

The app is signed locally for personal use. It does not use the official publisher's Apple team
identity or its associated-domain entitlement. Keep Electron sandboxing and the existing fuses.

Local verification reports and screenshots are in the ignored `.openbot-build/custom` folder.
