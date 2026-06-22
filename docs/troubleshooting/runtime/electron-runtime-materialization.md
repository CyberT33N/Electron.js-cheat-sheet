# Workspace Electron Runtime Materialization
[INTENT: CONTEXT]

This convention defines the workspace-level repair and verification procedure for Electron runtime
materialization in the shared monorepo.

It exists because the relevant failure is not application-local feature logic. It is a cross-cutting
install-domain and runtime-governance issue that touches:

- the root pnpm install domain
- the Node `26.2.0` workspace control plane
- the embedded Electron runtime line
- the shared runtime probe under `tools/runtime-exec/`
- the `test` smoke guard owned by `packages/test/tooling`

## Why This Lives In `docs/conventions/workspace`
[INTENT: REFERENZ]

This document belongs in the root `docs/conventions/workspace/` folder instead of an app-local
documentation area.

Reason:

- the root workspace owns pnpm installation and lockfile materialization
- the failure happens after workspace-level install and rebuild operations
- the Electron package may resolve successfully at the JavaScript layer while the native runtime
  payload is still incomplete
- the repair flow depends on the relationship between the Node `26` control plane and the embedded
  Node `20` Electron runtime line

Therefore the authoritative owner is the workspace runtime/install governance surface, not only
`apps/test/desktop`.

## Current Runtime Truth
[INTENT: REFERENZ]

The current approved runtime facts are:

- workspace host-node control plane: Node `26.2.0`
- root package-manager contract: `pnpm@11.7.0`
- Electron package line for `test`: `29.4.6`
- embedded Electron Node runtime: `20.9.0`
- `@types/node` family for the Electron app: major `20`

This is the key architectural asymmetry:

- the workspace is intentionally maintained on Node `26.2.0`
- the Electron application runtime is intentionally pinned lower through Electron itself

That means a normal workspace install may be valid for the control plane while the Electron runtime
payload still needs a compatibility-runtime repair step.

## Canonical Surfaces
[INTENT: REFERENZ]

The relevant authoritative surfaces are:

- root `package.json`
- root `pnpm-workspace.yaml`
- `apps/test/desktop/package.json`
- `packages/test/tooling/scripts/smoke-electron-runtime.mjs`
- `tools/runtime-exec/electron/resolve-electron-runtime.mjs`
- the currently resolved Electron package under `.pnpm/.../electron/.../install.js`

## Failure Shape
[INTENT: REFERENZ]

The failure class covered by this document is:

1. `pnpm install` or `pnpm rebuild electron` appears to complete or mostly complete
2. `electron/cli.js` is still resolvable from the application package
3. but the Electron runtime smoke probe still fails
4. because the native Electron payload was only partially materialized

This means the problem is not "Electron is absent".

The actual problem is:

- `path.txt` can be missing
- `dist/electron.exe` can be missing
- only a partial `dist/` tree can exist, for example `locales/`

## Required Symptoms To Recognize
[INTENT: REFERENZ]

Treat all of the following as first-class signals of this failure mode:

- `The Electron runtime probe could not start because the Electron binary is not materialized correctly in node_modules. Re-run "pnpm install" from the workspace root and then retry.`
- `Electron failed to install correctly, please delete node_modules/electron and try installing again`
- `electron/cli.js` resolves but the runtime still cannot launch
- `path.txt exists=false`
- `dist/electron.exe exists=false`
- `dist/` exists but contains only a partial subtree such as `locales/`
- `pnpm rebuild electron` returns without producing a healthy runtime tree
- direct `node install.js` under the workspace Node `26.2.0` surface returns without producing a
  healthy runtime tree

Important interpretation:

- a resolvable `electron/cli.js` is **not** proof of a healthy runtime
- a non-crashing `pnpm rebuild electron` is **not** proof of a healthy runtime

## Mandatory Verification Before Any Repair Claim
[INTENT: ANWEISUNG]

Before claiming that Electron is healthy, the package state must be checked explicitly.

Run this from the workspace root:

```powershell
node -e "const fs=require('node:fs'); const path=require('node:path'); const {createRequire}=require('node:module'); const appRoot=path.resolve(process.cwd(),'apps','test','desktop'); const req=createRequire(path.resolve(appRoot,'package.json')); const electronCli=req.resolve('electron/cli.js'); const electronDir=path.dirname(electronCli); const pathTxt=path.join(electronDir,'path.txt'); const distDir=path.join(electronDir,'dist'); const exe=path.join(distDir,'electron.exe'); console.log('electronCli=' + electronCli); console.log('electronDir=' + electronDir); console.log('path.txt exists=' + fs.existsSync(pathTxt)); if (fs.existsSync(pathTxt)) { const rel=fs.readFileSync(pathTxt,'utf8').trim(); console.log('path.txt content=' + rel); console.log('binary from path.txt exists=' + fs.existsSync(path.join(distDir, rel))); } console.log('dist dir exists=' + fs.existsSync(distDir)); console.log('electron.exe exists=' + fs.existsSync(exe));"
```

Healthy interpretation:

- `path.txt exists=true`
- `path.txt content=electron.exe`
- `binary from path.txt exists=true`
- `dist dir exists=true`
- `electron.exe exists=true`

Broken interpretation:

- `electronCli` exists but one or more of the runtime payload checks above is false

## Mandatory Repair Order
[INTENT: ANWEISUNG]

The repair order is fixed. Do not skip directly to improvised mutations.

### Step 1 - Baseline install-domain repair

Run the normal workspace baseline first:

```powershell
pnpm install
```

Then run the smoke guard:

```powershell
pnpm exec nx run test:smoke-electron-runtime
```

If the smoke guard succeeds, stop here.

### Step 2 - Explicit package-state verification

If the smoke guard fails, run the package-state verification block from the previous section.

### Step 3 - One explicit Electron rebuild attempt

If the runtime tree is incomplete, run:

```powershell
pnpm rebuild electron
```

Then run the verification block again.

If `path.txt` and `dist/electron.exe` are still missing, do **not** loop on rebuild blindly.

### Step 4 - Resolve the current `install.js`

The manual repair must target the currently resolved Electron package, not a guessed path.

From the workspace root:

```powershell
$appRoot = Join-Path (Get-Location) 'apps\test\desktop'
$electronInstall = node -e "const path=require('node:path'); const {createRequire}=require('node:module'); const appRoot=path.resolve(process.argv[1]); const req=createRequire(path.resolve(appRoot,'package.json')); const electronCli=req.resolve('electron/cli.js'); process.stdout.write(path.join(path.dirname(electronCli),'install.js'));" "$appRoot"
$electronInstall
```

### Step 5 - Set the Electron cache explicitly

Use the host cache explicitly so the repair path does not drift onto an unknown cache location.

```powershell
$env:ELECTRON_CACHE = Join-Path $env:LOCALAPPDATA 'electron\Cache'
$env:electron_config_cache = $env:ELECTRON_CACHE
```

### Step 6 - If rebuild and Node 26 repair both fail, use the compatibility runtime

This is the decisive rule for this workspace:

- if `path.txt` is still missing after `pnpm rebuild electron`
- and direct repair under the Node `26.2.0` control-plane surface still does not materialize the
  runtime
- then that is the signal to run the manual Electron installer with the matching compatibility
  runtime for the embedded Electron line

For the current repository state, that compatibility line is Node `20.9.0`.

Use either an explicit Node 20 executable or an equivalent `node20` wrapper if your environment
already provides one.

Example with an explicit Windows Node 20 executable:

```powershell
$node20 = 'C:\Users\<user>\AppData\Local\nvm\v20.9.0\node.exe'
& $node20 $electronInstall
```

Why this is mandatory:

- Electron `29.4.6` embeds Node `20.9.0`
- the workspace control plane is intentionally on Node `26.2.0`
- the validated repair path for the partially materialized Electron payload is the compatibility
  runtime, not another generic Node `26` rebuild loop

### Step 7 - Re-verify package state

After the compatibility-runtime install finishes, rerun the verification block.

Required healthy state:

- `path.txt exists=true`
- `path.txt content=electron.exe`
- `binary from path.txt exists=true`
- `dist dir exists=true`
- `electron.exe exists=true`

### Step 8 - Re-run the smoke guard

```powershell
pnpm exec nx run test:smoke-electron-runtime
```

Required healthy result:

```text
[smoke-electron-runtime] ok electron=29.4.6 node=20.9.0 @types/node=20.19.41
```

## Why The Compatibility Runtime Repair Is Correct
[INTENT: CONTEXT]

The workspace must **not** respond to this problem by lowering the entire control plane from Node
`26.2.0` to Node `20`.

That would be the wrong architectural boundary.

The correct interpretation is:

- Node `26.2.0` remains the workspace control-plane truth
- Electron `29.4.6` remains the application runtime truth
- the manual `install.js` repair is a targeted compatibility-runtime operation for the Electron
  payload only

This is a narrow repair to a broken runtime materialization state, not a justification to collapse
the workspace onto the lower runtime line.

## Escalation If Compatibility Repair Still Fails
[INTENT: REFERENZ]

If the compatibility-runtime repair still does not materialize `path.txt` and `dist/electron.exe`,
the next escalation is no longer "keep rebuilding".

At that point the runtime payload should be treated as an Electron binary provisioning problem.

The next escalation surface is:

- verify the cached Electron ZIP exists under `%LOCALAPPDATA%\electron\Cache`
- if necessary, mirror or extract the runtime payload explicitly into the expected Electron runtime
  location
- if the project adopts the stronger host-sync fallback, launch through an explicit
  `ELECTRON_EXEC_PATH`

That is a secondary escalation. It is **not** the first repair step while the compatibility-runtime
installer path has not yet been attempted.

## Invalid States
[INTENT: SPECIFICATION]

| Invalid state | Why it is invalid | Required remediation |
| --- | --- | --- |
| Treating a resolvable `electron/cli.js` as proof that Electron is healthy | The JavaScript package can resolve while the native runtime payload is still incomplete. | Verify `path.txt`, `dist/`, and `dist/electron.exe` explicitly. |
| Repeating `pnpm rebuild electron` indefinitely under Node `26.2.0` after the package-state check already proved the runtime is still incomplete | It ignores the validated compatibility-runtime repair path for this workspace. | Resolve `install.js` and run it with the embedded-runtime-compatible Node `20.9.0` surface. |
| Lowering the whole workspace control plane from Node `26.2.0` to Node `20` just to repair Electron | It collapses two intentionally separate runtime authorities into one weaker control plane. | Keep the workspace on Node `26.2.0` and run only the Electron payload repair on Node `20.9.0`. |
| Adding app-local pnpm or host-runtime governance fields to `apps/test/desktop/package.json` to work around Electron materialization | It puts workspace-control-plane concerns into the pure Electron app package. | Keep pnpm governance at the root and keep the Electron package runtime truth at Electron plus the smoke guard. |
| Claiming the fix is complete without rerunning `test:smoke-electron-runtime` | The repair goal is a healthy runtime probe, not only a changed file tree. | Re-run the smoke guard and require the healthy success marker. |
| Jumping directly to ad-hoc binary copying before the compatibility-runtime `install.js` repair has been attempted | It skips the validated repair order and makes the root cause harder to reason about. | Follow the fixed repair order: install, verify, rebuild, verify, compatibility-runtime `install.js`, verify, smoke. |
