# Nextome Documentation docs.nextome.dev

## How to DEPLOY
 * Simply push commits to `prod` branch. A Github Action will deploy the website automatically.


Tools used: 

- MkDocs: https://www.mkdocs.org/
- Material for MkDocs: https://squidfunk.github.io/mkdocs-material/getting-started/

## Installation: 

```
pip install mkdocs-material
```

To preview the site locally: 

```
mkdocs serve
```

## Deploy not versioned: 
To deploy the doc for the v2 without versioning 

```
mkdocs gh-deploy --force
```

## Deployed versioned: 
There are two branches, v0 (for the oldest docs) and main (for the 2.0.0).
To update a spacific version: 

1) Checkout to the branch
2) Deploy use 2.0.0 or 1.0.0
```
mike deploy --push --update-aliases 2.0.0
```
