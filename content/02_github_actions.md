+++
title = "Issue #002 — GitHub Actions + CI/CD na prática"
date = 2026-10-05

[taxonomies]
categories = ["github","zola"]
tags = ["github"]
 
[extra]
toc = true
+++
Subindo um site Zola para o GitHub.

<!-- more -->

## Intro:

Eu tinha um site rodando numa VPS com Nginx, mas resolvi migrar para o GitHub Pages.
A ideia parecia simples: criar um repositório, subir o site e pronto. Mas logo surge o primeiro problema: o site usava um tema antigo do Radion, e após instalar  a versão recente do Zola rodei o `$ zola build` resultando em: `ERROR: Found `:` but expected }}.`. 
O Zola estava olhando o tema e dizendo: "Eu não compreendo esse dialeto", então em vez de atualizar todo o site, voltei para a versão que já funcionava o Zola 0.21.0 e dessa vez o site voltou a funcionar!! :P

## WORKFLOW:
O GitHub Actions usa um arquivo chamado **workflow** escrito em YAML para definir o que deve acontecer. 

No meu site, ele fica em: `.github/workflows/deploy.yml`.

O `deploy.yml` é basicamente a receita do bolo: diz quando e como o GitHub deve executar cada etapa.

Dessa forma, o comando `git push` dispara todo o processo automaticamente.

## RUNNER:
O workflow precisa de uma máquina para executar os comandos:

```yaml
runs-on: ubuntu-latest
```

O GitHub fornece uma máquina virtual temporária chamada **runner**.
O Ubuntu do runner é apenas o ambiente de trabalho do pipeline.

> [!NOTE]
> GitHub-hosted runner = é uma máquina virtual temporária disponibilizada pelo GitHub para executar o workflow.

## BUILD:
O Zola transforma Markdown, templates e tema em arquivos estáticos.

O conteúdo do **public/** gerado pelo Zola é empacotado como **artifact**, que será entregue ao GitHub Pages.

## DEPLOY:
O **artifact** é entregue ao GitHub Pages.

O **runner** não é o servidor do site, é apenas a máquina usada para construir o site. Por isso ele pode ser descartado, pois ele só construiu o site mas quem mantém o site publicado é o GitHub Pages.

Assim como o Zola não fica instalado no GitHub Pages, ele só é necessário para gerar os arquivos, ou seja, para construir o site. O que fica publicado são esses arquivos gerados pelo **zola build**.

## PIPELINE:
O pipeline é o processo definido pelo `deploy.yml`. Dentro dele, temos as etapas: 

> git push → GitHub Actions → Runner Ubuntu → checkout (pega o código) → instalar o Zola (prepara o ambiente) → zola build → public/ → artifact (empacota o public/) → deploy (envia o artifact para o GitHub Pages)

> [!NOTE]
> Pipeline = É uma sequência automatizada de etapas executadas em ordem, que transforma o código em algo pronto para entrega ou publicação.

Resumindo em uma linha:
> [!TIP]
> git push → GitHub Actions → Runner Ubuntu → Instala o Zola 0.21.0 → roda o zola build → gera o public/ → envia como artifact → GitHub Pages → site online → Runner descartado

## CI/CD:
> [!NOTE]
> CI (Continuous Integration) = integrar, construir e verificar.
> CD (Continuous Delivery/Deployment) = entregar e publicar.

## Resultado:
A ideia acabou simplificando o processo: 

Antes: meu computador → VPS → rm -rf .zola public → zola build → sudo rm -rf /var/www/kageops.tech/*  → sudo cp -r public/* /var/www/kageops.tech/ → site no ar
                 
Agora: meu computador → git push 

O `git push` já faz todo o processo: git push → GitHub Actions → Zola → artifact → GitHub Pages

Uma VPS a menos para administrar.
Um pipeline a mais para entender.

**END OF FILE**
