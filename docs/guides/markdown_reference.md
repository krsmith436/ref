# Markdown Reference for krsmith436
|  [Markdown Basics](https://www.markdownguide.org/basic-syntax)  |  [Python-Markdown 3.10](https://python-markdown.github.io)  |

## Emojis

🕒 💻 🧭 💡 → 🖥 ⭐ 

## Adding "Admonitions"
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

Python-Markdown 3.10.2 suggests the following [Admonition types](https://python-markdown.github.io/extensions/admonition/); however, you’re free to use whatever you want:

!!! hint ""
    `attention`, `caution`, `danger`, `error`, `hint`, `important`, `note`, `tip`, and `warning`

