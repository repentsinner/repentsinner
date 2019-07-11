# [repentsinner.github.io](https://repentsinner.github.io)

### Github Pages & Jekyll quirks

* The Jekyll `_drafts` directory-based draft mechanism appears to be useless for Github Pages, since it builds remotely (on github) instead of locally on a development system. Every automated remote build is run w/o the required `--drafts` flag. It is recommended to `tag` as `draft` instead, and keep the `draft` tag out of the site listings. Instead, direct-link to a [drafts index](https://repentsinner.github.io/drafts).

* Good luck figuring out what is part of the template, and what is part of the site design. Highly recommend cloning chunks of [Jekyll Now](https://github.com/barryclark/jekyll-now) and rolling from there rather than trying to bootstrap by hand.
