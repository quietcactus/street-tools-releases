# street-tools-releases

Public release channel for **Street Tools**, a suite of standalone WordPress admin tools housed under a single plugin.

This repository holds no source code. Releases are mirrored here automatically whenever a release is published in the private source repository, so every tag here matches a tag there.

## Download

Get `street-tools.zip` from the [latest release](https://github.com/quietcactus/street-tools-releases/releases/latest). Older versions stay available on the [releases list](https://github.com/quietcactus/street-tools-releases/releases).

## Requirements

- WordPress 6.0 or later
- PHP 7.4 or later
- [Advanced Custom Fields](https://www.advancedcustomfields.com/) for ACF Default Backfill, Unused Field Checker and the ACF field overlay
- [Yoast SEO](https://yoast.com/wordpress/plugins/seo/) for Yoast Setup, and for the Metadata Checker's fast scan

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

Street Tools does not update through wordpress.org (`Update URI: false`), and it never will. To update, download the newest zip and upload it the same way you installed it, choosing **Replace current with uploaded** when WordPress asks. You can also replace the `street-tools/` folder over SFTP.

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
| Broken Link Checker | Scan published content for broken links and images, with batched HTTP checks. |
| Favicon Checker | Check that favicon markup, the .ico size variants and every icon file are present and non-empty. |
| H1 Checker | List every H1 saved in a post type's content and custom fields, including ACF and Elementor. |
| Image Checker | List the image every post holds per image field, spot empty or broken ones, and export the lot to CSV. |
| Metadata Checker | Find pages sharing a duplicate page title, title tag or meta description. With Yoast SEO active it reads the values Yoast renders in one fast pass. **Verify against live pages** fetches each page as a logged-out visitor sees it instead, catching titles a theme or another plugin writes. Sites without Yoast are always checked live. |
| Phone Number Checker | Scan published content for phone numbers not wrapped in a `tel:` link. |
| Robots.txt Checker | See the robots.txt the site serves, and whether a physical file overrides WordPress and your SEO plugin. |
| Schema Checker | List the JSON-LD and microdata structured data a page actually emits. |
| Unused Field Checker | Find ACF fields that no template references and no post has filled in, so you know which are safe to delete. |
| URL Checker | Find slugs that repeat a word from a parent URL or from themselves, or end in a `-2` style suffix. Stop words such as "and" or "the" are ignored, and the list is editable. |

### Content

| Tool | What it does |
|------|--------------|
| ACF Default Backfill | Fill empty ACF fields with their configured default value across all posts of a chosen post type. |
| Bulk Page Creation | Bulk create posts, pages or custom post types from a single repeater form. |
| Post Type Mover | Bulk move posts between post types, with a dry-run preview, ACF field mapping and per-run undo. |
| Site Architecture | Map every published page into a URL tree across all post types and export it to Excel, with title tags, meta descriptions, templates and indexable status. |

### Media

| Tool | What it does |
|------|--------------|
| Alt Text Checker | Find images with missing or filename-style alt text in the media library and in published content, fix media library alt text in place, and export the lists to CSV. Empty `alt=""` is listed separately, since it is correct for decorative images. |
| Bulk Image Assigner | Bulk assign or clear media images on ACF image fields or the featured image across a post type. |
| Webp Converter | Upload or pick existing images and convert or compress them to WebP in place. |

### Development

| Tool | What it does |
|------|--------------|
| Redirect List Builder | Turn pasted old and new URLs into a redirect list and download it as a .txt. |
| Search & Replace | Replace text across database tables, serialized data included, behind a required dry run. Each found row shows the page it belongs to. |
| Theme Editor | Monaco-based editor for every installed theme, with fuzzy file search, project-wide grep, go-to-definition and revertible saves. |
| Usage Finder | See what uses a page template, template part, field group, flexible layout or shortcode before you change or delete it. |
| Yoast Setup | Compare Yoast SEO settings with the PaperStreet checklist and apply the checklist in one step. Current settings are backed up before every write and can be restored from the same page. Requires Yoast SEO. |

## Settings

**Street Tools > Settings** holds per-user options, all off until each user switches them on:

- **Admin Bar Shortcut:** `Ctrl/Cmd+Shift+Y` shows or hides the admin bar on the frontend. This shares the "Show Toolbar when viewing site" profile setting.
- **Page Search:** `Ctrl/Cmd+K` on the frontend opens a search palette for pages, public custom post types and Street Tools screens. Drafts and private pages are included. Enter opens the page, Shift+Enter opens the editor.
- **Better Menus:** adds A to Z sub-item sorting and bulk moving to **Appearance > Menus**. Pick a parent item, then sort its sub-items or, with Bulk Select on, move ticked items under it. Nothing is saved until you click **Save Menu**.
- **ACF Field Overlay:** `Ctrl/Cmd+Shift+E` on the frontend outlines the content each ACF field on the page produced. Click an outline to open the post or options page in a new tab with that field opened, scrolled to and focused. Fields used by the template but not found on the page are listed separately. The **Edit ACF fields** admin bar item toggles it too. Esc closes it. Requires ACF, and only users who switched it on ever see it.
  - Not covered: fields on terms, users and blocks; post object, relationship and taxonomy fields; and values read unformatted.

## Reporting issues

[Open an issue](https://github.com/quietcactus/street-tools-releases/issues/new) with:

- The Street Tools version (shown next to the Street Tools name at the top of every Street Tools screen, linked to its release notes)
- Your WordPress and PHP versions
- Which tool is affected
- Steps to reproduce, and what you expected versus what happened
