## Migration Checklist: MkDocs (Material) to Astro (Citrus)

*   [ ] **Phase 1: Setup & Initial Configuration**
    *   [ ] Install Node.js, npm/yarn.
    *   [ ] Create new Astro project.
    *   [ ] Install/Integrate Astro Citrus theme.
    *   [ ] Configure `astro.config.mjs` (Tailwind, integrations).
    *   [ ] Install theme dependencies.
*   [ ] **Phase 2: Content Planning & Structure**
    *   [ ] Identify all content types (pages, blog posts).
    *   [ ] Plan Astro content collections for blog.
    *   [ ] Define content schemas (`src/content/config.ts`).
*   [ ] **Phase 3: Content Migration - Static Pages**
    *   [ ] Copy `index.md` to `src/pages/index.md` (or `.astro`).
    *   [ ] Copy `services.md` to `src/pages/services.md`.
    *   [ ] Copy `rag_resources.md` to `src/pages/rag_resources.md`.
    *   [ ] Copy `ld_services.md` to `src/pages/ld_services.md`.
    *   [ ] Update frontmatter for all static pages.
    *   [ ] Adjust internal links in static pages.
*   [ ] **Phase 4: Content Migration - Blog**
    *   [ ] Move blog posts from `docs/writing/posts/` to `src/content/blog/`.
    *   [ ] Update frontmatter for all blog posts (title, pubDate, description, author, image, tags, categories).
    *   [ ] Handle `docs/writing/.authors.yml` data (e.g., integrate into frontmatter or a new data file).
    *   [ ] Create `src/pages/writing/index.astro` (blog listing).
    *   [ ] Create `src/pages/writing/[slug].astro` (individual blog post display).
*   [ ] **Phase 5: Navigation & Layout**
    *   [ ] Replicate main navigation (Home, Services, Writing) in an Astro layout/component.
    *   [ ] Ensure active page highlighting in navigation.
    *   [ ] Choose a primary layout from Citrus or create one (e.g., `src/layouts/Layout.astro`).
*   [ ] **Phase 6: Styling & Customization**
    *   [ ] Translate `docs/stylesheets/extra.css` custom colors to `tailwind.config.mjs`.
    *   [ ] Migrate other custom styles from `extra.css` (using Tailwind or global CSS).
    *   [ ] Review and update MkDocs Material specific syntax (admonitions, icons).
    *   [ ] Customize Citrus theme components as needed.
*   [ ] **Phase 7: SEO & Metadata**
    *   [ ] Configure Satori for OG image generation (if using Citrus feature).
    *   [ ] Ensure correct `<title>` and `<meta name="description">` on all pages.
    *   [ ] Place `c8024f31b348420dab5b1b5dda317cfb.txt` in `public/`.
    *   [ ] Place `indexnow.json` in `public/` and update `urlList`.
*   [ ] **Phase 8: Analytics**
    *   [ ] Migrate Google Analytics script to main Astro layout.
*   [ ] **Phase 9: Build, Test, & Review**
    *   [ ] Run `npm run dev` for local development.
    *   [ ] Perform production builds with `npm run build`.
    *   [ ] Thoroughly test links, styles, images, functionality.
    *   [ ] Test responsiveness across devices/browsers.
    *   [ ] Validate SEO metadata and OG images.
    *   [ ] Check console for errors.
*   [ ] **Phase 10: Deployment & CI/CD**
    *   [ ] **GitHub Actions Workflow (`.github/workflows/ci.yml`):**
        *   [ ] Add Node.js setup step.
        *   [ ] Change dependency installation to npm/yarn.
        *   [ ] Update build command to Astro's build command.
        *   [ ] Change `publish_dir` to `dist/` (or Astro's output dir).
        *   [ ] (Optional) Adjust caching for Node.js modules.
    *   [ ] (If not GitHub Pages) Choose and configure hosting platform for Astro.
    *   [ ] Deploy the built site (via updated workflow or manually to new platform).
    *   [ ] Update DNS records (if applicable, likely no change if still GitHub Pages).
    *   [ ] Final live site testing. 