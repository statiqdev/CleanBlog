This is a clean blogging theme adapted from https://github.com/StartBootstrap/startbootstrap-clean-blog. Currently under heavy development and not ready for use.

# Settings

## Global

These can be set in a settings file.

- `SiteTitle`: The title of the site.
- `Description`: A description of the site.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/*`).

## Page

These can be set in front matter, a sidecar file, etc.

- `Title`: The title of the page (or post).
- `Description`: A description of the page.
- `Lead`: Descriptive text that expands on the title, usually used for posts.
- `Tags`: Tags for a blog post.
- `Published`: The date a page or post was published.
- `Image`: Path to an image for the page or post.

## Calculated

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter or used from your own pages.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").
- `IsPost`: Set to `true` if the current document is a blog post, which can be helpful to control conditional styling or layout content.

# Partials

Replace or copy any of these Razor partials in your `input` folder to override sections of the site:

- `/_head.cshtml`: Additional content for the `<head>` tag.
- `/_navigation.cshtml`: The navigation at the top of the layout.
- `/_navbar.cshtml`: The navigation bar at the top of the page.
- `/_header.cshtml`: The header section of the page.
- `/_posts.cshtml`: Displays a set of posts stored in the children of a document passed as the partial model data.
- `/_post.cshtml`: Displays an individual post inside a list of posts.
- `/_post-footer.cshtml`: Displays content at the bottom of a post (for example, a comments section).
- `/_sidebar.cshtml`: Additional content for the sidebar on the main index page (ignored if you provide your own index page).
- `/_footer.cshtml`: The footer at the bottom of all pages.

# Sections

In addition to globally changing sections of the site using the partials above you can also add the following Razor sections to any given page to override them for that page (which will typically disable the use of the corresponding partial):

- `Head`: Additional content for the `<head>` tag (this section is additive with the content in the `_head.cshtml` partial).
- `Navigation`: The navigation at the top of the layout.
- `Header`: The header section of the page.
- `Footer`: The footer section of the page.

# Index Page

You can provide your own `index.cshtml` (or `index.md`) if you like and that will override the default index page. You'll _have_ to provide your own index page if you don't
include any blog posts since the default index page is an archive of posts.

# Styles

To add new styles or override existing ones, create an input file at `scss/_overrides.scss` and add Sass styles there.