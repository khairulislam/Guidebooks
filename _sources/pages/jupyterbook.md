# [Jupyter Book](https://jupyterbook.org/en/stable/start/overview.html)

Install `jupyter-book` using the following instructions. In general the instructions are for `pip`
```bash
pip install -U jupyter-book
```
for via `conda-forge`
```bash
conda install -c conda-forge jupyter-book
```

Temporariliy for `version 0.15.1`, `docutils==0.17.1` and `sphinx==5` is needed because of `docutils` 
[issue](https://github.com/mcmtroffaes/sphinxcontrib-bibtex/issues/322) with bibliography. 
The [requirements.txt](../requirements.txt) file has been updated with the working versions.

## [Creating a template book](https://jupyterbook.org/en/stable/start/create.html)

```bash
jupyter-book create book/
```

It'll create a template book with the following structure: 
```
$ tree mybookname
mybookname/
├── _config.yml
├── _toc.yml
├── intro.md
├── logo.png
├── markdown-notebooks.md
├── markdown.md
├── notebooks.ipynb
├── references.bib
└── requirements.txt
```

Make sure to update the configuration (`_config.yml`) and table of contents (`_toc.yml`) files. 

## [Build your book](https://jupyterbook.org/en/stable/start/build.html)

Inside the `book` folder run the following command 

```bash
jupyter-book build .

# alternatively use the short form 
jb build .
```

By default, Jupyter Book will only build the HTML for pages that have been updated since the last time you built the book. If you’d like to force Jupyter Book to re-build a particular page, you can either edit the corresponding file in your book’s folder, or delete that page’s HTML in the `_build/html` folder. Or rebuild using the `--all` option. 


```bash
jupyter-book build --all .
```

If it still doesn't update your contents, delete the `build/html` folder. Then build the book. 

### Preview

To preview your book, you can open the generated HTML files in your browser. Either double-click the html file in your local folder, or enter the absolute path to the file in your browser navigation bar adding file:// at the beginning (e.g. `file://Users/my_path_to_book/_build/index.html`).

## [Add new content](https://jupyterbook.org/en/stable/start/new-file.html)

Add new pages in the [_toc.yml](../_toc.yml) file. The paths are relative to the base folder of the book.

## [Publish book online using GitHub Pages](https://jupyterbook.org/en/stable/start/publish.html)

Install `ghp-import` and from the `main` branch of the repository which contains the built book (`_build/html` folder) call `ghp-import`.

```bash
pip install ghp-import
ghp-import -n -p -f _build/html
```

This works by copying all of the contents of your built book (i.e., the `_build/html` folder) to a branch of your repository called `gh-pages`, and pushes it to GitHub. The `gh-pages` branch will be created and populated automatically for you by `ghp-import`.

