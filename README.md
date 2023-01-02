# Clean Blog

This is a clean blogging theme adapted from https://github.com/StartBootstrap/startbootstrap-clean-blog, with some additional goodies of its own.

## Minimum Statiq Web Version

This theme requires Statiq Web 1.0.0-beta.36 or later.

Using an earlier commit of the theme may allow the use of an earlier version of Statiq Web (look at the theme `settings.yml` file to determine the minimum Statiq Web version for a given version of the theme).

## Installation

Statiq themes go in a `theme` folder alongside your `input` folder. If your site is inside a git repository, you can add the theme as a git submodule:

```shell
git submodule add https://github.com/statiqdev/CleanBlog.git theme
```

Alternatively you can clone the theme directly:

```shell
git clone https://github.com/statiqdev/CleanBlog.git theme
```

Once inside the `theme` folder, Statiq will automatically recognize the theme.

If you're not going to modify theme files directly, you can leave the theme out of your repository and use the [Devlead.Statiq](https://www.nuget.org/packages/Devlead.Statiq) package to automatically download it as you build your site. See [this blog post](https://www.devlead.se/posts/2021/2021-04-10-devlead-statiq-part2-theme-from-uri) for details.

## Site Contents

You can add contents to your site using plain HTML and CSS, as well as any of the [template languages](https://www.statiq.dev/guide/content-and-data/template-languages/) supported by Statiq.Web. In the examples below, we will assume that you write your articles using Markdown, with [front matter](https://www.statiq.dev/guide/documents-and-metadata/front-matter) in YAML.

### Home Page

By default, your blog's home page will be an [archive](https://statiq.dev/web/content-and-data/archives), showing the latest blog posts in inverse chronological order.

Note that Statiq.Web does not generate archive pages if it finds no contents for them to index. In other words, **the theme will have no home page until you add a blog post**.

You can provide your own home page by adding an `index.cshtml` (or `index.md`) file in your `input` folder. Unless your custom home page is an archive too, it will be displayed regardless of the presence of any post.

#### Sidebar

On the right side of the list of posts, the default home page sports a vertical bar listing the ten most used tags. Clicking on any tag brings the user to the list of posts that have that tag, similarly to what happens of the "Tags" page.

All remaining space in the sidebar is yours! Just add a `_sidebar.cshtml` file in your `input` folder; it will be trated as a [Razor partial](https://www.statiq.dev/guide/content-and-data/template-languages/razor) and inserted just below the list of most used tags.

### Blog Posts

Add your blog posts to a `posts` folder under your own `input` folder. An example post might be named `input/posts/example.md` and contain the following content:

```text
Title: This Is An Example Post
Lead: Yay for examples!
Published: 12/13/2014
Tags:
  - Examples
  - Tag with spaces
---
This is my example blog post content.
```

The `---` line separates the _front matter_, i.e. YAML code containing page-specific settings, from the actual body of the post. YAML syntax is widely known and is usually intuitive; however, if you need a quick tutorial or a refresher you may find [this article](https://learnxinyminutes.com/docs/yaml/) useful.

As for the `.md` extension, it means that the article body is written with Markdown syntax. And yes, there's [a quick guide](https://learnxinyminutes.com/docs/markdown/) for it, too. See also [here](https://www.statiq.dev/guide/content-and-data/template-languages/markdown) for Markdown features specific to Statiq.Web.

#### Published Date

If you don't specify a `Published` date, Statiq will use the last modification date of the file. Since Git does not save and restore file times, you should always specify a `Published` date if you store your blog in a Git repository; otherwise, all your posts will appear to have been published at the date of checkout.

#### Tags

You may specify as many tags as you like, or even none. Of course, posts with no tags will not appear in the Tags archive.

### Static pages

Static pages are different from blog posts, in that they have no taxonomy (published date, tags) and thus do not appear in archives. Their usual purpose is to provide information on a site-wide basis, for example an author's bio, portfolio, and/or resume; a privacy policy; a contact form; etc.

Add your static pages to a `pages` folder under your own `input` folder. An example page might be named `input/pages/about.md` and contain the following content:

```text
NavbarTitle: About
Order: 1
Title: About me
Description: The life and times of James D. Author
---
Hi there, I'm James and this is my blog.

Short bio; why I opened a blog; miscellaneous yada yada.
```

## Customization

This theme offers lots of customization options. Methods through which you can personalize your blog include:

- **Settings** - Quite a lot can be accomplished just by tweaking [settings](#settings), either in a global `settings.yml` file, in the front matter of documents, or both.
- **File addition** - Certain files in the theme are just empty placeholders. Just add a corresponding file in your `input` folder and it will be used instead of the empty file. This way you can add your own HTML, scripts, and styles, without having to modify the theme.
- **File replacement** - You can replace every theme file with your own. Just copy any file from the theme's `input` folder to your own, modify the copy at your leisure, and enjoy your fully personalized blog.

> **NOTE:** This theme uses [Razor](https://www.statiq.dev/guide/content-and-data/template-languages/razor) to build HTML pages and [SASS](https://sass-lang.com) to build CSS styles. You may want to familiarize with their syntax (besides, of course, HTML and CSS) before making additions and/or modifications.

The remainder of this section illustrates how to customize some specific areas of the theme. More exhaustive references for settings and placeholder files are in subsequent sections.

### Basic Configuration

Here's an example of a `settings.yml` file:

```yaml
# Hosting settings
Host: jamesdauthor.github.io # Host name - This example is for GitHub pages
LinksUseHttps: true # Whether to generate HTTPS links in archives etc. - usually true, of course

# Branding
SiteTitle: My awesome blog
SiteDescription: The blog no one but me was missing
Copyright: => @$"All content © {DateTime.Now.Year} James D. Author" # You can use C# expressions prefixed by =>

# Blog settings
DateTimeInputCulture: "" # This determines the expected format for Published dates in posts - Empty string means invariant culture (yyyy-MM-dd format)
DateTimeDisplayCulture: en-US # This determines the format for displayed dates, e.g. in post headers and archive pages
GenerateSearchIndex: true # Setting this to false disables both the search box in navigation bar and the search page
```

### The Navigation Bar

The navigation bar is a series of links that appears at the top of every page of your blog on desktop browsers, or can be displayed by touching a "Menu" button in the top right corner on mobile browsers.

The navigation bar includes: links to all static pages (by default, see below), a "Posts" link to a historical archive of posts, and a "Tags" link to an index of all tags.

A search box is also included at the end of the navigation bar if search index generation is enabled.

#### Links To Static Pages

If you don't want a page to be linked in the navigation bar, add a `ShowInNavbar: false` setting in its front matter.

If you don't specify a value for `NavbarTitle` in a page, `Title` will be used as the link text.

Links are sorted by the value of their `Order` setting. Pages with `Order` greater than or equal to 100 will appear after "Posts" and "Tags".

### Destination Paths

By default, posts and static pages are output using the same folder structure they were found in your `posts` and `pages` folders, respectively. If you want to modify that, you can use [directory metadata](https://www.statiq.dev/guide/web/directory-metadata) to specify a different destination. If you combine that with [computed values](https://www.statiq.dev/guide/documents-and-metadata/metadata-values#computed-values), you can programatically set the destination of your posts.

For example, the following in a `_directory.yml` file in the `posts` folder will result in outputting posts with a path of "yyyy/mm/post-title.html":

```yaml
DestinationPath: => $"{Document.GetDateTime("Published").ToString("yyyy/MM")}/{Document.Destination.FileName}"
```

Keep in mind that what determines if a document is a post or a static page is its _source_ location (unless, of course, you manually set `IsPost` or `IsPage` in front matter). You can go wild with computed destinations, even to the point where your resulting site has no `posts` or `pages` directory at all: the correct styles will still be applied to each post or static page, and of course all links (e.g. in archives and navigation bar) will point to your computed destinations.

## Settings

### Global

These can be set in a settings file (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `SiteTitle`: The title of the site.
- `SiteDescription`: The subtitle shown on the home (index) page.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/**/*`).
- `PageSources`: A globbing pattern where to find static pages (defaults to `pages/**/*`).

### Page

These can be set in front matter, a sidecar file, etc. (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `Title`: The title of the page (or post).
- `NavbarTitle`: The title of the page to use in the top-level navbar (optional, `Title` will be used if not specified).
- `Description`: A description of the page; not displayed as a subtitle for posts, but used nonetheless to build a `<meta>` HTML tag.
- `Lead`: Descriptive text that expands on the title of a post.
- `Tags`: Tags for a blog post.
- `Published`: The date a page or post was published.
- `Image`: Path to an image for the page or post.
- `ImageAttribution`: Markdown of copyright attribution for the image.
- `ShowInNavbar`: Set to `false` to hide a static page in the top navigation bar.
- `IsPost`: Set to `false` to exclude the file from the set of posts. This will also disable post styling like displaying tags in the header.
- `IsPage`: Set to `false` to exclude the file from the set of static pages. This will also disable navigation bar linking.

### Post comments

These can be set in a settings file (in addition to any of Statiq's [settings](https://statiq.dev/guide/configuration/settings) and [web settings](https://statiq.dev/guide/configuration/web-settings)).

- `CommentEngine`: The comment engine to use for posts (empty string or "none" to _not_ display comments).

Currently, the only available comment engine is [`giscus`](https://giscus/app).

You can interface to a comment engine of your choice without modifying this theme:

- create a Razor partial named `_post-comments-name_of_your_engine.cshtml` file in your `input` folder, containing the necessary HTML code;
- set `CommentEngine` to `name_of_your_engine` to have your partial automatically included.

#### Giscus

[`giscus`](https://giscus/app) uses GitHub Discussions as comment storage.

The following settings must be set in your `settings.yml` file when `CommentEngine` is set to `giscus`:

- `GiscusRepoName`: Name of the repository whose discussions act as storage for post comments.
- `GiscusRepoId`: ID of the repository whose discussions act as storage for post comments.
- `GiscusCategoryId`: ID of the discussion category where new discussions will be created. It is recommended to use a category with the Announcements type so that new discussions can only be created by maintainers and giscus.

To configure Giscus correctly in your blog, go to https://giscus.app and follow the configuration instructions, then copy and paste configuration values from the preconfigured `<script>` tag to your settings file.

Alternatively, or if you need to tweak some advanced Giscus setting, create a file named `_post-comments-giscus.cshtml` in your `input` folder and just copy the preconfigured script into it.

### Calculated Settings

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter or used from your own pages.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").

## Partials

Replace or copy any of these Razor partials in your `input` folder to override sections of the site:

- `/_head.cshtml`: Additional content for the `<head>` tag.
- `/_navigation.cshtml`: The navigation bar at the top every page.
- `/_navbar.cshtml`: The items of the navigation bar at the top of the page.
- `/_header.cshtml`: The header section of the page.
- `/_posts.cshtml`: Displays a set of posts stored in the children of a document passed as the partial model data.
- `/_post.cshtml`: Displays an individual post inside a list of posts.
- `/_post-after-content.cshtml`: Displays content at the bottom of a post, before comments.
- `/_page-after-content.cshtml`: Displays content at the bottom of a static page.
- `/_common-after-content.cshtml`: Displays content at the bottom of _any_ page. Included just after `/_post-after-content.cshtml` / `/_page-after-content.cshtml`.
- `/_copyright.cshtml`: Displays a copyright attribution for the blog. By default, contains a paragraph with the `Copyright` setting.
- `/_post-comments.cshtml`: Displays comments for a post.
- `/_post-comments-{engine}.cshtml`: Displays the contents of the "comments" section of a post when the `CommentEngine` setting is equal, case-insensitively, to `{engine}`.
- `/_sidebar.cshtml`: Additional content for the sidebar on the main index page (ignored if you provide your own index page).
- `/_footer.cshtml`: The footer at the bottom of all pages.
- `/_scripts.cshtml`: Additional scripts or other content at the bottom of the page.

## Sections

In addition to globally changing sections of the site using the partials above you can also add the following Razor sections to any given page to override them for that page (which will typically disable the use of the corresponding partial):

- `Head`: Additional content for the `<head>` tag (this section is additive with the content in the `_head.cshtml` partial).
- `Navigation`: The navigation at the top of the layout.
- `Header`: The header section of the page.
- `Footer`: The footer section of the page.
- `Scripts`: Additional scripts or other content at the bottom of the page (this section is additive with the content in the `_scripts.cshtml` partial).

## Styles

There are three distinct extensibility points for styles:

- to override Bootstrap variables, for instance [customize Bootstrap options](https://getbootstrap.com/docs/5.2/customize/options/), create an input file at `scss/_bootstrap-variable-overrides.scss` and add your variables there;
- to override theme variables, for example to change the main color for the masthead gradient, create an input file at `scss/_variable-overrides.scss` and add your variables there;
- to override any styles, create an input file at `scss/_overrides.scss` and add your styles there.

You will find the definitions of overridable Sass variables in the following files:

- `input/vendor/bootstrap/scss/_variables.scss` (Bootstrap variables and options);
- the whole `input/vendor/startbootstrap-clean-blog/scss/variables` directory (to customize the Clean Blog theme);
- `input/scss/_variables.scss` (to customize this variation of CleanBlog - masthead gradient color is here).

## Porting From Wyam

This blogging theme is roughly compatible with the Wyam CleanBlog theme. Some notes if you're porting:

- You will need to [create a Statiq Web app](https://statiq.dev/web/) at the root of your site (you can keep the `input` directory).
  - Run `dotnet new console` at the root of your site.
  - Run `dotnet add package Statiq.Web --version x.y.z` (using the [latest Statiq Web version](https://www.nuget.org/packages/Statiq.Web)).
  - Change the generated `Program` class in `Program.cs` to:

```
using System;
using System.Threading.Tasks;
using Statiq.App;
using Statiq.Web;

namespace ...
{
  public class Program
  {
    public static async Task<int> Main(string[] args) =>
      await Bootstrapper
        .Factory
        .CreateWeb(args)
        .RunAsync();
  }
}
```

- Follow the [installation instructions above](#installation) to install the theme into your site.

- Create a `settings.yml` file at the root of your site and copy over settings from your `config.wyam` file
  - Since the new settings file is YAML you don't need to prefix strings or anything, for example `Settings[Keys.Host] = "daveaglick.com";` becomes `Host: daveaglick.com`.
  - If you defined a global "Title" setting in `config.wyam` the new theme should set "SiteTitle" instead (and if not, a "SiteTitle" should be defined).
  - If you defined an "Intro" setting, that should be placed in a new `_index.yml` file in your `input` directory with a key of "Description".

- If you created an `input/assets/css/override.css` file, move it to `input/scss/_overrides.scss` (and you can now use Sass inside the CSS overrides file).

- Replace any uses of `img-response` CSS class with `img-fluid` since this theme uses a newer version of Bootstrap and that CSS class changed.

- Rename and fix up any override theme files or partials according to the supported ones documented above.
  - For example, the old Wyam CleanBlog supported a `_PostFooter.cshtml` which should be renamed to `_post-footer.cshtml`.
  - The CSS may not match exactly since the new CleanBlog theme is based on the most recent CleanBlog project so you may need to take a look at the default partial implementations in this theme and adjust your override files accordingly.

- You can likely remove any build scripting and bootstrapping code since you can now run `dotnet run -- preview` to preview the site.
  - You can also now setup [built-in deployment](https://statiq.dev/web/deployment/).
