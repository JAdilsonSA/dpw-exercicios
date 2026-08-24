# E00.2 — Arqueologia de histórico

Repositório analisado: `nestjs/nest`

## 1. Quantos commits o repositório tem?

Comando:

```powershell
git rev-list --count HEAD
```

Saída:

```text
21648
```

## 2. Qual foi o primeiro commit, e em que data?

Comando:

```powershell
git log --reverse --format="%H | %ad | %an | %s" --date=short | Select-Object -First 1
```

Saída:

```text
f7c8d10fb20943bc7102c73d5ecbe49e6c0b5ea1 | 2017-01-08 | kamil.mysliwiec | Initial commit
```

O primeiro commit foi realizado em `2017-01-08`.

## 3. Quem mais modificou `packages/core/injector/injector.ts`?

Comando:

```powershell
git shortlog -sn -- packages/core/injector/injector.ts
```

Saída:

```text
88  Kamil Myśliwiec
12  Jay McDoniel
6   Kamil Mysliwiec
4   Jean-Baptiste Pionnier
4   Livio Brunner
3   Micael Levi (lab)
2   Jiri Hajek
2   Micael Levi L. Cavalcante
2   mag123c
```

O autor que mais modificou o arquivo foi `Kamil Myśliwiec`, com 88 commits.

## 4. O que mudou no último commit que tocou esse arquivo?

Comando:

```powershell
git log -1 -p --first-parent -- packages/core/injector/injector.ts
```

Saída relevante:

```text
commit 09dba60ce8dd47f9e6c518a86e2ac3cefdb6d68f
Merge: 4535f43b4 edb003497
Author: Kamil Mysliwiec <mail@kamilmysliwiec.com>
Date:   Fri Aug 14 19:57:59 2026 +0200

    Merge pull request #16391 from nestjs/v12.0.0

    release: v12.0.0 major release (approx. Q3 2026)

diff --git a/packages/core/injector/injector.ts b/packages/core/injector/injector.ts
index d4e6145f8..ee21d30e8 100644
--- a/packages/core/injector/injector.ts
+++ b/packages/core/injector/injector.ts
@@ -1,49 +1,44 @@
+import type { ForwardReference, Type } from '@nestjs/common';
 import {
-  InjectionToken,
+  type InjectionToken,
   Logger,
-  LoggerService,
-  OptionalFactoryDependency,
+  type LoggerService,
+  type OptionalFactoryDependency,
   Scope,
 } from '@nestjs/common';
```

No trecho exibido, houve reorganização dos imports do arquivo e vários tipos passaram a ser importados explicitamente com `type`, como `ForwardReference`, `Type`, `InjectionToken`, `LoggerService` e `OptionalFactoryDependency`.

## 5. Quantos commits foram feitos nos últimos 90 dias?

Comando:

```powershell
git rev-list --count --since="90 days ago" HEAD
```

Saída:

```text
705
```
