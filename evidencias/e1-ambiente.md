# E00.1 — Ambiente reprodutível

## Prova de reprodutibilidade

Os seguintes comandos foram executados no PowerShell:

```powershell
Remove-Item -Recurse -Force node_modules
pnpm install --frozen-lockfile
git status --short
```

Saída obtida:

```text
PS C:\dev\dpw-exercicios> Remove-Item -Recurse -Force node_modules
PS C:\dev\dpw-exercicios> pnpm install --frozen-lockfile
✓ Lockfile passes supply-chain policies (verified 11m ago)
Lockfile is up to date, resolution step is skipped
Packages: +1
+

Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: C:\Users\Adilson\AppData\Local\pnpm\store\v11
  Virtual store is at:             node_modules/.pnpm

Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 553ms using pnpm v11.23.0

PS C:\dev\dpw-exercicios> git status --short
PS C:\dev\dpw-exercicios>
```

O comando `git status --short` não apresentou nenhuma saída, mostrando que nenhum arquivo versionado foi alterado após a reinstalação das dependências.

## .gitignore

Link permanente para o `.gitignore`:

`https://github.com/JAdilsonSA/dpw-exercicios/blob/4a1f664c6318a48b00934cc17623efa11e2658a4/.gitignore#L1`


