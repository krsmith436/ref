Setting up a "headless" site means your repository contains the raw Markdown files and configuration, while GitHub automatically builds and hosts the professional-looking website for you.

---

## 1. Project Initialization

First, create the local structure for your documentation.

* **Install:** Run `python -m pip install mkdocs-material`.
* **Create:** Run `mkdocs new my-docs` to create the `my-docs` folder and generate the default files.
* **Organize:** Move your existing `.md` files into the `/docs` folder.


## 2. The Ideal Folder Structure

First, move your existing reference files into a folder named `docs`. MkDocs looks there by default. It should look something like this:

```text
my-reference-docs/
├── mkdocs.yml        # Your configuration
└── docs/             # All your .md files go here
    ├── index.md      # Your "Home" page
    ├── setup/
    │   └── install.md
    ├── guides/
    │   └── cheatsheet.md
    └── img/
        └── favicon.ico

```

## 3. Configure The Navigation

Edit the `mkdocs.yml` file in your root directory to define the look and navigation.

This is how you create nested menus. If you don't define this, MkDocs will just list files alphabetically, which isn't great for a "Knowledge Base."

```yaml
site_name: krsmith436 Knowledge Base
site_url: https://krsmith436.github.io/ref/

theme:
  name: material
  logo: img/logo.png
  favicon: img/favicon.ico
  
  features:
    - navigation.tabs      # Adds a top navigation bar
    - search.share         # Allows sharing search results
    - content.code.copy    # Adds a "Copy" button to code blocks

markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.emoji
  - attr_list
  
nav:
  - 🏠 Home: index.md
  - 🚂 Model Railroad:
      - Simple Computer Aided Railway Modeler: mr/scarm.md
  - 📚 Reference Guides:
      - Python Cheatsheet: guides/github_reference.md
      - Raspberry Pi Cheatsheet: guides/rpi_bash_reference.md
      - BASH and Python Time Reference: guides/rpi_time_reference.md
  - 🛠️ Technical Setup:
      - GitHub Headless Page: setup/github_headless_page.md
      - Pc Access to GitHub via SSH: setup/github_ssh_pc_access.md
```

### 3a. Adding "Admonitions"
Since this is for reference information, you’ll likely want to highlight "Notes" or "Warnings." Material for MkDocs has a great feature for this.

With this in your `mkdocs.yml`:

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

You can write in your `.md` files:

```markdown
!!! note "Pro Tip"
    This is a helpful tip for your future self!

!!! danger "Critical"
    Don't forget to backup the database before running this script.
```

If you don’t want a title, use a blank string "":

```markdown
!!! important ""
    This is an admonition box without a title.
```

Python-Markdown 3.10.2 suggests the following [Admonition types](https://python-markdown.github.io/extensions/admonition/); however, you’re free to use whatever you want:

!!! hint ""
    `attention`, `caution`, `danger`, `error`, `hint`, `important`, `note`, `tip`, and `warning`

### 3b. Adding `site_url`

Ensure your configuration file has the correct site_url so that links and assets resolve properly once hosted.

```yaml
site_name: krsmith436 Knowledge Base
site_url: https://krsmith436.github.io/ref/

theme:
  name: material
```

## 4. Launch Your Site Locally

Now, run the local server to see your new organization:

```bash
python -m mkdocs serve

```

Visit `http://127.0.0.1:8000`. You should see your categorized sidebar on the left and a functional search bar at the top.

## 5. Enable Automated Deployment (The "Headless" Part)

You don't want to manually upload HTML files. Instead, use a **GitHub Action** to do the heavy lifting.

1. In your local repository, create a folder path: `.github/workflows/`.
2. Create a file inside named `deploy.yml` and paste the following configuration:

```yaml
name: ci 
on:
  push:
    branches:
      - main # Or 'master' depending on your default branch name
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: pip install mkdocs-material # Add other plugins here if needed
      - run: mkdocs gh-deploy --force
```

!!! tip "Tip"
    If you use specific plugins (like `mkdocs-minify-plugin` or `mkdocs-git-revision-date-localized-plugin)`, make sure to add them to the `run: pip install ...` line in your YAML file so the build doesn't fail.

3. Every time you `git push` a Markdown change to `main`, GitHub will:
* Spin up a temporary virtual machine.
* Install Python and MkDocs.
* Build your site.
* Deploy it to a hidden `gh-pages` branch.

## 6. Connect to GitHub

Create a new, empty repository on GitHub (e.g., `my-reference-docs`) and link your local project to it:

```bash
git init
git add .
git commit -m "Initial docs setup"
git remote add origin https://github.com/krsmith436/ref.git
git push -u origin main

```

## 7. Activate the Site with GitHub Pages

1. Navigate to the `ref` repository on GitHub.
2. Click on **Settings > Pages**(in the left sidebar).
2. Under **Build and deployment**, ensure the **Source** is set to "Deploy from a branch."
3. Under **Branch**, select **`gh-pages`** and the `/(root)` folder (this branch is created automatically after your first Action run).
4. Click **Save**.
5. Your site will be live at `https://krsmith436.github.io/ref/`.