# CLAUDE.md - AI Assistant Guide for MitchellFoley.github.io

## Project Overview

This is a **static HTML portfolio/ePortfolio website** for Mitchell Foley, built on the **Kerge Resume/CV/vCard template v2.4** by lmpixels. It is hosted via GitHub Pages at `MitchellFoley.github.io`.

There is **no build system, no package manager, and no framework** — just plain HTML, CSS, JavaScript, and one PHP file for contact form handling.

## Repository Structure

```
├── index.html               # Main single-page application (all sections)
├── portfolio-1.html          # CSC6200 project detail page (AJAX-loaded)
├── portfolio-2.html          # CSC8470 Cornell-Cards detail page (AJAX-loaded)
├── Mitchell_Foley_Resume.pdf # Downloadable resume
├── favicon.ico
├── css/
│   ├── main.css              # Primary custom styles (~3500 lines)
│   ├── animations.css        # Page transition animations
│   ├── bootstrap.min.css     # Bootstrap 3
│   ├── animate.css           # CSS animation library
│   ├── owl.carousel.css      # Carousel plugin styles
│   ├── magnific-popup.css    # Lightbox plugin styles
│   ├── normalize.css
│   ├── font-awesome.css      # Legacy Font Awesome
│   ├── Linearicons-Free.css  # Icon font
│   └── pe-icon-7-stroke.css  # Icon font
├── js/
│   ├── main.js               # Custom site JavaScript (~364 lines)
│   ├── jquery-2.1.3.min.js   # Primary jQuery
│   ├── jquery-1.12.4.min.js  # Legacy jQuery fallback
│   ├── bootstrap.min.js      # Bootstrap 3 JS
│   ├── modernizr.custom.js   # Feature detection
│   ├── jquery.shuffle.min.js # Portfolio grid filtering
│   ├── jquery.magnific-popup.min.js
│   ├── owl.carousel.min.js
│   ├── masonry.pkgd.min.js
│   ├── imagesloaded.pkgd.min.js
│   ├── jquery.malihu.PageScroll2id.min.js
│   ├── validator.js           # Form validation
│   └── jquery.googlemap.js
├── fonts/
│   └── fontawesome-free/      # Font Awesome 5 (woff2, eot, svg)
├── images/
│   ├── photo.jpg              # Profile photo
│   ├── portfolio/             # Thumbnails (1-12.jpg)
│   └── portfolio/full/        # Full-size portfolio images
└── contact_form/
    └── contact_form.php       # Server-side form handler (reCAPTCHA v2)
```

## Development Workflow

### No Build Step Required

This project has **no build process**. Files are served as-is by GitHub Pages. To preview locally, use any static file server:

```bash
# Python 3
python3 -m http.server 8000

# Node.js (if npx available)
npx serve .

# PHP (needed for contact form testing)
php -S localhost:8000
```

### No Package Manager

There is no `package.json`, `Gemfile`, or equivalent. All dependencies are vendored directly in `js/`, `css/`, and `fonts/` directories.

### No Tests or Linting

There are no test frameworks, linting tools, or CI/CD pipelines configured.

## Architecture and Navigation

### Single-Page Application Pattern

`index.html` is the main entry point containing all page sections. Navigation uses **hash-based routing** with smooth scrolling (via PageScroll2id plugin). Sections are identified by ID:

| Hash         | Section                     |
|--------------|-----------------------------|
| `#about-me`  | Introduction, rotating title, CV download |
| `#resume`    | Education and experience timelines |
| `#portfolio` | Project showcase grid       |
| `#career`    | Career plan (short/medium/long-term) |
| `#contact`   | Contact info and form       |

### Portfolio Detail Pages

`portfolio-1.html` and `portfolio-2.html` are **loaded via AJAX** into a Magnific Popup modal when a portfolio item is clicked. They are not standalone pages — they contain only partial HTML (no `<html>`/`<head>`/`<body>` wrappers).

### Page Transitions

Animated page transitions are managed by `css/animations.css` and controlled via JavaScript. Each section uses class `pt-page` for transition targeting.

## Code Conventions

### HTML
- Semantic HTML5 with `<section>` elements
- Bootstrap 3 grid system (`col-xs-*`, `col-sm-*`, `col-md-*`)
- Data attributes for JS behavior (`data-group`, `data-groups`)
- Navigation triggers use class `pt-trigger`
- IDs use snake_case or kebab-case inconsistently (`site_header`, `about-me`)

### CSS (`css/main.css`)
- Organized in numbered subsections (1-12) with comment headers
- Vendor prefixes included (`-webkit-`, `-moz-`, `-o-`, `-ms-`)
- Primary color: `#0099e5` (blue), accent: `#FF9800` (orange)
- Mobile-first responsive design with media queries
- Google Fonts: Poppins (weights 200-700)

### JavaScript (`js/main.js`)
- jQuery-dependent, uses IIFE strict mode: `(function($) { "use strict"; ... })(jQuery);`
- Document ready via `$(document).on('ready', ...)`
- Plugin initialization pattern for Shuffle, Masonry, Owl Carousel, Magnific Popup
- AJAX-based portfolio detail loading with hash routing

## Key Files to Edit

| Task                        | File(s)                          |
|-----------------------------|----------------------------------|
| Update personal info/bio    | `index.html` (About section)     |
| Add/edit portfolio items    | `index.html` + new `portfolio-N.html` |
| Modify styling              | `css/main.css`                   |
| Change site behavior        | `js/main.js`                     |
| Update resume               | Replace `Mitchell_Foley_Resume.pdf` |
| Edit contact form backend   | `contact_form/contact_form.php`  |
| Add portfolio images        | `images/portfolio/` and `images/portfolio/full/` |

## Security Notes

- `contact_form/contact_form.php` contains a hardcoded reCAPTCHA secret key. This should be moved to an environment variable or server config if the form is actively used.
- The PHP contact form does not sanitize `$_POST` values before embedding in the email body — potential XSS/injection vector if the email client renders HTML.

## Known Constraints

- jQuery 2.1.3 does not support IE8 and below; the fallback jQuery 1.12.4 is included but both are loaded (only one should be used)
- Bootstrap 3 is end-of-life — no security patches
- No `.gitignore` file exists
- Google Maps integration in `js/jquery.googlemap.js` requires an API key to function
- The contact form requires a PHP-capable server (GitHub Pages does not support PHP)
