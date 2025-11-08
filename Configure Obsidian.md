## Dossiers

- Journaling: Daily notes folder
- \_Templates
	- Add Daily Notes file
- \_Attachments

## Configuration

- Editor
	- Show Line number
- Files and links
	- Delete files: Move to Obsidian trash
	- Default location for new notes: Same folder as current file
	- Default location for new attachments: in the folder specified below => \_Attachments
- Appearance
	- Dark Mode
	- Theme: Things
- Hotkeys
	- ==TBD==

## Core plugins

- Daily notes
	- New file location: Journaling
	- Template file location: \_Template/Daily Notes
- File Recovery
	- History length: 30
- Note composer
	- ==TBD==
- Templates
	- Template folder location: \_Templates

## Community plugins

- Calendar
- Outliner
- GitHub Sync
- JupyMD

### Git Setup

```bash
git init
git add .
git commit -m "First commit."
git remote add origin https://github.com/payns75/Learning.git
git branch -M main
git push -u origin main
```

#### Adding existing repository

```bash
git clone https://github.com/payns75/Learning.git
```

#### Configure plugin

Remote URL => https://github.com/payns75/Learning.git

Then sync with GitHub repository by clicking on the git icon on the left side of obsidian.

### JupyMD Setup

#### Prerequisites

##### Python

##### Jupyter Notebook

``` bash
	  python -m install notebook
	  ```

##### Jupytext

``` bash
	  python -m install jupytext
	  ```

##### Matplotlib

``` bash
	  python -m install matplotlib
	  ```
	  