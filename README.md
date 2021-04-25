# docs-debug
Temporary repo to debug stuff here: https://github.com/squidfunk/mkdocs-material/discussions/2592

## steps to reproduce

```
git clone https://github.com/meleu/docs-debug
cd docs-debug/
docker container run --rm -v ${PWD}:/docs squidfunk/mkdocs-material build
```

Now, inside the `site/` dir, every `index.html` file that is inside a subdirectory, when they have a link to another pages, they are not linked to `../Another-Page`. Instead, they are linked to `Current-Page/Another-Page`.

A detailed example is described below.


## `nav:` section

In the [`mkdocs.yml`](mkdocs.yml) file, we have a `nav` like this:

```yml
nav:
  - General:
    - index.md
    - RetroAchievements Manifesto: RetroAchievements-Manifesto.md
  - Portugues:
    - Introdução: Home-pt_BR.md
    - Geral:
      - Manifesto RetroAchievements: RetroAchievements-Manifesto-pt_BR.md
```

## `.md` file

In [`docs.wiki/Home-pt_BR.md`](https://raw.githubusercontent.com/meleu/docs-debug/master/docs.wiki/Home-pt_BR.md) there's a line with this link:
```md
[Manifesto RetroAchievements](RetroAchievements-Manifesto-pt_BR)
```

## building the site

I've built the site with this command:
```
docker container run --rm -v ${PWD}:/docs squidfunk/mkdocs-material build
```


## the generated HTML file

In the generated [`Home-pt_BR/index.html`](https://github.com/meleu/docs-debug/blob/master/site/Home-pt_BR/index.html#L472) the generated link is:
```html
<a href="RetroAchievements-Manifesto-pt_BR">Manifesto RetroAchievements</a>
```

As you can see, the `href` points to `RetroAchievements-Manifesto-pt_BR`, and as we're in the `Home-pt_BR` dir, the link actually goes to `Home-pt_BR/RetroAchievements-Manifesto-pt_BR`

## how it was before

In an old installation of mkdocs-material, it converted that same link in that `.md` file like this:
```html
<a href="../RetroAchievements-Manifesto-pt_BR">Manifesto RetroAchievements</a>
```
(the file can be seen here: https://github.com/RetroAchievements/docs/blob/gh-pages/Home-pt_BR/index.html#L2348)

As you can see, the link automatically came prefixed with the `../`.
