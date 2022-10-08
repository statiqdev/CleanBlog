This is a clean blogging theme adapted from https://github.com/StartBootstrap/startbootstrap-clean-blog.

# Minimum Statiq Web Version

This theme requires Statiq Web 1.0.0-beta.36 or later.

Using an earlier commit of the theme may allow the use of an earlier version of Statiq Web (look at the theme `settings.yml` file to determine the minimum Statiq Web version for a given version of the theme).

# Installation

Statiq themes go in a `theme` folder alongside your `input` folder. If your site is inside a git repository, you can add the theme as a git submodule:

```
git submodule add https://github.com/statiqdev/CleanBlog.git theme
```

Alternatively you can clone the theme directly:

```
git clone https://github.com/statiqdev/CleanBlog.git theme
```

Once inside the `theme` folder, Statiq will automatically recognize the theme. If you want to tweak the theme you can edit files directly in the `theme` folder or copy them to your `input` folder and edit them there.

Note that the `index.cshtml` page in the theme is an [archive](https://statiq.dev/web/content-and-data/archives) which means that it uses blog posts from the `posts` folder of your own `input` folder to populate generate the output page. If you don't have any blog posts or aren't planning on adding files to the `posts` folder, no output file will be generated. **This means that right out of the box the theme will out produce any output pages until you add a blog post**. You'll need to replace the `index.cshtml` file with your own if you want an index file that doesn't use blog posts.

# Blog Posts

Add your blog posts to a `posts` folder under your own `input` folder. An example post might be named `input/posts/example.md` and contain the following content:

```
IsPost: true
Title: This Is An Example Post
Lead: Yay for examples!
Published: 12/13/2014
Tags:
  - Examples
  - Code
---
This is my example blog post content.
```

## Post Destination Path

By default, posts are output using the same folder structure they were found in your `posts` folder. If you want to modify that, you can use [directory metadata](https://www.statiq.dev/guide/web/directory-metadata) to specify a different destination. If you combine that with [computed values](https://www.statiq.dev/guide/documents-and-metadata/metadata-values#computed-values), you can programatically set the destination of your posts.

For example, the following in a `_directory.yml` file in the `posts` folder will result in outputting posts with a path of "yyyy/mm/post-title.html":

```
DestinationPath: => $"{Document.GetDateTime("Published").ToString("yyyy/MM")}/{Document.Destination.FileName}"
```

# Settings

## Global

These can be set in a settings file (in addition to any [Statiq Web settings](https://statiq.dev/web/configuration/settings)).

- `SiteTitle`: The title of the site.
- `Description`: A description of the site.
- `PostSources`: A globbing pattern where to find blog posts (defaults to `posts/*`).

## Page

These can be set in front matter, a sidecar file, etc. (in addition to any [Statiq Web settings](https://statiq.dev/web/configuration/settings)).

- `Title`: The title of the page (or post).
- `NavbarTitle`: The title of the page to use in the top-level navbar (optional, `Title` will be used if not specified).
- `Description`: A description of the page.
- `Lead`: Descriptive text that expands on the title, usually used for posts.
- `Tags`: Tags for a blog post.
- `Published`: The date a page or post was published.
- `Image`: Path to an image for the page or post.
- `ShowInNavbar`: Set to `false` to hide the page in the top navigation.
- `IsPost`: Set to `false` to exclude the file from the set of posts. This will also disable post styling like displaying tags in the header.

## Calculated

The following keys are calculated in `settings.yml` and can be overridden by providing new values in your settings or front matter or used from your own pages.

- `PageTitle`: The title that's set for the current page and shown in the browser (by default this is "[Site Title] - [Document Title]").

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
- `/_scripts.cshtml`: Additional scripts or other content at the bottom of the page.

# Sections

In addition to globally changing sections of the site using the partials above you can also add the following Razor sections to any given page to override them for that page (which will typically disable the use of the corresponding partial):

- `Head`: Additional content for the `<head>` tag (this section is additive with the content in the `_head.cshtml` partial).
- `Navigation`: The navigation at the top of the layout.
- `Header`: The header section of the page.
- `Footer`: The footer section of the page.
- `Scripts`: Additional scripts or other content at the bottom of the page.

# Index Page

You can provide your own `index.cshtml` (or `index.md`) if you like and that will override the default index page. You'll _have_ to provide your own index page if you don't
include any blog posts since the default index page is an archive of posts.

# Styles

To add new styles or override existing ones, create an input file at `scss/_overrides.scss` and add Sass styles there.

# Searching

If you set `GenerateSearchIndex` to `true`, Statiq Web will automatically generate a search index for your site and the theme will include a search box in the navigation bar and generate a dedicated search page for your site. You can add your own search index page by creating a `search.cshtml` file in your `input` folder.

# IIS/Azure

You'll probably need to use a `web.config` file with redirect rules when deploying a static site on IIS or Azure App Service in order to support extensionless URLs and other static site conventions. [The Statiq Web documentation suggests one such set of rewrite rules](), but since CleanBlog currently outputs both a `/posts.html` and a `/posts` folder (likewise for `/tags.html` and `/tags`), it confuses the rewrite rules presented there. Here's an alternative set of rewrite rules that supports extensionless files and folders with the same name:

```
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="html">
          <match url="(.+)/?" />
          <conditions logicalGrouping="MatchAny">
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" />
          </conditions> 
          <action type="Rewrite" url="{R:1}.html" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

You can read more about this issue at https://github.com/statiqdev/statiqdev.github.io/issues/32.

# Porting From Wyam

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
