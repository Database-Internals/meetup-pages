# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site for the Database Internals Meetup community. The site hosts:
- Meetup video recordings and transcripts
- Podcast episodes
- Reading lists organized by database topics (query optimization, storage, concurrency control, etc.)
- Contributor profiles

The site uses the Minima theme with custom styling and is automatically deployed to GitHub Pages via GitHub Actions on push to `main`.

## Development Setup and Common Commands

### Installation and Local Development

```bash
# Install dependencies (Ruby gems)
bundle install

# Run local development server (accessible at http://localhost:4000)
bundle exec jekyll serve

# Build static site to _site/ directory (for production verification)
bundle exec jekyll build
```

### Project Structure

- `_config.yml` - Jekyll configuration (site title, theme, navigation)
- `_posts/` - Blog posts for meetups and podcasts (naming: `YYYY-MM-DD-slug.md`)
- `_data/people.yml` - Contributor profiles with images and descriptions
- `_includes/` - HTML includes for custom components (head.html for custom CSS/meta, social.html)
- `assets/` - CSS and image files
  - `fixup.css` - Custom styling for hero sections on index, meetups, and podcasts pages
  - `images/` - Profile photos and background images
- `meetups/`, `podcasts/`, `reading-lists/` - Main content sections
- `about.md`, `index.md` - Static pages

## Content Guidelines

### Adding a New Post (Meetup or Podcast)

Posts are stored in `_posts/` with naming convention: `YYYY-MM-DD-slug.md`

**Post Frontmatter Structure:**
```yaml
---
layout: post
title: "[Type] [Number]: [Title]"
date: YYYY-MM-DD HH:MM:SS +0300
tags: [meetup]  # or [podcast]
---
```

**Post Content:**
- Use `<!--more-->` separator to define excerpt shown on index page
- For videos: embed with `<iframe>` tag and link to platform (YouTube, etc.)
- Use icons from Font Awesome 6.0 (e.g., `<i class="fab fa-youtube"></i>`)
- Markdown with HTML is supported

### Adding Contributors

Edit `_data/people.yml` with:
```yaml
- image: /assets/images/filename.jpg
  name: "Full Name"
  description: "Optional role/description"
```

Images should be placed in `assets/images/`.

### Creating Meetup Posts from TimepadIn Events

When a new meetup is announced on [TimepadIn](https://databaseinternals.timepad.ru/), create a corresponding post using this checklist:

1. **Identify the event details**
   - Get event number, date/time, location, speakers, and agenda from TimepadIn announcement

2. **Create the filename**: `YYYY-MM-DD-videoN.md`
   - Use the actual event date (not announcement date)
   - Use sequential numbering (video10 follows video9)
   - Verify the number against TimepadIn to avoid duplicates

3. **Use the appropriate template**
   - Standard meetup: 2-3 talks, evening timing (19:00), company office location (see `2025-10-15-video9.md`)
   - ISPRAS Open conference: 4-5 talks, afternoon timing (13:00), special location, passport required (see `2025-05-10-video5.md`)

4. **Content structure**
   - YAML frontmatter with correct date and tags
   - Brief description paragraph with `>` quote syntax
   - `<!--more-->` separator for excerpt
   - TODO comments for video embeds (to be filled after event)
   - Program/schedule section with times and speaker names
   - Detailed talk descriptions with speaker credentials

5. **Important notes**
   - Ensure UTF-8 encoding (Russian text must render correctly)
   - Preserve Russian text exactly as provided in TimepadIn
   - Use relative URLs: `[Back to Home]({{ "/" | relative_url }})`
   - Font Awesome icons for video links: `<i class="fab fa-youtube"></i>` and `<i class="fab fa-vk"></i>`

6. **Test locally (optional)**
   - Run `bundle exec jekyll serve` and verify formatting at `http://localhost:4000/meetups/`

7. **Commit and deploy**
   - Commit with message: `"Add Database Internals Meetup #N post"`
   - Push to main branch (GitHub Actions auto-deploys)

8. **Post-event: Add video embeds**
   - Once videos are available (typically within 1-2 weeks), update the post with YouTube and VK embeds
   - Replace TODO comments with actual iframe tags
   - Use the format from existing posts (video5 or video9)

**Common issues:**
- Mismatched numbering with TimepadIn → verify sequential number before creating
- Encoding problems with Russian text → ensure file is saved as UTF-8
- Video embeds not displaying → check iframe src URLs and allowfullscreen attribute
- Post not appearing on site → verify `tags: [meetup]` in frontmatter and rebuild Jekyll

### Reading Lists Organization

The reading lists page (`reading-lists/index.md`) organizes papers by database topics using a hierarchical structure. Topics include:
- Query Optimization (rewriting, cost-based, rule-based)
- Query Execution (compilation, vectorized, parallel)
- Storage (indexes, buffer manager, compression)
- Distributed Databases, Concurrency Control, and many others

Add papers to the relevant section with proper markdown formatting.

## Styling and Layout

### Hero Section Styling

The index, meetups, and podcasts pages have hero headers with background images. This is controlled by:
- `assets/fixup.css` - Defines `.page-index`, `.page-meetups .post-title`, `.page-podcasts .post-title` styles
- `_includes/head.html` - Includes custom CSS and Font Awesome

The hero background image uses `assets/images/db_internals_meetup.jpg` with a dark overlay for text contrast.

### Responsive Design

Mobile responsiveness is handled in `fixup.css` with media queries for screens under 640px width. Hero sections reduce height and font size on mobile.

### Theme Customization

The site uses Jekyll's Minima theme with the "solarized" skin. Customizations are made via:
- `_config.yml` - Theme settings (skin, social links)
- `_includes/head.html` - Additional CSS and meta tags
- `assets/fixup.css` - Custom styles overriding theme

## GitHub Pages Deployment

The site is automatically deployed via `.github/workflows/jekyll-gh-pages.yml` on every push to `main`:
1. Workflow triggers on push to main branch
2. Runs `jekyll build` to generate static site
3. Uploads artifacts to GitHub Pages
4. Deploys to live site

**Important**: Do not manually edit the deployment workflow unless modifying CI/CD behavior.

### Testing Before Deployment

**Recommended**: Always test changes locally before pushing to main:

```bash
# Install dependencies (if not already done)
bundle install

# Run local development server
bundle exec jekyll serve

# In another terminal, verify the site in browser at http://localhost:4000
# Check that new/modified posts appear with correct formatting
```

**What to verify**:
- Posts appear in the meetups or podcasts collection
- Russian text renders correctly (UTF-8 encoding)
- Links and images load properly
- Video embeds display (once added)
- No Jekyll build warnings or errors

**If local testing fails**: Fix the issue before pushing. GitHub Actions will catch build failures and prevent deployment, but testing locally provides faster feedback.

**Local environment notes**: If you encounter bundler or Ruby version issues locally, the GitHub Actions workflow uses its own Ruby environment and should build successfully even if local builds fail. However, it's still recommended to test if possible.

## Git and Version Control

- Track only source files: markdown files, YAML data files, images in `assets/`, CSS files
- Never commit the `_site/` directory (generated on build)
- Never commit `Gemfile.lock` (auto-generated by bundler)
- The `.gitignore` already excludes these

## Language and Content

This site is bilingual (Russian/English):
- Site metadata (titles, navigation) is in English
- Meetup and podcast content varies by language
- Some pages include Russian headers and descriptions (e.g., reading-lists/index.md)

When editing content, preserve the existing language style.

## Troubleshooting Local Development

**Issue**: Site not updating after edits
- Jekyll watches for changes; most edits trigger automatic rebuild
- For `_config.yml` changes, stop and restart `bundle exec jekyll serve`

**Issue**: Port 4000 already in use
```bash
bundle exec jekyll serve --port 4001
```

**Issue**: Missing gems
```bash
bundle install
bundle update
```

## Synchronization with TimepadIn Events

This section describes how to keep the website synchronized with meetup announcements on TimepadIn.

### Monitoring for New Events

- **Frequency**: Check [TimepadIn](https://databaseinternals.timepad.ru/) every 2 weeks for new event announcements
- **Community channels**: Subscribe to Telegram/Discord notifications for event announcements
- **Lead time**: Events are typically announced 2-4 weeks before the scheduled date

### Event Numbering and Sequencing

- Meetups are numbered sequentially: Database Internals Meetup №1, №2, etc.
- Current status: Meetups #1-10 exist on the website; future numbering (#11+) to be determined
- **Before creating a post**: Always verify the number from TimepadIn to avoid duplicate numbering

### Quality Assurance Checklist

Before committing a new post, verify:
- [ ] Event number matches TimepadIn and is sequential
- [ ] Date and time are correct (13:00 for ISPRAS Open, 19:00 for standard meetups)
- [ ] All speaker names and positions are accurate
- [ ] Russian text is preserved exactly and renders correctly (UTF-8 encoding)
- [ ] Schedule times are consistent with TimepadIn announcement
- [ ] Talk descriptions match the source material
- [ ] File naming follows convention: `YYYY-MM-DD-videoN.md`
- [ ] Post has been tested locally (optional but recommended)

### Handling Special Event Types

**ISPRAS Open Conferences:**
- Typically 4-5 talks (compared to 2-3 for standard meetups)
- Afternoon timing: starts at 13:00, not 19:00
- Note the specific location and conference details
- Mention passport requirement for in-person attendance
- May include panel discussions or special sessions

**Hybrid Events (офлайн + онлайн):**
- Clearly state hybrid format in the header
- Include both location (for in-person) and registration/streaming details
- Mention any special arrangements (like Yandex hosting the stream)

**Online-Only Events:**
- Omit physical location details
- Focus on streaming platform and registration links
- Note any company sponsors (e.g., "Трансляция мероприятия была организована... компанией")

### Post-Event Video Embedding

After an event occurs:
1. Wait for videos to be uploaded to YouTube and VK (typically 1-2 weeks)
2. Update the post file by replacing TODO comments with actual embed codes
3. Verify both YouTube and VK embeds display correctly
4. Commit the update with message: `"Add video embeds for Meetup #N"`

### Recommended Workflow

```
TimepadIn Event Announced
    ↓
Create meetup post using checklist (within 1 week)
    ↓
Commit: "Add Database Internals Meetup #N post"
    ↓
Push to main (GitHub Actions auto-deploys)
    ↓
Verify post appears on live site
    ↓
Event occurs
    ↓
Videos published (1-2 weeks after event)
    ↓
Update post with video embeds
    ↓
Commit: "Add video embeds for Meetup #N"
```

### Why Not Automated?

The current manual process is appropriate because:
- Low frequency (~12 events/year) doesn't justify automation overhead
- TimepadIn has no public API for scraping events
- Video embeds require manual updates anyway
- Human review ensures content quality and consistency
- Special events require flexible handling
- Maintenance burden would exceed time saved
