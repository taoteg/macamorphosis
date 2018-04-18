# Macamorphosis Branching Strategy

### Project Branches

- `master`
- `gh-pages`
- `dev`
- `content-backup`
- `theme-hacker`
- `theme-millenial`
- `remotes/origin/gh-pages`
- `remotes/origin/master`
- `remotes/origin/theme-hacker`
- `remotes/origin/theme-millenial`


### Branch Descriptions

- `master` is functioning as the integration branch. It is a place to manipulate the other branches from these workflows.
- `gh-pages` is the published content that shows up on the live webpage. It functions as the production branch.
- `dev` is the WIP content that has not yet been published live. It is where the work is done.
- `theme-*` are all branches used to track themes, mitigate theme switching and to prevent loss of old content during theme transitions.

_NOTE: The theme-* branches vary wildly in structure and content because the underlying themes do. If a theme change is desired, content must be migrated from the old theme in branch theme-X to the new theme under branch theme-Y. Once the new theme has all the contnt in it, the usual branching workflow is as defined below._


## Workflows

There are several branching workflows to be aware of:

```
DEV -> CONTENT-BACKUP         # Backing up current content.
MASTER -> THEME-THEMENAME     # Creating a new theme branch.
THEME-THEMENAME -> DEV        # Using the new theme in development.
DEV -> MASTER                 # Staging the content for publication.
MASTER -> GH-PAGES            # Publishing content on live site.
```

### Theme Transition Workflow

1. Make sure all current content is pushed into the `content-backup` branch before doing anything:
    ```
    $ git checkout content-backup
    $ git status
    ````
2. If anything is not tracked, be sure and get it added in before you do the next steps or you could lose it:

    ```
    $ git add .
    $ git commit -m "Content backup"
    $ git push
    ```


### Theme Transition Workflow

1. Checkout the master branch:
    ```
    $ git checkout master
    ```
2. Create a new branch for the new theme:
    ```
    $ git checkout -b theme-THEMENAME
    ```
3. Copy the new theme codebase into the macamorphosis project root folder, replacing all previous content.
    - **WARNING: DO NOT DELETE THE .git folder!!**
4. Commit the new code into the new theme branch:
    ```
    $ git add .
    $ git commit -m "New theme added - theme-THEMENAME."
    ```
5. Create a remote copy of the theme branch to push code upstream:
    ```
    $ git push -u origin theme-THEMENAME
    ```
6. Checkout the `dev` branch:
    ```
    $ git checkout dev
    ```
7. Merge the new theme code into the dev branch:
    ```
    $ git merge theme_THEMENAME
    ```
    - **WARNING: You will overwrite the content in this branch, so be sure you completed the content backup workflow first.**
8. Commit the new code into the upstream repo:
    ```
    $ git add .
    $ git commit -m "Updated site content..."
    $ git push
    ```


### Dev Workflow

1. Work on the `dev` branch content until ready to publish.
2. Commit the updates into the upstream repo:
    ```
    $ git add .
    $ git commit -m "Updated site content..."
    $ git push
    ```
3. You can replicate these changes into your `content-backup` branch as you feel fit, but it is not _required_ unless a theme change is happening.


### Publication Workflow

1. Propogate the `dev` changes into `master`:
    ```
    $ git checkout master
    $ git merge dev
    $ git push
    ```
2. Checkout the `gh-pages` branch and merge in the new content:
    ```
    $ git checkout gh-pages
    $ git merge master
    ```
3. Resolve any conflicts and push code into repo:
    ```
    $ git push
    ```

Be sure and return to your `dev` branch when you want to work on the site content.
