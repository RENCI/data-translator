# Data Translator

This is the first draft of a site outline built with [Gatsby](https://www.gatsbyjs.org). Gatsby is a free and open source static site generator based on [React](https://reactjs.org). Sites built with Gatsby are modern and _fast_.

## Project Structure

The structure of the project is shown below.

```bash
$ tree -L 2
.
├── gatsby-browser.js
├── gatsby-config.js
├── gatsby-node.js
├── gatsby-ssr.js
├── LICENSE
├── node_modules
│   ...
├── package.json
├── package-lock.json
├── public
│   ├── icons
│   ├── index.html
│   ├── manifest.webmanifest
│   └── static
├── README.md
└── src
    ├── components
    ├── images
    ├── pages
    ├── styles
    └── templates
```

## Development

To develop locally, you can spin up a development server by executing `$ gatsby develop` in the project's root directory. The site is served on port 8000.

### Adding Content

#### Pages

Pages can be added by creating [Markdown](https://www.markdownguide.org/cheat-sheet/) files in the `src/pages` directory. Each markdown file must follow a predefined format, as illusrated below.

```bash
$ cat /src/pages/about-gamma.md
---
path: "/about/gamma"
title: "Gamma"
seo:
    title: "Gamma"
    keywords: "data translator renci"
    description: "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Veritatis, porro!"
---


Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quidem, reiciendis minus, perspiciatis aut eligendi blanditiis esse nobis magnam qui, quam expedita odit, natus temporibus aliquam consequatur porro reprehenderit odio asperiores.

...
```

The first chunk of each page file contains the "frontmatter" in YAML syntax, which lives between the two sets of triple dashes (`---`) and describes metadata unique to each page.

- `path` - This indicates the path the relative broswer will visit to access the page.
- `title` - This defines the page title.
- `seo` - This contains attributes to define properties for [search engine optimization](https://moz.com/beginners-guide-to-seo). The following SEO attributes can be provided on a per-page basis: `description`, `lang`, `meta`, `keywords` (as a comma-separated string), and `title`.

The SEO is required for each page, and so is its `title` attribute, being the only thing definitely unique to each page. The remaining attributes will default to the global SEO settings if empty strings are defined in a page's frontmatter. For example, if the frontmatter has

```yaml
seo:
    title: "Sample Page"
    description: ""
    keywords: ""
```

then the `description` and `keywords` attributes defined in `/gatsby-config.js` will be used for that page's SEO content.

\* Note the difference between the two `title` attributes: the topmost `title` attribuate is rendered as HTML as the title of the page, in an `<h1>` tag, while the SEO `title` is what is provided to search engines and is what populates `document.title`. 

The frontmatter can be extended to include other metadata that may be important and unique to specific pages. For example, for a blogging application, one way wish to include `date` and `author` entries in the frontmatter.

Below the frontmatter will be the actual [Markdown](https://www.markdownguide.org/cheat-sheet/) syntax that will render as HTML on the page. The `title` in the frontmatter will render as the inner text of an `<h1>` element.

#### Menu

Once a new page as been created, it can be added to the site's main navigation menu by a simple addition to the `gatsby-config.js` file. That file exports an object, and the menu items live in the array defined by `siteMetadata.menuLinks`.

Each element of the `menuLinks` array is an object consisting of `text (the text to be shown in the menu` and the `path` (the route the link will point to). To add a sublist of menu items, simply include a `submenu` property with an array of more menu item objects.

Below is a portion of this menu definition, which illustrates the expected format.

```javascript
menuLinks: [
    {
        text: "Home",
        path: "/",
    },
    {
        text: "About",
        path: "/about",
        submenu: [
            { text: "Program", path: "/about/program", },
            { text: "Gamma", path: "/about/gamma", },
            { text: "Green", path: "/about/green", },
        ]
    },
    // ...
],
```

## Deployment

This site is currently deployed to Github Pages with a `gh-pages` Node module. Details on building and deploying the application follow.

Suppose your GitHub username is `jane-doe`, and the name of the repository to which the build will be deployed is `my-cool-repo`. Then after deployment, the site will be acccessible at `jane-doe.github.io/my-cool-repo`.

### Specify the Repo

The repo needs to be added to the main export in `gatsby-config.js` as a property of the main export.

```js
pathPrefix: "/my-cool-repo",
```

This tells Gatsby to prefix the site's routes with your repo name during the build process. For example, users will be accessing the site via routes that look like `jane-doe.github.io/my-cool-repo/about` and `jane-doe.github.io/my-cool-repo/contact`.

### Define the Build Script

Now, we make our lives easier by defining a build script in `package.json`.

The fiollowing custom script

```js
"scripts": {
    "deploy": "gatsby build --prefix-paths && gh-pages -d public"
}
```

tells Gatsby to create the production build of the site, use prefix paths (detailed above), and tells `gh-pages` to push everything in the `public` directory to the `gh-pages` branch of the specified repo.

### Deploy

To deploy the bundled, static version of the site, we can simply execute the following command.

```bash
$ npm run deploy
```

## About Gatsby

Looking for more about Gatsby? Full documentation for Gatsby lives [on the website](https://www.gatsbyjs.org/). Here are some places to start:

- **For most developers, we recommend starting with our [in-depth tutorial for creating a site with Gatsby](https://www.gatsbyjs.org/tutorial/).** It starts with zero assumptions about your level of ability and walks through every step of the process.

- **To dive straight into code samples, head [to our documentation](https://www.gatsbyjs.org/docs/).** In particular, check out the _Guides_, _API Reference_, and _Advanced Tutorials_ sections in the sidebar.

## Resources

- Gatsby - [https://www.gatsbyjs.org/](https://www.gatsbyjs.org/)
- React - [https://reactjs.org](https://reactjs.org)
- gh-pages - [https://www.gatsbyjs.org/docs/how-gatsby-works-with-github-pages/](https://www.gatsbyjs.org/docs/how-gatsby-works-with-github-pages/)
- Markdown - [https://www.markdownguide.org/](https://www.markdownguide.org/)