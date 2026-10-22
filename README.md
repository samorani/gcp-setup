# gcp-setup

One script that takes an app from your laptop to a live website on Google Cloud.

## Why this exists

You could set up a Google Cloud project by typing a dozen `gcloud` commands by hand:
create the project, link billing, turn on the right APIs, grant permissions, deploy.
It works, but every person does it slightly differently, mistakes are easy, and nobody
can tell what was done.

Organizations don't work that way. They write the steps down **once**, as a script,
keep it in a shared repository, and everyone runs the same script. When it breaks,
someone fixes the script, and the fix helps everyone from then on.

That is what [gcp_setup.sh](gcp_setup.sh) is. **The goal of this repo is a script that
works for everyone in the class.** You are its users and its maintainers.

The Flask app in this repo (`app.py`, `templates/`) is only a test app, there so the
script has something to deploy. You don't need to change it.

The script is not tied to Flask. It deploys any folder that has a `requirements.txt`
(Python), a `package.json` (Node.js) or a `Dockerfile`. Google detects the language and
builds the app.

## How to use it

In the commands below, replace the placeholders written in CAPITALS (shown in
$\color{red}{\textsf{red}}$ under each command) with your own values.

You need a terminal: the Terminal app on Mac, or **Git Bash** on Windows.

1. **Install the Google Cloud CLI (`gcloud`).** Follow
   [Google's install guide](https://cloud.google.com/sdk/docs/install), or ask Claude Code
   to install it on your laptop for you. Then check that it works:
   ```bash
   gcloud --version
   ```
   If this prints version numbers, you're set. If it says `command not found`, close
   the terminal, open a new one and try again.

2. **Log in** (only needed once):
   ```bash
   gcloud auth login
   ```

3. **Find your billing account id:**
   ```bash
   gcloud billing accounts list
   ```
   It's the `ACCOUNT_ID` column and looks like `0A1B2C-3D4E5F-6A7B8C`.

4. **Run the script** from this folder. Pick a new project id: lowercase letters,
   digits and hyphens. It must be unique across all of Google Cloud, so add some numbers.
   ```bash
   bash gcp_setup.sh --project YOUR-PROJECT-ID --billing YOUR-BILLING-ID
   ```
   Replace: $\color{red}{\textsf{YOUR-PROJECT-ID}}$ (e.g. `my-app-4821`) and
   $\color{red}{\textsf{YOUR-BILLING-ID}}$ (from step 3).

At the end it prints your website's URL.

You can run it again at any time. It checks each step, skips anything already done, and
redeploys. To just redeploy after changing code:
```bash
bash gcp_setup.sh --deploy-only
```

Run `bash gcp_setup.sh --help` to see all the options.

## If something doesn't work

Don't work around it by typing `gcloud` commands by hand. That fixes it for you, but not
for anyone else. Do one of these instead:

### Option A: report it (open an issue)

GitHub has a built-in issue tracker for each repository.

1. Go to the repo's **Issues** tab: https://github.com/samorani/gcp-setup/issues
2. Click **New issue**.
3. Include:
   - what you ran (the exact command)
   - the full error message (copy and paste it; please don't send a screenshot)
   - your system: Mac or Windows, and the output of `gcloud --version`

First check whether someone has already opened an issue for the same problem. If they
have, add a comment to it instead.

### Option B: fix it (open a pull request)

If you can figure out the fix, propose it. Never commit straight to `main`. Work on your
own branch and open a *pull request*, so the change gets reviewed before it's merged.

1. Clone the repo and create a branch for the fix:
   ```bash
   git clone https://github.com/samorani/gcp-setup
   cd gcp-setup
   git checkout -b BRANCH-NAME
   ```
   Replace: $\color{red}{\textsf{BRANCH-NAME}}$ with a short description, e.g. `fix-billing-retry`.

   Already cloned? Start from the latest version instead:
   `git checkout main`, `git pull`, then `git checkout -b BRANCH-NAME`.

2. Edit `gcp_setup.sh`, then run it again to test your change.

3. Commit and push the branch:
   ```bash
   git add gcp_setup.sh
   git commit -m "WHAT-YOU-FIXED"
   git push -u origin BRANCH-NAME
   ```
   Replace: $\color{red}{\textsf{WHAT-YOU-FIXED}}$ with one line, e.g.
   `Retry billing link longer for new projects`, and $\color{red}{\textsf{BRANCH-NAME}}$
   with the same name as in step 1.

4. On GitHub, click **Compare & pull request**, and explain what was broken and how you
   fixed it. If there is an issue for it, write `Fixes #12` (with that issue's number)
   so the issue closes when the fix is merged.

Keep each fix small and focused on a single problem.
