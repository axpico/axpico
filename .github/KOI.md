# Koi: how it works and how to restart it

The koi on the profile README is drawn by [git-koi](https://github.com/yalongwastaken/git-koi)
through the workflow `.github/workflows/koi.yml`. The workflow writes `koi/koi.gif`,
and the README shows that file.

## When it runs

- **Every night at 00:20 UTC** (01:20 Italian time in winter, 02:20 in summer). It draws the day that just ended.
- **Whenever `koi.yml` is changed** and pushed.
- **By hand**, whenever you want (see below).

It makes a commit (`koi: draw the day's koi`, by `github-actions[bot]`) only when the GIF changed.
No commit on a given day is normal, not an error.

## Run it by hand

On GitHub: **Actions** tab → **Koi** in the left list → **Run workflow** → branch `main` → **Run workflow**.

With the GitHub CLI:

```sh
gh workflow run koi.yml --repo axpico/axpico
gh run watch --repo axpico/axpico          # follow the run
```

## Restart it after GitHub pauses it

In a public repo, GitHub **disables scheduled workflows after 60 days with no activity**.
When the koi doesn't change it makes no commits, so this can happen by itself.
You'll get an email from GitHub, and the Actions tab will show a yellow banner on **Koi**.

To turn it back on:

- On GitHub: **Actions** → **Koi** → **Enable workflow** (the button in the banner).
  Then run it once by hand so the image is up to date.
- With the CLI:

  ```sh
  gh workflow enable koi.yml --repo axpico/axpico
  gh workflow run koi.yml --repo axpico/axpico
  ```

Any commit to the repo also resets the 60-day counter.

## The colours

- **The colours come from your languages.** git-koi reads your public repositories, and the
  public repos you contributed to, and paints the koi in GitHub's colour for each language.
  Private repos don't count.
- **The base colour** is `body:` in `koi.yml`: `white`, `black`, `asagi`, `tea`, `gold` or `platinum`.
  Change it and push, and the workflow redraws the koi at once.
- Contributions make it grow and streaks add marks. The previous day's activity sets how it swims.

## If something breaks

| Symptom | Likely cause | Fix |
|---|---|---|
| Broken image in the README | The workflow never succeeded, so `koi/koi.gif` doesn't exist | Run it by hand, then open the run log |
| The koi doesn't update | The workflow is paused (60 days) or failing | Look at the Actions tab: enable it or read the error |
| Run fails at "Download the git-koi release" | A git-koi release is missing an asset | Pin a full tag in `koi.yml`, e.g. `yalongwastaken/git-koi@v1.0.2` |
| Run fails at the push step | The workflow can't write | Settings → Actions → General → Workflow permissions → "Read and write" |
| The koi section is gone from the README | The README was regenerated from the CV | Add the "My koi" section to the CV template too |

Run logs: https://github.com/axpico/axpico/actions/workflows/koi.yml

## Remove it

Delete `.github/workflows/koi.yml`, the `koi/` folder, and the "My koi" section in `README.md`.
