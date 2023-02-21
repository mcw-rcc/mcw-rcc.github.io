# MCW Research Computing documentation

Using mkdocs and github to host our documentation.

## Contributing

### Setup local web server

- Clone the repo from `https://github.com/mcw-rcc/mcw-rcc.github.io`
- Create a new feature branch. **Do not make changes to the main branch.**
- Setup a Python virtual environment

    ```bash
    python3 -m install virtualenv
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    ```

- Deploy locally

    ```bash
    mkdocs serve
    ```

- Open a browser and enter `https://localhost:8000`

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

Remember to refresh your `https://localhost:8000` browser session to preview your changes.

### Style Guide

Work in progress.
