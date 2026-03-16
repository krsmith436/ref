|  🔗 [Markdown Basics](https://www.markdownguide.org/basic-syntax)  |  🔗 [Python-Markdown 3.10](https://python-markdown.github.io)  |

## Emojis

🔗 📂 🕒 💻 🧭 💡 → 🖥 ⭐ ⚙️ 

## Adding "Admonitions" with MkDocs
You can highlight "Notes" or "Warnings." Material for MkDocs has a great feature for this.

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
#### MkDocs Admonition Types

!!! hint ""
	| Type | Aliases | Typical Color |
	| --- | --- | --- |
	| **`note`** | — | Blue |
	| **`abstract`** | `summary`, `tldr` | Light Blue |
	| **`info`** | `todo` | Cyan |
	| **`tip`** | `hint`, `important` | Teal |
	| **`success`** | `check`, `done` | Green |
	| **`question`** | `help`, `faq` | Indigo |
	| **`warning`** | `caution`, `attention` | Orange |
	| **`failure`** | `fail`, `missing` | Red |
	| **`danger`** | `error` | Dark Red |
	| **`bug`** | — | Pink |
	| **`example`** | — | Purple |
	| **`quote`** | `cite` | Grey |

