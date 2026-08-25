# E00.5 — Roteiro de diagnóstico

## Roteiro

| Passo | Comando                                                             | Se a saída for X                                            | Então                                                                                             |
| ----- | ------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| 1     | `Get-Location` e `Get-ChildItem package.json`                       | O `package.json` não aparece na pasta atual                 | O comando provavelmente está sendo executado na pasta errada. Entrar na pasta correta do projeto. |
| 2     | `Get-Content package.json`                                          | O pacote não aparece em `dependencies` ou `devDependencies` | O pacote não foi declarado no projeto. Instalar ou adicionar a dependência corretamente.          |
| 3     | `Test-Path node_modules\prettier`                                   | Retorna `False`                                             | O pacote está declarado, mas não está instalado localmente. Prosseguir para confirmar a falha.    |
| 4     | `node -e "require('prettier'); console.log('prettier encontrado')"` | Aparece `Cannot find module 'prettier'`                     | A falha foi reproduzida e a hipótese de dependências ausentes foi confirmada.                     |
| 5     | `pnpm install --frozen-lockfile`                                    | A instalação termina sem erro                               | As dependências foram restauradas. Repetir o teste de import para confirmar a correção.           |

## Demonstração

A falha foi provocada removendo a pasta `node_modules`:

```powershell
Remove-Item -Recurse -Force node_modules
```

Em seguida, foi executado:

```powershell
node -e "require('prettier'); console.log('prettier encontrado')"
```

Saída:

```text
node:internal/modules/cjs/loader:1520
  throw err;
  ^

Error: Cannot find module 'prettier'
Require stack:
- C:\dev\dpw-exercicios\[eval]

code: 'MODULE_NOT_FOUND'
```

## Passo 1 — Verificar a pasta do projeto

Comando:

```powershell
Get-Location
```

Saída:

```text
Path
----
C:\dev\dpw-exercicios
```

Comando:

```powershell
Get-ChildItem package.json
```

Saída relevante:

```text
Diretório: C:\dev\dpw-exercicios

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        23/08/2026     21:21            206 package.json
```

O projeto e o `package.json` foram encontrados, então a hipótese de estar na pasta errada foi eliminada.

## Passo 2 — Verificar se o pacote está declarado

Comando:

```powershell
Get-Content package.json
```

Saída:

```json
{
  "name": "dpw-exercicios",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "verificar": "node --version && pnpm --version"
  },
  "devDependencies": {
    "prettier": "^3.9.6"
  }
}
```

O `prettier` aparece em `devDependencies`, então o pacote está corretamente declarado no projeto.

## Passo 3 — Verificar a instalação local

Comando:

```powershell
Test-Path node_modules\prettier
```

Saída:

```text
False
```

Isso mostrou que o pacote estava declarado, mas não estava instalado localmente.

## Passo 4 — Reproduzir a falha

Comando:

```powershell
node -e "require('prettier'); console.log('prettier encontrado')"
```

Saída relevante:

```text
Error: Cannot find module 'prettier'
Require stack:
- C:\dev\dpw-exercicios\[eval]

code: 'MODULE_NOT_FOUND'
```

A falha foi reproduzida e confirmou que a dependência não estava disponível no ambiente local.

## Passo 5 — Corrigir

Comando:

```powershell
pnpm install --frozen-lockfile
```

Saída:

```text
✓ Lockfile passes supply-chain policies (verified 1d ago)
Lockfile is up to date, resolution step is skipped
Packages: +1

Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: C:\Users\Adilson\AppData\Local\pnpm\store\v11
  Virtual store is at:             node_modules/.pnpm

Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 980ms using pnpm v11.23.0
```

Depois da instalação, o teste foi repetido:

```powershell
node -e "require('prettier'); console.log('prettier encontrado')"
```

Saída:

```text
prettier encontrado
```

A causa real era a ausência da pasta `node_modules`. O pacote estava corretamente declarado no `package.json`, mas as dependências não estavam instaladas no ambiente local.
