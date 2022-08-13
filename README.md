# Insights - Software Development Insights by Groosg

This repo hosts the code for https://groosg.github.io/insights/.

It uses [MkDocs](https://www.mkdocs.org/getting-started/) and [Material for
MkDocs](https://squidfunk.github.io/mkdocs-material/getting-started/).

## Local development

- Add content to `/docs`.
- Edit configurations and add your content to the navigation by changing
`mkdocs.yml`

### Evaluate your changes locally

The simplest way to check your changes is to use the Docker image so you don't
need to install any Python module on your machine. You can do it with:

```shell
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```

If you prefer to install things locally, use the
[Material for MkDocs installation](https://squidfunk.github.io/mkdocs-material/getting-started/#installation)
since it will get all the dependencies, including MkDocs, installed.
