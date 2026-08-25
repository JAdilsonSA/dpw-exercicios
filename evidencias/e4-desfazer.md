# E00.4 — Desfazer sem pânico

## Tabela dos cenários

| # | Cenário                                            | Comando utilizado                                         |
| - | -------------------------------------------------- | --------------------------------------------------------- |
| 1 | Descartar alteração ainda não adicionada ao stage  | `git restore README.md`                                   |
| 2 | Remover arquivo adicionado por engano do stage     | `git restore --staged README.md`                          |
| 3 | Corrigir a mensagem do último commit antes do push | `git commit --amend -m "test: demonstra amend de commit"` |
| 4 | Desfazer o último commit mantendo as alterações    | `git reset --soft HEAD~1`                                 |
| 5 | Reverter um commit já enviado para o remoto        | `git revert HEAD`                                         |

## Caso 1 — Descartar alteração antes do git add

### Antes

Comando:

```powershell
git status
```

Saída:

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Comando utilizado:

```powershell
git restore README.md
```

### Depois

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

## Caso 2 — Remover arquivo do stage

Após alterar o `README.md`, o arquivo foi adicionado ao stage:

```powershell
git add README.md
```

### Antes

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   README.md
```

Comando utilizado:

```powershell
git restore --staged README.md
```

### Depois

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

## Caso 3 — Corrigir a mensagem do último commit

Foi criado inicialmente o commit com a mensagem incorreta:

```powershell
git commit -m "mensagem errada"
```

### Antes

Comando:

```powershell
git log --oneline -3
```

Saída:

```text
4d1eb29 (HEAD -> main) mensagem errada
489a0c6 (origin/main) docs: adiciona evidencia do conflito de merge
344c69a merge: resolve conflito de titulo
```

A mensagem foi corrigida com:

```powershell
git commit --amend -m "test: demonstra amend de commit"
```

### Depois

```text
cddb7e0 (HEAD -> main) test: demonstra amend de commit
489a0c6 (origin/main) docs: adiciona evidencia do conflito de merge
344c69a merge: resolve conflito de titulo
```

O hash mudou de `4d1eb29` para `cddb7e0`, pois o `amend` substituiu o commit anterior por um novo commit.

## Caso 4 — Desfazer o último commit mantendo as alterações

### Antes

```text
cddb7e0 (HEAD -> main) test: demonstra amend de commit
489a0c6 (origin/main) docs: adiciona evidencia do conflito de merge
344c69a merge: resolve conflito de titulo
```

Comando utilizado:

```powershell
git reset --soft HEAD~1
```

### Depois

```text
489a0c6 (HEAD -> main, origin/main) docs: adiciona evidencia do conflito de merge
344c69a merge: resolve conflito de titulo
5417e50 (origin/feat/titulo-b, feat/titulo-b) feat: altera titulo para versao B
```

O arquivo continuou preservado no stage:

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   caso3.txt
```

## Caso 5 — Reverter um commit já enviado ao remoto

Foi criado e enviado o commit:

```text
2edd9ba test: adiciona arquivo para demonstrar revert
```

### Antes

Comando:

```powershell
git log --oneline -3
```

Saída:

```text
2edd9ba (HEAD -> main, origin/main) test: adiciona arquivo para demonstrar revert
489a0c6 docs: adiciona evidencia do conflito de merge
344c69a merge: resolve conflito de titulo
```

O commit já havia sido enviado ao GitHub. Para desfazê-lo foi utilizado:

```powershell
git revert HEAD
```

### Depois

```text
7606bc5 (HEAD -> main, origin/main) Revert "test: adiciona arquivo para demonstrar revert"
2edd9ba test: adiciona arquivo para demonstrar revert
489a0c6 docs: adiciona evidencia do conflito de merge
```

O `git revert` não apagou o commit anterior. Em vez disso, criou um novo commit que desfaz suas alterações.

## Reflog

Comando:

```powershell
git reflog -10
```

Saída:

```text
7606bc5 (HEAD -> main, origin/main) HEAD@{0}: revert: Revert "test: adiciona arquivo para demonstrar revert"
2edd9ba HEAD@{1}: commit: test: adiciona arquivo para demonstrar revert
489a0c6 HEAD@{2}: reset: moving to HEAD~1
cddb7e0 HEAD@{3}: commit (amend): test: demonstra amend de commit
4d1eb29 HEAD@{4}: commit: mensagem errada
489a0c6 HEAD@{5}: commit: docs: adiciona evidencia do conflito de merge
344c69a HEAD@{6}: commit (merge): merge: resolve conflito de titulo
da5252a (origin/feat/titulo-a, feat/titulo-a) HEAD@{7}: checkout: moving from feat/titulo-b to main
5417e50 (origin/feat/titulo-b, feat/titulo-b) HEAD@{8}: commit: feat: altera titulo para versao B
4206c8d HEAD@{9}: checkout: moving from main to feat/titulo-b
```

## Link permanente do commit de revert

[Ver commit de revert no GitHub](https://github.com/JAdilsonSA/dpw-exercicios/commit/7606bc5cf78eee688c50a37c18388795bb21f319)

## Por que o caso 5 é diferente do caso 4?

O caso 4 usa `reset` para mover o `HEAD` e reescrever o histórico local, mantendo as alterações para continuar trabalhando.
O caso 5 usa `revert` para criar um novo commit que desfaz outro commit já publicado, preservando o histórico existente.
Reescrever um commit que já foi enviado pode quebrar o repositório de quem já o baixou, por isso o `revert` é a opção segura nesse caso.
