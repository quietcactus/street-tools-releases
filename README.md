# street-tools-releases

Public release channel for **Street Tools**, a suite of standalone WordPress admin tools housed under a single plugin.

## About

Street Tools bundles the audits, content utilities and developer tools that come up on almost every WordPress build into one plugin. Each tool lives on its own admin page, works on its own, and only loads where it is used, so nothing extra runs on the frontend.

This repository holds no source code. The source lives in a private repository, and every release published there is mirrored here automatically, so every tag here matches a tag there. Each release carries:

- `street-tools.zip`, the installable plugin
- `info.json`, the manifest the plugin reads to offer updates in WordPress

Current version: **3.11.0**. See the [releases list](https://github.com/quietcactus/street-tools-releases/releases) for the full changelog.

## Download

Get `street-tools.zip` from the [latest release](https://github.com/quietcactus/street-tools-releases/releases/latest). Older versions stay available on the [releases list](https://github.com/quietcactus/street-tools-releases/releases).

## Requirements

- WordPress 6.0 or later
- PHP 7.4 or later
- [Advanced Custom Fields](https://www.advancedcustomfields.com/) for ACF Default Backfill, ACF Field Audit, Unused Field Checker and the ACF Field Overlay setting
- [Yoast SEO](https://yoast.com/wordpress/plugins/seo/) for Yoast Setup

A tool whose required plugin is not active stays listed but is marked unavailable.

## Install

1. In WordPress, go to **Plugins > Add New > Upload Plugin**.
2. Upload `street-tools.zip` and activate it.
3. Open **Street Tools** in the admin sidebar, or use the **Settings** link on the plugin's row in the Plugins screen.

Or unzip it into `wp-content/plugins/` so the plugin lives at `wp-content/plugins/street-tools/street-tools.php`.

Every tool requires the `manage_options` capability, so only administrators can use them.

### Hiding the plugin

The plugin shows on the Plugins screen and in the admin sidebar by default. To hide either one, add these to `wp-config.php`:

```php
define('STREET_TOOLS_HIDE_PLUGIN_ROW', true); // hide the row on the Plugins screen
define('STREET_TOOLS_HIDE_MENU', true);       // hide the Street Tools admin menu
```

## Update

Since 3.9.0, Street Tools updates from this repository through the normal WordPress update flow. When a new release is published here, it shows up on **Dashboard > Updates** and on the Plugins screen like any other plugin update. The per-site **Enable auto-updates** toggle works as usual; nothing turns it on for you.

- The plugin checks this repository's latest release at most every 12 hours, so a new release can take that long to appear.
- It never talks to wordpress.org. The plugin's `Update URI` points here, which also keeps it from being replaced by an unrelated wordpress.org plugin with a similar name.

Sites on a version older than 3.9.0 have no updater yet. Update those once by hand: download the newest zip and upload it the same way you installed it, choosing **Replace current with uploaded** when WordPress asks, or replace the `street-tools/` folder over SFTP. After that, updates arrive in WordPress.

Check the release notes for breaking changes before upgrading.

### Upgrading from Paperstreet Tools

Street Tools installs into a new folder, so WordPress sees it as a separate plugin. Install and activate `street-tools.zip`. On first load it migrates the old data (Search & Replace undo tables, Post Type Mover history, Theme Editor revisions, per-user settings) and deactivates the old copy. Then delete `wp-content/plugins/paperstreet-tools/` over SFTP.

The migration does not carry over:

- Bookmarks to `admin.php?page=wptools-<tool-id>`. Pages now live at `admin.php?page=street-tools-<tool-id>`.
- Theme or mu-plugin code hooked to old `wptools_` filters or actions. Rename the prefix to `street_tools_`.
- Cached scan results. They rebuild on the next scan.

## Tools

### Audits / Checks

| Tool | What it does |
|------|--------------|
| ACF Field Audit | Pick a post type and some ACF fields, then see what every post has stored in them, images and repeater rows included. Requires ACF. |
| Broken Link Checker | Scan published content for broken links and images, with batched HTTP checks. |
| Favicon Checker | Check that favicon markup, the .ico size variants and every icon file are present and non-empty. |
| H1 Checker | List every H1 saved in a post type's content and custom fields, including ACF and Elementor. |
| Metadata Checker | Find pages sharing a duplicate page title, title tag or meta description. With Yoast SEO active it reads Yoast's rendered values without fetching each page. Without Yoast, or when you ask it to verify live pages, it reads each page as a logged-out visitor sees it. |
| Phone Number Checker | Scan published content for phone numbers not wrapped in a `tel:` link. |
| Robots.txt Checker | See the robots.txt the site serves, and whether a physical file overrides WordPress and your SEO plugin. |
| Schema Checker | List the JSON-LD and microdata structured data a page actually emits. |
| Unused Field Checker | Find ACF fields that no template references and no post has filled in, and delete them from the same screen. Fields that are still in use can be deleted too, behind a risk confirmation. Shift-click selects a range. Requires ACF. |
| URL Checker | Find slugs that repeat a word from their parent URL or end in a `-2` style suffix. |

### Content

| Tool | What it does |
|------|--------------|
| ACF Default Backfill | Fill empty ACF fields with their configured default value across all posts of a chosen post type, choosing which fields to backfill. Requires ACF. |
| Bulk Page Creation | Bulk create posts, pages or custom post types from a single repeater form. Can also seed up to 50 lorem ipsum posts for testing and trash them all again in one click without touching real content. |
| Post Type Mover | Bulk move posts between post types, with a dry-run preview, ACF field mapping and per-run undo. |
| Site Architecture | Map every published page into a URL tree across all post types and export it to Excel, with title tags, meta descriptions, templates and indexable status. |

### Media

| Tool | What it does |
|------|--------------|
| Alt Text Checker | Find images with missing or filename-style alt text, fix media library alt text in place, and export the lists to CSV. |
| Bulk Image Assigner | Bulk assign or clear media images on ACF image fields or the featured image across a post type. |
| Webp Converter | Upload or pick existing images and convert or compress them to WebP in place, with the option to repoint every place the old image is used, ACF options pages included. WebP and SVG uploads are accepted as they are. |

### Development

| Tool | What it does |
|------|--------------|
| Redirect List Builder | Turn pasted old and new URLs into a redirect list and download it as a .txt. |
| Search & Replace | Replace text across database tables, serialized data included, behind a required dry run. Shows the page each found row belongs to, highlights matches in the samples, and can select every table at once. |
| Theme Editor | Monaco-based editor for every installed theme, with fuzzy file search, project-wide grep, go-to-definition and revertible saves. |
| Usage Finder | See what uses a page template, template part, field group, flexible layout or shortcode before you change or delete it, and which posts embed iframes. |
| Yoast Setup | Compare Yoast SEO settings with the PaperStreet checklist and apply the checklist in one step. Current settings are backed up before every write and can be restored from the same page. Requires Yoast SEO. |

Image Checker was removed in 3.10.8.

## Settings

**Street Tools > Settings** holds per-user options, all off until each user switches them on:

- **Admin Bar Shortcut:** `Ctrl/Cmd+Shift+Y` shows or hides the admin bar on the frontend. This shares the "Show Toolbar when viewing site" profile setting.
- **Page Search:** `Ctrl/Cmd+K` on the frontend opens a search palette for pages, public custom post types, Street Tools screens and the WordPress dashboard. On WP Engine sites it also lists the WP Engine page and its Caching tab. Drafts and private pages are included. Enter opens the page, Shift+Enter opens the editor, and holding Ctrl with either opens it in a new tab.
- **Better Menus:** adds A to Z sub-item sorting and bulk moving to **Appearance > Menus**. Pick a parent item, then sort its sub-items or, with Bulk Select on, move ticked items under it. Nothing is saved until you click **Save Menu**.
- **ACF Field Overlay:** `Ctrl/Cmd+Shift+E` on the frontend outlines content that comes from ACF fields. Each outline links to its field in the editor, opened in a new tab, and Esc closes the overlay. The **Edit ACF fields** item in the admin bar does the same, for browsers that keep the shortcut for themselves. Requires ACF.
- **Convert Uploads to WebP:** JPG and PNG images you upload to the Media Library are converted to WebP, and only the WebP is kept. Other file types are left alone, and if the server cannot make WebP the original is kept.

## Uninstall

Deleting the plugin from the Plugins screen drops the Search & Replace undo tables, which hold copies of changed content, on every site of a multisite network. Deactivating it leaves everything in place.

## Reporting issues

[Open an issue](https://github.com/quietcactus/street-tools-releases/issues/new) with:

- The Street Tools version (shown in the Street Tools header and in the release you installed)
- Your WordPress and PHP versions
- Which tool is affected
- Steps to reproduce, and what you expected versus what happened
