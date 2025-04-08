<!--
  Copyright 2025 Stanislav Senotrusov

  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->

# html-assets-hash

*A lightweight CLI tool that appends short hashes to your HTML asset URLs to help avoid loading outdated files from the browser cache.*

This tool replaces placeholder version parameters in your HTML asset URLs with a hash of the corresponding file’s contents. It scans for `<script>` and `<link>` tags that include `src` or `href` attributes with a version parameter like `?v=`, such as:

```html
<script src="app.js?v="></script>
<link href="style.css?v=" rel="stylesheet">
```

This placeholder acts as an intentional marker — only assets with `?v=` will be modified. When run, the tool computes a SHA-256 hash of each referenced asset (e.g. `app.js`, `style.css`) and inserts the first 8 characters of that hash into the URL:

```html
<script src="app.js?v=a1b2c3d4"></script>
<link href="style.css?v=e5f6g7h8" rel="stylesheet">
```

If a version string is already present (e.g. `?v=old1234`), it will be updated with the current hash. Assets without a `?v=` marker remain unchanged, giving you control over which URLs should be processed.

* External URLs (like CDNs) are ignored  
* Asset URLs with multiple query parameters (e.g. `?v=1234&foo=bar`) are not modified

For simplicity and zero dependencies, the HTML is parsed using regular expressions. While not a general-purpose solution for HTML parsing, it's reliable for this narrowly defined use case.

## Usage

```bash
npx html-assets-hash <html-file-path> [base-path]
```

* `<html-file-path>`: Path to the HTML file to process  
* `[base-path]`: (Optional) Base directory to resolve relative asset paths. Defaults to the HTML file's directory

### Example

```bash
npx html-assets-hash public/index.html
```

This will transform:

```html
<script src="app.js?v="></script>
<link href="styles.css?v=old456" rel="stylesheet">
```

Into:

```html
<script src="app.js?v=a1b2c3d4"></script>
<link href="styles.css?v=e5f6g7h8" rel="stylesheet">
```

## License

This project is licensed under the terms of the [Apache License, Version 2.0](LICENSE)

## Get involved and see the contributors

To learn how to contribute, check out the [CONTRIBUTING](CONTRIBUTING.md) file. You can also take a look at the [CONTRIBUTORS](CONTRIBUTORS.md) file to see who has helped out so far.
