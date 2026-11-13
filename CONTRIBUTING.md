# Contributing

Short version: one branch per change, one page per file, commit messages that explain
*why*, and a pull request that says how you checked it.

## Run it locally

There is nothing to install. Open `index.html` in a browser, or serve the folder if you
want the same origin behaviour as the live site:

```sh
python -m http.server 8000
# then open http://localhost:8000
```

Test in a narrow window as well as a wide one — the layout switches at 820px.

## Before you start

Open an issue for anything larger than a typo. It costs a paragraph and saves you from
building a feature that does not fit. Small fixes can go straight to a pull request.

## Branch names

Format: `type/short-slug`

| Type | Use for | Example |
| --- | --- | --- |
| `feat/` | a new page, or new user-visible behaviour | `feat/links-page` |
| `fix/` | repairs something that is wrong | `fix/space-repeat-hold` |
| `chore/` | markup, styling, tooling, housekeeping | `chore/site-nav` |
| `docs/` | prose only, no behaviour change | `docs/contributing` |

Rules: lowercase, hyphens between words, three to five words, no names, dates or ticket
numbers (`feat/links-page`, not `Kshithij_linksPage_13Nov`). The slug should describe the
change so that `git branch` reads like a to-do list.

## Commit messages

```
type(scope): imperative summary, 72 characters at most

Why this change exists and what it touches. A reviewer should not have
to guess the problem being solved. Wrap at 72 columns.
```

- **type** — `feat`, `fix`, `chore`, `docs`, `refactor`, `test`.
- **scope** — the page or area, in one word: `timer`, `nav`, `links`, `contact`, `guide`.
- **summary** — imperative mood. "add links page", not "added" or "adds".
- **body** — the *reason*. The diff already shows what changed.
- **footer** — `Closes #12` to close an issue, `Refs #12` to link without closing.

One logical change per commit. If a commit message needs the word "also", it is probably
two commits.

## Files and folders

The structure is flat on purpose:

```
index.html        the timer — the only page with JavaScript
guide.html        how the timer behaves
links.html        project links
contact.html      where to report things
README.md         what the project is
CONTRIBUTING.md   this file
```

- No build step, no `package.json`, no bundler, no dependencies. The browser runs what is
  in the repository.
- Each page repeats its own inline CSS and header. That duplication is deliberate: pages
  stay independent, so two people can work on two pages without fighting over one
  stylesheet.
- If a shared file ever becomes unavoidable, add it as `assets/…` and update every page in
  the same pull request, so no page is left half-styled.
- Images go in `img/`.

## Breaking changes

`main` is what GitHub Pages publishes. A broken merge to `main` is a broken public site,
so keep every merge to `main` in a state you have opened in a browser.

The solve history lives under the `hold-timer.solves.v1` key. If the stored shape ever
changes, bump the version in that key and migrate the old data — do not silently drop
people's times.

## Pull requests

- **Title** mirrors the commit summary.
- **Body** says what changed, why, and how you checked it (which browser, wide and narrow).
- **Keep it focused.** Unrelated reformatting makes the real change invisible.
- **Say what you did not do**, if anything is left for later.
- There are no automated checks here, so a human opens the page before merging.

## Merging

The merge method shapes the history, so pick it deliberately:

| Method | Use it when | History |
| --- | --- | --- |
| **Squash and merge** | the branch has work-in-progress commits ("fix typo", "try again") | one clean commit on `main` |
| **Create a merge commit** (`--no-ff`) | each commit in the branch is meaningful, and the PR boundary is worth keeping | branch preserved, merge commit added |
| **Rebase and merge** | a short, clean branch, and a linear history is wanted | commits replayed, no merge commit |
| **Fast-forward** | local reshuffling only, or a branch that is genuinely up to date | no trace that a branch existed |

Rebase and force-push only your own branch. Never rewrite a branch someone else has
pulled from.

### Do not merge when

- you could not open the page and use it yourself;
- a review comment is still open, or the author has not answered it;
- the pull request mixes unrelated changes — ask for a split instead of merging and hoping;
- it changes shared behaviour with no linked issue to explain the decision;
- a conflict was resolved by deleting the other side's work;
- the author force-pushed over the review, hiding what changed — ask why before continuing.

Declining is part of the job. Say why, link to the guideline, and leave the door open: a
contributor who gets a reason usually comes back with a better patch, and one who gets
silence does not come back at all.

## Review checklist

1. Does it do exactly one thing?
2. Does it work in a browser — mouse, touch, and keyboard?
3. Are existing solves still there after the change?
4. Does every `href` point at a file that exists?
5. Does `git log --oneline` read clearly afterwards?
