# Data Store Days - Group 2
A repository containing the documentation of the Group 2 for the Data Store Days organized in BAM on April, 9 to 11, 2025.



### Launching the documentation locally

Create a virtual environment:
```sh
python3 -m venv .venv
. .venv/bin/activate
pip install --upgrade pip
```

Install the requirements:
```sh
pip install uv
uv pip install -r requirements.txt
```

Build and launch the MkDocs page:
```sh
mkdocs build
mkdocs serve
```

Click on the local address to work on the documentation page.
