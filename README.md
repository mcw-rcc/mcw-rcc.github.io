# MCW Research Computing documentation

Using MkDocs and GitHub to host our documentation.

## Contributing

### Prerequisites

Setting up an IDE is optional but we recommend the following tools.

- [Visual Studio Code](https://code.visualstudio.com/)
- [GitHub Desktop](https://desktop.github.com/)
- [Git for Windows](https://gitforwindows.org/) (Mac/Linux use system Git)

### Obtain the code

You need your own copy of the code to get started. This does require a GitHub account and you should sign up for a free account if you don't already have one.

To obtain a copy of the code, fork the main repo by clicking the **Fork** button on the main repo page <https://github.com/mcw-rcc/mcw-rcc.github.io>. This will create a copy of the main repository under your user account.

Finally, clone (i.e. download) the forked repository to your local machine using GitHub Desktop.

### Setup local environment

- Clone the repo from `https://github.com/mcw-rcc/mcw-rcc.github.io`
- Create a new feature branch. **Do not make changes to the main branch.**
- Setup a Python virtual environment

    ```bash
    # MacOS/Linux terminal instructions
    python3 -m venv venv
    source venv/bin/activate
    python -m pip install -r requirements.txt
    ```

    or

    ```bash
    # Windows command prompt instructions
    python.exe -m venv venv
    venv\Scripts\activate.bat
    python.exe -m pip install -r requirements.txt
    ```

- Start local server

    ```bash
    # MacOS/Linux
    mkdocs serve
    ```

    or

    ```bash
    # Windows command prompt
    # Modify path to your venv
    python.exe -X utf8 D:\mcw-rcc.github.io\venv\Scripts\mkdocs.exe serve
    ```

- Open a browser and enter `http://localhost:8000`

### Adding a new page

To add a new page, edit the `mkdocs.yml` file and add a new entry to the `nav:` section.

For example, add a line to the software section:

```yaml
  - Software:
    - Using Modules: software/modules.md
    - Requesting Software: software/module-request.md
    - Guides:
      - software/R.md
      - software/python.md
      - software/tensorflow.md
      - software/pytorch.md
      - software/matlab.md
      - software/schrodinger.md
      - software/singularity.md
      - software/my-new-guide.md
```

This will create a new entry in the site navigation for a page called `my-new-guide.md`. If you're adding a page, make sure this page name is descriptive. For example, a page about GitHub might be called `github.md`.

The page title that shows in the side navbar, and at the top of the page, will be the first entry in the page.

For our Github page:

```md
# Learning GitHub
```

Remember to refresh your `http://localhost:8000` browser session to preview your changes.

### Style Guide

Work in progress.
