This is a clean blogging theme adapted from https://github.com/StartBootstrap/startbootstrap-clean-blog. Currently under heavy development and not ready for use.

# Settings

- `SiteTitle`: The title of the site.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/*`).
- `HeaderTitle`: Can be set on a page to change the title displayed in the header (defaults to the same value as `Title`).

## Calculated

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").

# Partials

Replace or copy any of these Razor partials in your `input` folder to override sections of the site:

- `/_head.cshtml`: Additional content for the `<head>` tag.
- `/_navigation.cshtml`: The navigation at the top of the layout.
- `/_navbar.cshtml`: The navigation bar at the top of the page.
- `/_header.cshtml`: The header section of the page.
- `/_posts.cshtml`: Displays a set of posts stored in the children of a document passed as the partial model data.
- `/_post.cshtml`: Displays an individual post.

# Sections

In addition to globally changing sections of the site using the partials above you can also add the following Razor sections to any given page to override them for that page:

- `Head`: Additional content for the `<head>` tag (this section is additive with the content in the `_head.cshtml` partial).
- `Navigation`: The navigation at the top of the layout.
- `Header`: The header section of the page.
