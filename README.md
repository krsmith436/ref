## 📂 The `ref` Project for krsmith436

The **`ref`** directory is a standalone documentation repository. It uses a "headless" architecture where raw Markdown files are automatically transformed into a searchable, professional website via **MkDocs** and **GitHub Pages**.

### Structure Overview:

* **`ref/` (Root):** The home of the project configuration.
* **`mkdocs.yml`**: The "brain" of the site. It controls the theme (Material), the navigation menu, and search functionality.
* **`.github/workflows/`**: Contains the automation logic that deploys the site whenever you push an update.


* **`docs/` (Content Source):** This is where the actual reference information lives.
* **`index.md`**: The landing page for your reference site.
* **`setup/`**: Technical installation and configuration logs.
* **`guides/`**: Procedural walkthroughs and cheatsheets.



### Key Features:

* **Instant Search:** A built-in global search bar to find reference notes across all folders.
* **Version Independent:** This repository exists separately from any code branches, acting as a permanent encyclopedia.
* **Auto-Deployment:** No manual HTML editing. Saving a `.md` file and pushing to GitHub updates the live site automatically.

---

### Pro-Tip: The "Quick Jump"

Since you are using this for personal reference, once this is set up, you can press the **`f`** key while on your live website to instantly focus the search bar—making it faster to find your notes than clicking through folders.