# E00.3 — Conflito de merge

## 1. Saída do merge que gerou o conflito

Comando executado:

```powershell
git merge feat/titulo-b
```

Saída:

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

## 2. Conteúdo do README.md durante o conflito

Antes de resolver o conflito, o arquivo `README.md` apresentou os seguintes marcadores:

```text
<<<<<<< HEAD
# DPW — Exercícios do M00 — Versão A
=======
# DPW — Exercícios do M00 — Versão B
>>>>>>> feat/titulo-b
```

O conflito foi resolvido manualmente e os marcadores foram removidos do arquivo final.

## 3. Grafo do histórico

Comando executado:

```powershell
git log --graph --oneline --all
```

Saída:

```text
*   344c69a (HEAD -> main) merge: resolve conflito de titulo
|\
| * 5417e50 (feat/titulo-b) feat: altera titulo para versao B
* | da5252a (feat/titulo-a) feat: altera titulo para versao A
|/
* 4206c8d (origin/main) docs: adiciona evidencia de arqueologia de historico
* f6d45fc docs: adiciona evidencia do ambiente reprodutivel
* 4a1f664 feat: configura ambiente reprodutível
* 5bdbd98 Fix formatting issues in README.md
* e4beec7 Add initial README with exercises and instructions
* 54da26d Estrutura inicial
```

## 4. Verificação dos dois pais do merge

Comando executado:

```powershell
git show --format=%P -s 344c69a
```

Saída:

```text
da5252ab1e001ac983bab0e7fad14b8138a03a73 5417e507c17f4f64479bc2a8e12c44bfcfe3ca23
```

A presença de dois hashes confirma que `344c69a` é um commit de merge com dois pais.

## 5. Links

[Commit de merge](https://github.com/JAdilsonSA/dpw-exercicios/commit/344c69a691d1711c66fce43ebf3ae83b372f3155)

[Grafo de branches e merges](https://github.com/JAdilsonSA/dpw-exercicios/network)

## 6. Por que o Git não conseguiu resolver sozinho?

O Git não conseguiu resolver automaticamente porque a mesma linha do `README.md` foi alterada nas duas branches.
Cada branch possuía uma versão diferente da mesma linha.
Como as alterações ocorreram no mesmo trecho, o Git não conseguiu decidir qual versão deveria permanecer.
