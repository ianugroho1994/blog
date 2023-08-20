<div align="center">
  <img alt="Astro Theme Cactus logo" src="./gh-assets/astro-cactus-logo.png" width="70" />
</div>
<h1 align="center">
  🚀 My Personal Blog site 🌵
</h1>



## Key Features

- Use Astro Fast 🚀
- Use[Astro Theme Cactus](https://github.com/chrismwilliams/astro-theme-cactus)

## Demo 💻

Check out the [Demo](https://blog.ianugroho.com/), hosted on Netlify

## Commands

Replace pnpm with your choice of npm / yarn

| Command          | Action                                                         |
| :--------------- | :------------------------------------------------------------- |
| `pnpm install`   | Installs dependencies                                          |
| `pnpm dev`       | Starts local dev server at `localhost:3000`                    |
| `pnpm build`     | Build your production site to `./dist/`                        |
| `pnpm postbuild` | Pagefind script to build the static search of your blog posts  |
| `pnpm preview`   | Preview your build locally, before deploying                   |
| `pnpm sync`      | Generate types based on your config in `src/content/config.ts` |


## Adding posts

This theme utilises [Content Collections](https://docs.astro.build/en/guides/content-collections/) to organise Markdown and/or MDX files, as well as type-checking frontmatter with a schema -> `src/content/config.ts`.

Adding a post is a simple as adding your .md(x) file(s) to the `src/content/post` folder, the filename of which will be used as the slug/url. The two posts included with this template are there as an example of how to structure your frontmatter. Additionally, the [Astro docs](https://docs.astro.build/en/guides/markdown-content/) has a detailed section on markdown pages.

### Frontmatter

| Property (\* required) | Description                                                                                                                                                                                                                                                                                       |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| title \*               | Self explanatory. Used as the text link to the post, the h1 on the posts' page, and the pages title property. Has a max length of 60 chars, set in `src/content/config.ts`                                                                                                                        |
| description \*         | Similar to above, used as the seo description property. Has a min length of 50 and a max length of 160 chars, set in the post schema.                                                                                                                                                             |
| publishDate \*         | Again pretty simple. To change the date format/locale, currently **en-GB**, update/pass the **locale** arg to function **getFormattedDate**, found in `src/utils/date.ts`.                                                                                                                        |
| tags                   | Tags are optional with any created post. Any new tag(s) will be shown in `yourdomain.com/posts` + `yourdomain.com/tags`, and generate the page(s) `yourdomain.com/tags/[yourTag]`                                                                                                                 |
| coverImage             | This is an optional object that will add a cover image to the top of a post. Include both a `src`: "_path-to-image_" and `alt`: "_image alt_". You can view an example in `src/content/post/cover-image.md`                                                                                       |
| ogImage                | This is an optional property. An OG Image will be generated automatically for every post where this property **isn't** provided. If you would like to create your own for a specific post, include this property and a link to your image, the theme will then skip automatically generating one. |

## Acknowledgment

This theme is inspired by [Hexo Theme Cactus](https://github.com/probberechts/hexo-theme-cactus)

## License

MIT
