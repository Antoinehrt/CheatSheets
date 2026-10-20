---
date: 2026-10-20
tags:
  - Obsidian
  - MarkDown
  - Syntax
description: "PKM description: Every Markdown and Obsidian-specific snippet in one place - formatting, internal links, embeds, callouts, tables, code, maths, diagrams, comments, properties and tags"
---
## 📋 **Table of Contents**

- [1. Text formatting](#1-text-formatting)
- [2. Headings, paragraphs and lists](#2-headings-paragraphs-and-lists)
- [3. Internal links](#3-internal-links)
- [4. Embeds](#4-embeds)
- [5. External links and images](#5-external-links-and-images)
- [6. Callouts](#6-callouts)
- [7. Quotes, commens and separators](#7-quotes-comments-and-separators)
- [8. Code](#8-code)
- [9. Tables](#9-tables)
- [10. Footnotes](#10-footnotes)
- [11. Properties (frontmatter)](#13-properties-frontmatter)
- [12. Tags](#14-tags)
---

## 1. Text formatting

|Effect|Syntax|Result|
|---|---|---|
|Bold|`**text**` or `__text__`|**text**|
|Italic|`*text*` or `_text_`|_text_|
|Bold + italic|`***text***`|_**text**_|
|Strikethrough|`~~text~~`|~~text~~|
|Highlight|`==text==`|==text==|
|Inline code|`` `code` ``|`code`|
|Subscript|`H<sub>2</sub>O`|H<sub>2</sub>O|
|Superscript|`x<sup>2</sup>`|x<sup>2</sup>|
|Line break|two spaces at the end of a line, or `<br>`||

> 💡 Highlight (`==`) is **Obsidian only**. It will not render on GitHub.

---

## 2. Headings, paragraphs and lists

### Headings

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

### Lists

```markdown
- Unordered item
- Another item
    - Nested item (Tab to indent)

1. Ordered item
2. Another item
    1. Nested item

- [ ] Task to do
- [x] Task done
```

> Tasks: `Ctrl+Enter` toggles the checkbox on the current line.

---

## 3. Internal links

```markdown
[[Note name]]                      Link to a note
[[Note name|Displayed text]]       Link with an alias
[[Note name#Heading]]              Link to a heading
[[Note name#^block-id]]            Link to a block
[[#Heading]]                       Link to a heading in the same note
[[#^block-id]]                     Link to a block in the same note
[[Folder/Note name]]               Link with a path (when two notes share a name)
[[Note name#Heading|Text]]         Heading link with an alias
```

### Block ID

Add `^id` at the end of a paragraph to make it linkable:

```markdown
This paragraph can be linked from anywhere. ^my-block
```

For a list, table or quote, put the ID on its own line **after** the block, with a blank line before it:

```markdown
> A quote to reference

^quote-id
```

### Search the vault while typing a link

|Typed|Searches|
|---|---|
|`[[`|Notes|
|`[[#`|Headings of the current note|
|`[[##`|Headings of the whole vault|
|`[[^`|Blocks of the current note|
|`[[^^`|Blocks of the whole vault|

> ⚠️ In a **table**, escape the pipe of an alias: `[[Note\|Alias]]`.

### Markdown-style internal link

```markdown
[Displayed text](Note%20name.md)
[Displayed text](<Note name.md>)
[Displayed text](Note%20name.md#Heading)
```

Spaces must be encoded as `%20`, or the whole path wrapped in `< >`.

---

## 4. Embeds

Add `!` before a link to display the content instead of a link.

```markdown
![[Note name]]                  Whole note
![[Note name#Heading]]          One section
![[Note name#^block-id]]        One block
![[image.png]]                  Image
![[image.png|300]]              Image, width 300 px
![[image.png|300x200]]          Image, width x height
![[document.pdf]]               PDF
![[document.pdf#page=3]]        PDF at page 3
![[audio.mp3]]                  Audio
![[video.mp4]]                  Video
![[Obsidian_DB.base]]           A Base (database view)
![[Canvas_name.canvas]]         A Canvas
```

Image with alt text and size:

```markdown
![[image.png|Alt text|300]]
![Alt text|300](image.png)
```

Embedded search result:

````markdown
```query
tag:#Linux path:"OS"
```
````

---

## 5. External links and images

```markdown
[Displayed text](https://example.com)
[Displayed text](https://example.com "Tooltip title")
<https://example.com>
![Alt text](https://example.com/image.png)
![Alt text|300](https://example.com/image.png)
```

Reference-style:

```markdown
[Displayed text][ref]

[ref]: https://example.com
```

---

## 6. Callouts

### Syntax

```markdown
> [!note]
> Content of the callout.

> [!tip] Custom title
> Content with a custom title.

> [!warning]- Collapsed by default
> Click the title to open.

> [!info]+ Expanded by default, but foldable
> Click the title to close.
```

|Suffix|Behaviour|
|---|---|
|none|Always open, not foldable|
|`-`|Foldable, **closed** by default|
|`+`|Foldable, **open** by default|

### Available types

|Type|Aliases|Use for|
|---|---|---|
|`note`||General note|
|`abstract`|`summary`, `tldr`|Summary|
|`info`||Information|
|`todo`||Action to do|
|`tip`|`hint`, `important`|Advice|
|`success`|`check`, `done`|Positive result|
|`question`|`help`, `faq`|Question|
|`warning`|`caution`, `attention`|Warning|
|`failure`|`fail`, `missing`|Failure|
|`danger`|`error`|Critical danger|
|`bug`||Known bug|
|`example`||Example|
|`quote`|`cite`|Quotation|

### Examples

```markdown
> [!warning] Never use `sudo` with `ssh-add`
> The key would be added to the root agent, not yours.

> [!tip] Rule of thumb
> If a second observable depends on the first, use `switchMap`.

> [!bug] Known issue
> The language switcher does not work in `ng serve`.

> [!example]- Full example
> ```bash
> ng build --configuration=production
> ```
```

### Nested callouts

```markdown
> [!question] Can callouts be nested?
> > [!success] Yes
> > They can be nested at several levels.
```

### Custom callout (CSS snippet)

```css
.callout[data-callout="custom"] {
  --callout-color: 255, 140, 0;
  --callout-icon: lucide-flame;
}
```

Then use it with `> [!custom] Title`.

---

## 7. Quotes, comments and separators

```markdown
> A simple quote
> on several lines

> Nested
>> quote

---
***
___
```

Three ways to write a horizontal rule: `---`, `***` or `___`.

### Comments (hidden in reading view)

```markdown
%% Inline comment, invisible in reading view %%

%%
Multi-line comment.
Also invisible in reading view.
%%
```

```markdown
<!-- Standard HTML comment, also hidden -->
```

> 💡 `%% %%` is **Obsidian only**. Use `<!-- -->` if the note is also read on GitHub.

---

## 8. Code

### Inline

```markdown
Use `git status` to check the repository.
```

### Block

````markdown
```bash
git status
```
````

Common language identifiers: `bash`, `shell`, `powershell`, `java`, `ts`, `js`, `json`, `yaml`, `html`, `css`, `scss`, `sql`, `postgresql`, `python`, `csharp`, `dockerfile`, `properties`, `nginx`, `xml`, `markdown`, `diff`, `text`.

### Show a code block inside a code block

Use **four** backticks for the outer fence:

`````markdown
````markdown
```bash
echo "hello"
```
````
`````

### Alternative fences

```markdown
~~~bash
echo "tildes work too"
~~~
```

---

## 9. Tables

```markdown
| Left   | Centre | Right |
| :----- | :----: | ----: |
| text   |  text  |  text |
| a      |   b    |     c |
```

|Left|Centre|Right|
|:--|:-:|--:|
|text|text|text|

- `:---` left aligned, `:---:` centred, `---:` right aligned.
- Escape a pipe inside a cell: `\|`.
- Line break inside a cell: `<br>`.
- Inline code containing a pipe still needs `\|`.
- Tip: `Ctrl+P` → _Insert table_, or the Advanced Tables community plugin (`Tab` jumps from cell to cell).

---

## 10. Footnotes

```markdown
This sentence has a footnote.[^1]

Inline footnote.^[This text is the footnote itself.]

[^1]: The footnote content, displayed at the bottom of the note.
```

---

## 11. Properties (frontmatter)

Properties live at the very top of the note, between `---` lines, in **YAML**.

```yaml
---
date: 2026-10-20
tags:
  - Obsidian
  - Markdown
description: A short summary of the note
aliases:
  - Obsidian syntax
  - MD cheat sheet
cssclasses:
  - wide-page
Parent: "[[Angular]]"
---
```

### Property types

| Type          | Example                                         |
| ------------- | ----------------------------------------------- |
| Text          | `description: Some text`                        |
| List          | `tags:` then `- item` lines                     |
| Number        | `priority: 3`                                   |
| Checkbox      | `done: true`                                    |
| Date          | `date: 2026-10-20`                              |
| Date and time | `due: 2026-10-20T14:30:00`                      |
| Link          | `Parent: "[[Note name]]"` (**quotes required**) |

### Reserved properties

| Property     | Role                                        |
| ------------ | ------------------------------------------- |
| `tags`       | Tags of the note                            |
| `aliases`    | Alternative names, used by link suggestions |
| `cssclasses` | CSS classes applied to the note             |

> ⚠️ A link in a property must be wrapped in quotes: `"[[Note]]"`. Without them the YAML is invalid. ⚠️ Values containing `:` or starting with `#`, `[`, `{`, `*`, `&` must also be quoted.

---

## 12. Tags

```markdown
#tag
#parent/child                 Nested tag
#Multi_Word_Tag               No spaces: use _ or - or camelCase
```

- A tag needs at least one non-numeric character (`#2026` is **not** a tag, `#y2026` is).
- Allowed: letters, numbers, `_`, `-`, `/`.
- In frontmatter, write tags **without** `#`:

```yaml
tags:
  - Linux
  - Docker
```

- Search: `tag:#Linux` in the search bar.

---

## 📚 Ressources

- [Obsidian Help: Basic formatting syntax](https://help.obsidian.md/syntax)
- [Obsidian Help: Advanced formatting syntax](https://help.obsidian.md/advanced-syntax)
- [Obsidian Help: Callouts](https://help.obsidian.md/callouts)
- [Obsidian Help: Properties](https://help.obsidian.md/properties)
- [Obsidian Help: Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown)
- [Markdown Guide](https://www.markdownguide.org/)
- [[GIT]] - Related: README and `.gitignore` for the vault