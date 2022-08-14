---
card:
    title: "Twitter Card with Material for MkDocs"
    description: "This tutorial shows how to implement a Twitter card with Material for MkDocs."
---

# Twitter Card with Material for MkDocs

This tutorial shows how to implement a Twitter card with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Material for MkDocs [supports social cards](https://squidfunk.github.io/mkdocs-material/setup/setting-up-social-cards/) in the [Insiders](https://squidfunk.github.io/mkdocs-material/insiders/#what-is-insiders) model, which is a version with a set of features exclusive for sponsors. Since I'm just an average Joe trying to share my stuff on Twitter, I decided to take a stab and implement the Twitter card myself.

Checking the [Twitter documentation](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started) you can see that it's nothing else than a set of HTML meta tags and the Material for MkDocs documentation even [gives you the path](https://squidfunk.github.io/mkdocs-material/setup/setting-up-social-cards/#meta-tags) with the template variables that you need to use to fill these meta tags.

!!! tip "Consider sponsoring Material for MkDocs"
    If your company is using Material for MkDocs consider becoming a sponsor to get access to the hot features instead of using hacks such as the one presented here :wink:

## Create an override block

The first thing we need to do is create an [override block](https://squidfunk.github.io/mkdocs-material/customization/?h=#overriding-blocks) that allows adding content to the HTML head tag, for this we use the `extrahead` block.

```html
# theme/main.html
{% extends "base.html" %}

{% block extrahead %}
  {% if config.extra.card %}
      <!-- begin:twitter card -->
      <meta name="twitter:card" content="{{ config.extra.card.type | e }}" />
      <meta name="twitter:site" content="{{ config.extra.card.twitter | e }}" />
      <meta name="twitter:creator" content="{{ config.extra.card.twitter | e }}" />
      <meta property="og:url" content="{{ page.canonical_url }}" />
      {% if page and page.meta.card and page.meta.card.title %}
      <meta property="og:title" content="{{ page.meta.card.title }}" />
      {% else %}
      <meta property="og:title" content="{{ page.title }}" />
      {% endif %}
      {% if page and page.meta.card and page.meta.card.description %}
      <meta property="og:description" content="{{ page.meta.card.description }}" />
      {% else %}
      <meta property="og:description" content="{{ config.site_description }}" />
      {% endif %}
      <meta property="og:image" content="{{ [config.site_url, config.extra.card.image] | join }}" />
      <meta property="og:image:type" content="image/png">
      <!-- end:twitter card -->
  {% endif %}
{% endblock %}

```

Note that the `main.html` file goes under the path defined in the [custom_dir](https://www.mkdocs.org/user-guide/configuration/#custom_dir) configuration in the `mkdocs.yml` file, which in this case is `theme/`.

```yaml
# mkdocs.yml
...

theme:
  name: material
  custom_dir: theme
...
```

## Configure the card

In this implementation, we are using some extra configuration to retrieve the card definitions. So in the `mkdocs.yml` file add the `card` attribute under `extra`:

```yaml
# mkdocs.yml
...

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/groosg
    - icon: fontawesome/brands/twitter
      link: https://twitter.com/gr00sg
...
  card:
    twitter: "@gr00sg"
    image: assets/images/twitter_card.png
    type: summary

...
```

- `twitter` - is the Twitter handler for the author.
- `image` - is the path for the image used in the card. It also goes under the directory defined in [custom_dir](https://www.mkdocs.org/user-guide/configuration/#custom_dir), `theme` in this case.
- `type` - is the type of the Twitter card. In this case it will be be `summary` or `summary_large_image` (see the [Twitter docs](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started) for a complete reference).

## Customize the card title and description

By default, the card will use the [page title](https://www.mkdocs.org/dev-guide/themes/#pagetitle) and the [site description](https://www.mkdocs.org/user-guide/configuration/#site_description) to fill the `og:title` and `og:description` tags. To override this behavior, we can add a `card` property to the page front matter with the properties `title` and `description`. Add the following lines at the top of a Markdown file:

```yaml
---
card:
  title: "A Serious GitHub Account Setup Guide"
  description: "In this guide, I'm going to show you how to configure your GitHub account to
improve security and privacy."
---

...
```

!!! note
    We are intentionally making these properties specific to the card because right now they are not used to customize the actual page title and description (I might do that later and refactor this).

The [built-in meta plugin](https://squidfunk.github.io/mkdocs-material/reference/#built-in-meta-plugin) is another feature that the Insiders version takes care of for us.
