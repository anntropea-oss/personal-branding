# Ann Tropea Professional Website

A standalone static website for Ann Tropea's professional work across editorial leadership, communications strategy, technical communication, AI literacy, consulting services, higher education, and public-impact storytelling. The structure borrows the useful process from the referenced Codex project: a separate web property, SEO-ready metadata, launch verification, and a written problem/solution trail.

## Local Preview

```bash
python3 -m http.server 4173
```

Then open `http://localhost:4173`.

## Customize Before Launch

- Replace the GitHub Pages URL in `index.html`, `robots.txt`, and `sitemap.xml` if moving to a custom domain.
- Add a preferred contact email if the contact button should be email-based instead of LinkedIn-based.
- Add real professional links, case studies, talks, writing samples, edited publications, podcast samples, GitHub projects, and social profiles.
- Add a resume PDF if the `Request Resume` button should become a direct download.
- Replace portfolio request links with public sample URLs or local PDFs when ready.
- Replace or deepen the homepage copy once the strongest role target is chosen.
- Regenerate `sitemap.xml` or update `lastmod` on every content release.

## Launch Checklist

- [x] Choose canonical domain.
- [x] Update canonical URL, Open Graph URL/image, `robots.txt`, and `sitemap.xml`.
- [ ] Verify all internal links.
- [ ] Preview desktop and mobile layouts.
- [ ] Deploy from Git.
- [ ] Confirm the homepage returns `200`.
- [ ] Confirm noncanonical hosts redirect correctly.
- [ ] Submit sitemap in Search Console.
- [ ] Log launch issues and fixes in `SOLUTIONS.md`.
