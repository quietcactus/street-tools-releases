# street-tools-releases

Public release channel for **Street Tools**, a suite of standalone WordPress admin tools housed under a single plugin.

This repository holds no source code. Releases are mirrored here automatically whenever a release is published in the private source repository, so every tag here matches a tag there.

## Download

Get `street-tools.zip` from the [latest release](https://github.com/quietcactus/street-tools-releases/releases/latest). Older versions stay available on the [releases list](https://github.com/quietcactus/street-tools-releases/releases).

## Install

1. In WordPress, go to **Plugins > Add New > Upload Plugin**.
2. Upload `street-tools.zip` and activate it.
3. Open **Street Tools** in the admin sidebar.

Or unzip it into `wp-content/plugins/` so the plugin lives at `wp-content/plugins/street-tools/street-tools.php`.

Notes:

- The plugin hides itself from the Plugins screen once active. Use the **Street Tools** admin menu to reach it.
- Every tool requires the `manage_options` capability, so only administrators can use them.

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

| Tool | What it does |
|------|--------------|
| ACF Default Backfill | Fill empty ACF fields with their configured default value across all posts of a chosen post type. |
| ACF Field Audit | Find ACF fields that no template references and no post has filled in, so you know which are safe to delete. |
| Broken Link Checker | Scan published content for broken links and images, with batched HTTP checks. |
| Bulk Image Assigner | Bulk assign or clear media images on ACF image fields or the featured image across a post type. |
| Bulk Page Creation | Bulk create posts, pages or custom post types from a single repeater form. |
| Favicon Checker | Check that favicon markup, the .ico size variants and every icon file are present and non-empty. |
| Phone Number Checker | Scan published content for phone numbers not wrapped in a `tel:` link. |
| Post Type Mover | Bulk move posts between post types, with a dry-run preview, ACF field mapping and per-run undo. |
| Redirect List Builder | Turn pasted old and new URLs into a redirect list and download it as a .txt. |
| Robots.txt Check | See the robots.txt the site serves, and whether a physical file overrides WordPress and your SEO plugin. |
| Schema Checker | List the JSON-LD and microdata structured data a page actually emits. |
| Search & Replace | Replace text across database tables, serialized data included, behind a required dry run. |
| Theme Editor | Monaco-based editor for every installed theme, with fuzzy file search, project-wide grep, go-to-definition and revertible saves. |
| Usage Finder | See what uses a page template, template part, field group, flexible layout or shortcode before you change or delete it. |
| Webp Converter | Upload or pick existing images and convert or compress them to WebP in place. |

## Settings and shortcuts

**Street Tools > Settings** holds per-user options, all off by default:

- **Admin bar shortcut:** `Ctrl/Cmd+Shift+Y` shows or hides the admin bar on the frontend.
- **Page search:** `Ctrl/Cmd+K` opens a search palette for pages and public custom post types. Enter opens the page, Shift+Enter opens the editor.

## Reporting issues

[Open an issue](https://github.com/quietcactus/street-tools-releases/issues/new) with:

- The Street Tools version (shown in the release you installed)
- Your WordPress and PHP versions
- Which tool is affected
- Steps to reproduce, and what you expected versus what happened
