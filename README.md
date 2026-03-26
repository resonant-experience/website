## Hi there 👋

This repository contains:
- Root website (main site): `index.html`, assets in `assets/`
- Jekyll blog source: `blog-src/`
- Jekyll blog generated output: `blog/`

## Local development (blog)

1. Install Ruby + Bundler + Jekyll:
   - `sudo apt update && sudo apt install -y ruby-full build-essential`
   - `gem install bundler`
   - `cd blog-src && bundle install`

2. Run development server:
   - `cd blog-src`
   - `bundle exec jekyll serve --baseurl "/blog"`
   - Open `http://127.0.0.1:4000/blog/`

3. Build static blog output (for deploy):
   - `cd blog-src`
   - `bundle exec jekyll build --baseurl "/blog" --destination ../blog-static`

4. Publish generated output to final blog folder:
   - `cd blog-src`
   - `rsync -a --delete ../blog-static/ ../blog/`

## Link behavior

- Main site is served from `/` (root), existing `index.html` remains in place.
- Blog is served from `/blog/` and includes the site-style top nav.

## Testing before push

1. Validate root site:
   - `cd /home/gst/github.com/resontant-experience/website`
   - `python3 -m http.server 8000`
   - Open `http://127.0.0.1:8000/`

2. Validate blog site production output:
   - `cd /home/gst/github.com/resontant-experience/website/blog-src`
   - `bundle exec jekyll build --baseurl "/blog" --destination ../blog-static`
   - `rsync -a --delete ../blog-static/ ../blog/`
   - In root server after step 1, refresh `http://127.0.0.1:8000/blog/`

3. Validate blog dev mode (live reload):
   - `cd /home/gst/github.com/resontant-experience/website/blog-src`
   - `bundle exec jekyll serve --baseurl "/blog"`
   - Open `http://127.0.0.1:4000/blog/`

4. Optional publish sanity checks:
   - Click top logo from `/blog/` and confirm it returns to `/`
   - Click `Blog` nav from `/` and confirm it goes to `/blog/`

## Notes

- Keep `blog-src` for editing posts and templates. Do not commit `blog-static` if you prefer to keep only source and generated folder (`blog/`) in Git.
- `blog/` should contain generated assets for GitHub Pages when using site configured with `/blog` base URL.

### Insert date for a single new post

To get local timezone current datetime in Jekyll format and paste it into `date:` front matter:

```bash
date +"%Y-%m-%d %H:%M:%S %z"
```

Example output:

`2026-03-24 21:29:12 +1100`

## Adding downloadable assets to blog posts

To include static files like PDFs, images, or documents that readers can download:

1. Place the file in `blog-src/assets/` (or a subfolder, e.g., `blog-src/assets/downloads/`).

2. In your post Markdown, link to it using Jekyll's `relative_url` filter:

   ```markdown
   [Download PDF]({{ '/assets/example.pdf' | relative_url }})
   ```

3. Build the site: the file will be copied to `blog/assets/example.pdf` and accessible at `/blog/assets/example.pdf` on the live site.

4. Commit the assets in `blog-src/assets/` to Git so they are tracked and deployed.

