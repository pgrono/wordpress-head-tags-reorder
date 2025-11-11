# Head Tags Reorder

A simple yet powerful WordPress plugin that automatically reorders the tags in your `<head>` section for optimal loading performance, ensuring critical resources are prioritized.

![WordPress](https://img.shields.io/badge/WordPress-5.5%2B-blue.svg)
![PHP](https://img.shields.io/badge/PHP-7.4%2B-blueviolet.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## The Problem

By default, WordPress, themes, and various plugins add tags (`<meta>`, `<link>`, `<script>`, `<style>`) to the `<head>` section based on their own execution priority. This often results in a suboptimal order that can negatively impact page rendering speed and Core Web Vitals.

For example, crucial preconnect or preload hints might appear after less important meta tags, or render-blocking stylesheets might be delayed by other elements.

## The Solution

This plugin solves the problem by intelligently rearranging the entire `<head>` section based on modern web performance best practices. It ensures that the most critical tags are placed first, allowing the browser to discover and fetch important resources sooner.

## Key Features

-   **Automatic Reordering**: No configuration needed. Just install and activate.
-   **Universal Compatibility**: Works with any theme or plugin (including Yoast SEO, caching plugins, etc.) because it processes the final HTML output.
-   **Performance-First Logic**: The sorting order is designed to improve key performance metrics like First Contentful Paint (FCP) and Largest Contentful Paint (LCP).
-   **Lightweight & Efficient**: The plugin has a minimal footprint and uses PHP's native `DOMDocument` for safe and reliable HTML parsing.
-   **Preserves All Tags**: It correctly handles all tags, including HTML comments, ensuring nothing is lost or broken.

## How It Works

The plugin uses a robust output buffering technique:

1.  **Capture**: It hooks into `wp_head` at the very beginning (priority `0`) and starts an output buffer, capturing everything that other functions would normally print.
2.  **Process**: It then hooks into `wp_head` at the very end (priority `9999`), stops the buffer, and gets the complete HTML content of the `<head>`.
3.  **Parse & Sort**: The captured HTML is safely parsed into a DOM tree. Each tag is assigned a "weight" based on its performance impact. The tags are then sorted in descending order of their weight.
4.  **Print**: The newly sorted list of tags is printed, resulting in a perfectly ordered `<head>` section in your site's source code.

## The Sorting Logic

Tags are reordered according to the following priority list (from highest to lowest):

1.  **Meta Charset** (`<meta charset="...">`)
2.  **Meta HTTP-Equiv** (e.g., X-UA-Compatible)
3.  **Title Tag** (`<title>`)
4.  **Meta Viewport**
5.  **Preconnect & DNS-Prefetch** (`<link rel="preconnect">`)
6.  **Async Scripts** (`<script async>`)
7.  **Inline Critical Styles** (`<style>...</style>`)
8.  **Synchronous Stylesheets** (`<link rel="stylesheet">`)
9.  **Preload Hints** (`<link rel="preload">`)
10. **Synchronous Scripts** (`<script src="...">`)
11. **Deferred Scripts** (`<script defer>`)
12. **SEO & Social Meta Tags** (Description, Open Graph, Twitter Cards)
13. **Canonical URL** (`<link rel="canonical">`)
14. **Prefetch & Prerender Hints**
15. **Other Meta Tags**
16. **Other Tags & Comments**

## Installation

1.  **Download**: Download the `.zip` file from this repository.
2.  **Upload**: In your WordPress admin dashboard, go to `Plugins` > `Add New` > `Upload Plugin`.
3.  **Select & Install**: Choose the downloaded `.zip` file and click `Install Now`.
4.  **Activate**: Activate the plugin.

That's it! The plugin will now automatically optimize the `<head>` section on every page load.

## License

This project is licensed under the MIT License - see the `LICENSE.md` file for details.
