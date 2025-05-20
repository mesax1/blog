## Migration Plan: MkDocs (Material) to Astro (Citrus)

### I. Plan of Actions (Step-by-Step)

1.  **Set up Astro Environment & Citrus Theme:**
    *   Install Node.js and npm/yarn if not already present.
    *   Create a new Astro project: `npm create astro@latest` or `yarn create astro`.
    *   Follow the Astro CLI prompts to set up your project (e.g., select "Empty" or a basic starter).
    *   Integrate the Astro Citrus theme. You can usually do this by:
        *   Cloning or downloading the theme's repository from [its GitHub page (linked from the theme page)](https://astro.build/themes/details/astro-citrus/).
        *   Copying the relevant theme files (layouts, components, styles) into your Astro project's `src` directory.
        *   Installing any theme-specific dependencies listed in its `package.json`.
    *   Configure `astro.config.mjs` to correctly set up Tailwind CSS (as Citrus uses it) and any other integrations the theme requires (e.g., Satori for OG images, Shiki for code highlighting).

2.  **Content Migration Strategy:**
    *   **Identify Content Types:**
        *   Static Pages (e.g., `index.md`, `services.md`, `rag_resources.md`, `ld_services.md`)
        *   Blog Posts (currently under `docs/writing/posts/`)
        *   Navigation Structure (defined in MkDocs, will need to be replicated in Astro)
    *   **Astro Content Collections:**
        *   Plan how to use Astro's content collections for your blog posts and potentially other structured content. This will likely involve creating a `src/content/blog/` directory.
        *   Define schemas for your content collections (e.g., `src/content/config.ts`) to manage frontmatter properties like title, description, date, tags, categories.

3.  **Migrate Static Pages:**
    *   Copy the Markdown files from your `docs` directory (e.g., `index.md`, `services.md`, `rag_resources.md`, `ld_services.md`) into Astro's `src/pages/` directory or a relevant subdirectory.
    *   Astro uses file-based routing. For example, `docs/index.md` might become `src/pages/index.md` (or `.astro` if you want to use Astro components directly). `docs/services.md` would become `src/pages/services.md`.
    *   Update frontmatter in these Markdown files to be compatible with Astro and the Citrus theme. This might involve changing syntax or adding new fields (e.g., `layout`, `title`, `description`). The Citrus theme documentation should specify required/recommended frontmatter.
    *   Review and adjust internal links to work with Astro's routing. MkDocs' `[Link](./relative/path.md)` might need to change to `/relative/path/` in Astro.

4.  **Migrate Blog Posts:**
    *   Move the blog posts from `docs/writing/posts/` to the Astro content collection directory you defined (e.g., `src/content/blog/`).
    *   Update the frontmatter of each blog post according to the schema defined in `src/content/config.ts`. This will likely include fields like `title`, `pubDate`, `description`, `author`, `image` (for OG images), `tags`, `categories`.
    *   The `docs/writing/.authors.yml` file will need to be handled differently. Astro doesn't have a direct equivalent. You might:
        *   Include author information directly in the frontmatter of each post.
        *   Create a separate data file (e.g., `src/data/authors.json`) and reference it in your Astro components/layouts.
    *   Create Astro pages to list blog posts (e.g., `src/pages/writing/index.astro`) and display individual posts (e.g., `src/pages/writing/[slug].astro`). These pages will fetch data from your blog content collection.

5.  **Replicate Navigation:**
    *   Your current navigation is defined by the file structure and potentially `mkdocs.yml` (which is not provided, but typical for MkDocs).
    *   In Astro, navigation is typically built using Astro components. You'll need to create or modify a layout component (likely provided by the Citrus theme, e.g., `src/layouts/Layout.astro` or a specific header component) to include links to your main pages: Home, Services, Writing.
    *   Ensure the navigation highlights the active page.

6.  **Styling and Customization:**
    *   **Tailwind CSS:** The Citrus theme uses Tailwind CSS.
        *   Your existing custom CSS in `docs/stylesheets/extra.css` defines custom color properties for `phthalo-green`. You'll need to translate these into Tailwind's configuration (`tailwind.config.mjs`). You can extend Tailwind's color palette with your custom green shades.
        *   Any other custom styles from `extra.css` will need to be either:
            *   Re-implemented using Tailwind utility classes.
            *   Added as global styles in an Astro CSS file (e.g., `src/styles/global.css`) imported into your main layout.
    *   **Theme Customization:** The Citrus theme page mentions it's "easy to customise." Explore its components and configuration options to match your desired look and feel. This might involve modifying Astro components in `src/components/` or layout files in `src/layouts/`.
    *   **MkDocs Material Specific Syntax:** Review your Markdown content for any Material for MkDocs specific syntax (e.g., admonitions, icons like `:material-briefcase:`). These will likely need to be replaced with:
        *   HTML/JSX within your Markdown files (if Astro's Markdown processor allows it).
        *   Custom Astro components that replicate the functionality. For icons, you might use an SVG icon library compatible with Astro/Tailwind.

7.  **SEO and Metadata:**
    *   **OG Images:** The Citrus theme mentions "Satori for auto generating OG images for blog posts." Configure this feature.
    *   **Metadata:** Ensure each page has appropriate `<title>`, `<meta name="description">`, and other relevant meta tags. Astro layouts are a good place to manage default and page-specific metadata.
    *   **`indexnow.json`:** The file `site/indexnow.json` (and the corresponding `c8024f31b348420dab5b1b5dda317cfb.txt` which is currently in `docs/` but should be in the public/static output directory) is for search engine indexing.
        *   Ensure `c8024f31b348420dab5b1b5dda317cfb.txt` is placed in Astro's `public/` directory so it's copied to the root of your built site.
        *   The `urlList` in `indexnow.json` will need to be updated to reflect the new Astro site structure. This file should also be in the `public/` directory.
        *   Consider automating the update of `urlList` if your site structure changes frequently.

8.  **Analytics:**
    *   Your current `site/index.html` includes Google Analytics setup.
    *   Migrate this setup to your main Astro layout file (e.g., `src/layouts/Layout.astro`) to ensure analytics are tracked across all pages. The Astro documentation has guides on integrating analytics scripts.

9.  **Build and Test:**
    *   Regularly run `npm run dev` (or `yarn dev`) to preview your site locally.
    *   Run `npm run build` (or `yarn build`) to create a production build and test it thoroughly.
    *   Check all links, images, styles, and functionality.
    *   Test on different devices and browsers.

10. **Deployment & CI/CD:**
    *   **GitHub Actions Workflow Update:**
        *   Modify the existing `.github/workflows/ci.yml` for Astro.
        *   **Setup Node.js:** Add a step to set up Node.js (e.g., `actions/setup-node@v3` with a specific Node.js version compatible with Astro and your theme).
        *   **Install Dependencies:** Change the Python dependency installation to npm/yarn dependency installation (e.g., `npm ci` or `yarn install`).
        *   **Build Command:** Replace `mkdocs build` with the Astro build command (e.g., `npm run build` or `yarn build`).
        *   **Publish Directory:** Update the `publish_dir` in the `peaceiris/actions-gh-pages` action to point to Astro's default output directory, which is usually `dist/` (instead of `./site`).
        *   **Caching (Optional but Recommended):** Adjust caching to cache Node.js modules (e.g., `~/.npm` or the `node_modules` directory).
    *   **Hosting Platform Configuration:** Configure your deployment environment (e.g., Netlify, Vercel, or continue with GitHub Pages) for an Astro project. Astro's documentation provides deployment guides for various platforms. If sticking with GitHub Pages, the updated workflow will handle the deployment.
    *   **DNS Records:** Update DNS records if necessary (this should remain the same if you are still using `juanpml.com` with GitHub Pages). 