---
tags:
  - notes
---

### Pós-projeto: workflow de branches (não forks)

**Lógica:** você trabalha numa branch separada durante a semana, sem tocar na `main`. No fim da semana, revisa o que mudou e decide: mescla (se valeu a pena) ou descarta (se foi bagunça/experimento).

**Início da semana:**

powershell

```powershell
git checkout main
git pull
git checkout -b semana-13-07
```

A partir daqui, o plugin Obsidian Git (ou você manualmente) commita normalmente — mas tudo fica na branch `semana-13-07`, isolado da `main`.

**Fim da semana — revisar antes de decidir:**

powershell

```powershell
git diff main semana-13-07
```

Isso mostra tudo que mudou na semana comparado à `main`. Dá pra revisar nota por nota.

**Se valeu a pena → mesclar:**

powershell

```powershell
git checkout main
git merge semana-13-07
git push
git branch -d semana-13-07
```

**Se não valeu (bagunça, experimento que não quer manter) → descartar:**

powershell

```powershell
git checkout main
git branch -D semana-13-07
```

A `main` nem fica sabendo que aquilo existiu.

**Por que não fork aqui:** fork exigiria um repo GitHub inteiro à parte, e pra "mesclar de volta" você precisaria abrir um Pull Request contra seu próprio repo — passo extra sem benefício nenhum pra um projeto solo, de um dispositivo só. Branch resolve o mesmo problema com metade da complexidade.