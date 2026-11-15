# Threat model — yaoleifly/yaoleifly.github.io

## System overview
A static blog that Hexo 5.4.0 generated (default `landscape` theme) and that GitHub Pages serves at `https://yaoleifly.github.io`. The repository holds only generated output: HTML pages (`index.html`, `archives/**`, `2021/07/01/*/index.html`), CSS and fonts (`css/`), and JavaScript (`js/script.js`, `js/jquery-3.4.1.min.js`, `fancybox/jquery.fancybox.min.js`). The content is still the Hexo demo ("Hello World" posts, `og:url` http://example.com, author "John Doe"). The last commit was in July 2021. The Hexo source (config, theme, markdown) is not in this repository.

There is no server-side code, database, authentication, forms that post data (the search box is a GET to google.com), cookies, or secrets.

## Components
- **Site pages**: the generated HTML (`index.html`, `archives/`, `2021/`).
- **Theme script**: `js/script.js` (search toggle, share box, image captions/gallery, mobile nav).
- **Vendored libraries**: `js/jquery-3.4.1.min.js`, `fancybox/` (fancyBox 3.5.7), `css/fonts/` (Font Awesome).
- **Hosting**: GitHub Pages; its configuration is not in code.

## Trust boundaries and entry points
- Anonymous visitors load pages over HTTPS from GitHub Pages. The only input they control is the URL (path, query, fragment). They cannot change the content.
- Content is author-controlled: posts are built from the author's markdown. `js/script.js` puts `data-url`, `data-title` and image `alt` into HTML without escaping, but those values come from the author's content.
- Third-party loads: `https://cdn.jsdelivr.net/npm/typeface-source-code-pro` (CSS, no SRI). External share links go to Twitter, Facebook, Pinterest and LinkedIn.
- Write access: whoever can push to the repository or the GitHub account (outside the scope of code).

## Assets
- Integrity of the site and the reputation of the `yaoleifly.github.io` origin (defacement, injected script, phishing).
- Visitors' browsers (script that runs on the origin). Other `*.github.io` sites are a different origin, and no cookies or sessions are at stake.

## Attacker profiles
- Anonymous web visitor who crafts URLs or links (reflected or DOM XSS through the URL).
- Third-party supply chain (CDN-hosted CSS, vendored libraries with known CVEs).
- (Assumed, unconfirmed) No malicious content authors: only the owner writes content.

## Existing controls
- GitHub Pages serves HTTPS. The site is static, so there is no server attack surface.
- Most JavaScript is vendored locally rather than loaded from a CDN.

## Out of scope / accepted risk
- None stated by the user yet.

## Open questions
- Is the site live and does it matter, or is it an abandoned demo?
- Does anyone other than the owner contribute content?
- Is the Hexo source (`yaoleifly/hexo`?) in scope?
