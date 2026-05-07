# Migration Notes

This repository now uses the Jekyll/al-folio academic template from `ztqakita/academic-personal-website`.

The previous Hugo/Toha site source has been preserved under `_legacy_hugo_site/` so the old posts, assets, layouts, and configuration are still available in the repository. The Jekyll build excludes that archive directory, so it will not be published as part of the new site.

Useful preserved paths:

- `_legacy_hugo_site/content/`: old Hugo posts and pages
- `_legacy_hugo_site/static/`: old static files, post images, and resume
- `_legacy_hugo_site/data/`: old profile, sections, projects, and experience data
- `_legacy_hugo_site/layouts/`: old Hugo layout customizations

New template paths to customize next:

- `_config.yml`: site-wide identity, URL, features, and build settings
- `_pages/about.md`: homepage biography and profile block
- `_data/socials.yml`: social links, email, and CV link
- `_projects/`: project cards
- `_posts/`: Jekyll blog posts
