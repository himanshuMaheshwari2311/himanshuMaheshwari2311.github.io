# stark-dev.com

Personal website and blog built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Local Development

```bash
# Install Hugo
brew install hugo

# Clone with submodules
git clone --recurse-submodules <repo-url>

# Run dev server
hugo server -D

# Build for production
hugo --gc --minify
```

## Adding Content

### Blog post
```bash
hugo new content/posts/my-new-post.md
```

### Book review
```bash
hugo new content/bookshelf/book-title.md
```

## Deployment

Pushes to `master` trigger automatic deployment to GitHub Pages via GitHub Actions.
