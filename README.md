# The Triad Lab Website (Memory | Methods | Mind)

Official website for **The Triad Lab** led by **Dr. Paul Riesthuis** at KU Leuven (Faculty of Law & Criminology).

Aesthetic theme inspired by *The Legend of Zelda: Ocarina of Time* (Sacred Triforce Gold, Kokiri Emerald, Temple of Time Sapphire, and Hyrule Parchment).

---

## 🏛️ Website Structure

This website is built with **[Quarto](https://quarto.org)** and **[R](https://www.r-project.org)**, configured to deploy seamlessly via **GitHub Pages**.

```
RiesthuisWebsite/
├── _quarto.yml                     # Main site configuration (navbar, footer, search, render list)
├── custom.scss                     # Zelda Ocarina of Time styling (Triforce gold, emerald, sapphire)
├── index.qmd                       # Homepage (Hero with logo, Three Pillars, Featured Research, Bio)
├── research/
│   └── index.qmd                   # Research Lines (Memory & Deception, SESOI Methods, Mind & Law)
├── publications/
│   └── index.qmd                   # Full catalog of journal articles, registered reports & preprints
├── team/
│   └── index.qmd                   # Dr. Paul Riesthuis profile, education, collaborations & opportunities
├── posts/
│   ├── index.qmd                   # R Analyses & Tutorials listing page
│   ├── 2afc/                       # 2AFC memory analysis tutorial
│   ├── irt_2pl/                    # Item Response Theory (2PL) tutorial
│   ├── irt_rasch/                  # Item Response Theory (Rasch) tutorial
│   ├── old_new-memory-task/        # Old/New recognition memory task
│   ├── roc_curve/                  # ROC curve analysis tutorial
│   ├── rocpower/                   # ROC power analysis Shiny app
│   ├── safetesting/                # Safe testing & sequential inference in R
│   ├── sesoi-power/                # AMPPS SESOI power simulation walkthrough
│   └── ...                         # Additional reproducible posts
├── resources/
│   ├── index.qmd                   # Open Science repositories, OSF data, workshop materials
│   └── talks/                      # Downloadable presentation slides & PDFs
├── contact/
│   └── index.qmd                   # KU Leuven address, email, social & academic profiles
├── images/
│   ├── logo.jpg                    # The Triad Lab Triforce emblem
│   └── paul_riesthuis.jpg          # Lab Director portrait
├── docs/                           # Pre-rendered static website for GitHub Pages
├── .nojekyll                       # Ensures GitHub Pages serves all Quarto assets correctly
└── .github/workflows/publish.yml   # Automated GitHub Actions workflow
```

---

## 🛠️ How to Edit & Make Changes with R & Quarto

All pages are written in `.qmd` (Quarto Markdown) files. You can open and edit any `.qmd` file in **RStudio**, **VS Code**, or any text editor.

### 1. Editing Existing Pages
- Open `index.qmd`, `research/index.qmd`, `publications/index.qmd`, `team/index.qmd`, `resources/index.qmd`, or `contact/index.qmd`.
- Edit the text, update links, or add executable R chunks ````{r} ... ````.
- Save the file.

### 2. Adding a New R Tutorial / Blog Post
1. Create a new folder inside `posts/`, e.g. `posts/bayesian-memory/`.
2. Inside that folder, create an `index.qmd` file with YAML metadata at the top:
   ```yaml
   ---
   title: "My New R Analysis Title"
   author: "Dr. Paul Riesthuis"
   date: "2026-10-08"
   description: "A short summary of what this R tutorial covers."
   image: "featured.png"  # or "../../images/logo.jpg"
   categories:
     - "Memory"
     - "R Tutorial"
   toc: true
   ---

   # Your Content Here

   ```{r}
   # Executable R code
   x <- rnorm(100)
   summary(x)
   ```
   ```
3. Save the file. It will automatically show up in the **R Analyses & Posts** listing page!

---

## 🚀 Local Preview & Rendering

In your RStudio terminal or PowerShell:

### Live Preview (updates as you edit):
```bash
quarto preview
```

### Full Build (renders output to `docs/`):
```bash
quarto render
```

---

## 🌐 Deploying to GitHub Pages

The project is configured with `output-dir: docs` and includes `.nojekyll`.

### Option A: Direct GitHub Pages from `/docs` (Recommended)
1. Push your repository to GitHub:
   ```bash
   git add .
   git commit -m "Update website content"
   git push origin main
   ```
2. On GitHub, go to **Settings** &rarr; **Pages**.
3. Under **Build and deployment**:
   - **Source:** Deploy from a branch
   - **Branch:** `main` (or `master`) / folder: `/docs`
   - Click **Save**.
4. Your website will be live at: `https://paulriesthuis.github.io/RiesthuisWebsite/`

### Option B: GitHub Actions CI/CD
A pre-configured GitHub Actions workflow is included at `.github/workflows/publish.yml`. If you switch GitHub Pages source to **GitHub Actions**, every push will automatically render and publish the site.

---

## 🛡️ Theme & Design Tokens

- **Triforce Gold:** `#c59b27` / `#f3ca40` (Accents, hero badges, borders, CTA buttons)
- **Kokiri Forest Emerald:** `#145a46` / `#2eb88a` (Primary navigation, Pillar I, highlights)
- **Temple of Time Sapphire:** `#0d233a` / `#2b7bc4` (Headers, backgrounds, callouts)
- **Shadow Purple & Goron Ruby:** `#5e2781` / `#9b2226` (Pillar III, justice & alert elements)
- **Hyrule Parchment:** `#faf7f0` (Body background, clean paper contrast)
- **Typography:** *Cinzel* (Headings & Heraldry) + *Plus Jakarta Sans* (Readable body) + *JetBrains Mono* (R code blocks)