This is a clean blogging theme adapted from https://github.com/StartBootstrap/startbootstrap-clean-blog. Currently under heavy development and not ready for use.

# Settings

- `SiteTitle`: The title of the site.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/*`).

## Calculated

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").

# Navigation

Override `/_navigation.cshtml` by creating a local copy to override the top-level navigation.