<!-- banner-v2.svg lives in this same repo (YADUNANDAN-SINGH/YADUNANDAN-SINGH), next to this README -->
<div align="center">

<img src="banner-v2.svg" width="100%" alt="Yadunandan Singh — no black boxes. rebuild first, import later."/>

<br/><br/>

### AI Engineer — fast data backends and local-first AI

I speed up data tooling upstream at [c2siorg/DataLoom](https://github.com/c2siorg/dataloom), and build AI that runs on the hardware people already own — a model only where it earns its place, plain code wherever the answer has to be right. · Jammu, India

[**Portfolio**](https://yadunandan-singh.pages.dev/) · [**Writing**](https://medium.com/@yadunandan-ai-dev) · [**LinkedIn**](https://www.linkedin.com/in/yadunandan-singh-ai-dev/) · [**Email**](mailto:yadunandansingh027@gmail.com)

<br/>

![Python](https://img.shields.io/badge/Python-1a1b27?style=flat-square&logo=python&logoColor=667eea)
![pandas](https://img.shields.io/badge/pandas-1a1b27?style=flat-square&logo=pandas&logoColor=667eea)
![FastAPI](https://img.shields.io/badge/FastAPI-1a1b27?style=flat-square&logo=fastapi&logoColor=667eea)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-1a1b27?style=flat-square&logo=postgresql&logoColor=667eea)
![TypeScript](https://img.shields.io/badge/TypeScript-1a1b27?style=flat-square&logo=typescript&logoColor=667eea)
![React](https://img.shields.io/badge/React-1a1b27?style=flat-square&logo=react&logoColor=667eea)
![Rust](https://img.shields.io/badge/Rust-1a1b27?style=flat-square&logo=rust&logoColor=667eea)
![Ollama](https://img.shields.io/badge/Ollama-1a1b27?style=flat-square&logo=ollama&logoColor=667eea)
![Docker](https://img.shields.io/badge/Docker-1a1b27?style=flat-square&logo=docker&logoColor=667eea)
![TensorFlow](https://img.shields.io/badge/TensorFlow-1a1b27?style=flat-square&logo=tensorflow&logoColor=667eea)
![Django](https://img.shields.io/badge/Django-1a1b27?style=flat-square&logo=django&logoColor=667eea)
![WebAssembly](https://img.shields.io/badge/WebAssembly-1a1b27?style=flat-square&logo=webassembly&logoColor=667eea)

<br/>

<sub><a href="https://github.com/search?q=author%3AYADUNANDAN-SINGH+org%3Ac2siorg+is%3Apr+is%3Amerged&type=pullrequests"><b>22</b> merged upstream PRs</a> &nbsp;·&nbsp; <b>20</b> of them from issues I filed &nbsp;·&nbsp; quality check on a 100 MB CSV: <b>155 s → 14.5 s</b> &nbsp;·&nbsp; 🥈 <b>2nd place</b>, CT BuildFest 2.0</sub>

</div>

<br/>

## Merged upstream

**[22 merged PRs](https://github.com/search?q=author%3AYADUNANDAN-SINGH+org%3Ac2siorg+is%3Apr+is%3Amerged&type=pullrequests)** in [c2siorg](https://github.com/c2siorg) since June 2026 — 21 in [DataLoom](https://github.com/c2siorg/dataloom), 1 in [TensorMap](https://github.com/c2siorg/tensormap) — and [25 issues filed](https://github.com/search?q=author%3AYADUNANDAN-SINGH+org%3Ac2siorg+is%3Aissue&type=issues); 20 of the DataLoom PRs started from my own issue. Mostly backend: pandas performance, scaling, CI.

| PR | Before | After |
|:--|:--|:--|
| [#536](https://github.com/c2siorg/dataloom/pull/536) uploads | Quality check, 612k-row 100 MB CSV: **155.1 s** | **14.5 s** |
| [#517](https://github.com/c2siorg/dataloom/pull/517) dtype inference | 1M-row text column: **0.481 s** | **0.007 s**, same results |
| [#540](https://github.com/c2siorg/dataloom/pull/540) background jobs | 180-step pipeline timed out at 30 s; server kept writing | `202` + job id at once, progress, cancel |
| [#563](https://github.com/c2siorg/dataloom/pull/563) benchmark CI | No guard against slowdowns | 11 benchmarks; PRs fail at >50% slower |
| [#526](https://github.com/c2siorg/dataloom/pull/526) undo / redo | Undo replayed the whole log (3 bugs); no redo | Undo **and** redo, 32 tests, E2E spec |

<details>
<summary><strong>How each fix works</strong> · plus #507, #521, #455</summary>
<br/>

- **DataLoom [#536](https://github.com/c2siorg/dataloom/pull/536)** · *Scaling* — the upload limit was only checked after the whole body had been read (and the file was read twice, once to count and once to copy). Added a pure-ASGI middleware that counts bytes as the body streams in and answers 413 the moment it passes the budget, a single-pass `.part` → `os.replace` write so a failed upload leaves nothing behind, and sampled pre-gates in profiling. `assess_quality` on a 612k-row, 100 MB CSV: **155.1s → 14.5s**. Parts 1 and 3 of [#533](https://github.com/c2siorg/dataloom/issues/533).
- **DataLoom [#517](https://github.com/c2siorg/dataloom/pull/517)** · *Perf* — dtype inference ran on every dataset read, walking every value of every text column through regex checks: ~5 s per request on a 1M-row dataset with ten ordinary text columns. Added a seeded, one-sided sampled pre-gate that can only *reject* a column, never convert one — conversion still goes through the full path, so results are unchanged. On 1M-row columns: free text **0.481s → 0.007s**, order IDs **0.485s → 0.007s**.
- **DataLoom [#540](https://github.com/c2siorg/dataloom/pull/540)** · *Scaling* — every slow operation ran inside one HTTP request; the client gives up at 30 s, but FastAPI's threadpool keeps going. Reproduced on `main` with a 100 MB CSV and a 180-step pipeline: the UI said "Failed to apply pipeline", the server finished anyway at 36.9 s and wrote 180 change-log rows, the retry failed with a duplicate-column `400`, and a read of the same project waited 28 s behind the write lock. Runs and reverts now return `202` with a job id, run off the request path with progress tracked in a `jobs` table, and can be cancelled before anything is written. Phase 1 of 3 of [#537](https://github.com/c2siorg/dataloom/issues/537).
- **DataLoom [#563](https://github.com/c2siorg/dataloom/pull/563)** · *CI perf* — 11 benchmarks on a fixed-seed 100k-row CSV: date inference, an uncached read, five Operations, profiling, and quality assessment. A *Backend Benchmarks* check times `main` and the PR on the same runner and fails the PR if any benchmark's min time is more than 50% slower. In the normal test job the suite runs once, untimed, so it can't rot. Read-only token and no secrets — the same trust boundary as `lint.yml`.
- **DataLoom [#526](https://github.com/c2siorg/dataloom/pull/526)** · *Architecture* — undo replayed the entire change log, which caused three bugs ([#525](https://github.com/c2siorg/dataloom/issues/525)), including undo returning data the user never had; redo didn't exist. Replaced it with snapshot-based undo **and** redo: new DB model + migration, backend endpoints, a `useUndoState` hook, 32 backend tests (each bug has a regression test that fails on `main`), and a Playwright E2E spec.
- **DataLoom [#507](https://github.com/c2siorg/dataloom/pull/507)** · *Perf* — pagination only existed inside the preview branch, so every write (upload, save, undo, revert, transform, append, pipeline) returned the full dataset — ~9 MB per save on a 121k-row project. Lifted into a shared `paginate_dataframe()` helper and updated the frontend to match.
- **DataLoom [#521](https://github.com/c2siorg/dataloom/pull/521)** · *Perf* — every endpoint re-parsed the working copy from disk on every request. Added a size-bounded LRU cache validated against an `(mtime_ns, size, inode)` fingerprint — plus invalidate-on-write in a `finally` block, because a fingerprint alone can't see a same-tick, same-length rewrite (a cell edit `3 → 4`) on coarse-timestamp filesystems.
- **DataLoom [#455](https://github.com/c2siorg/dataloom/pull/455)** · *CI security* — lint was blocked on every fork PR under `pull_request_target`. Split it along the trust boundary: read-only `lint.yml` on `pull_request`, and a `lint-comment.yml` on `workflow_run` that holds the write permission. Closes the pwn-request hole instead of bypassing the guard.

</details>

<details>
<summary><strong>The other 14 merged PRs</strong></summary>
<br/>

- **DataLoom [#554](https://github.com/c2siorg/dataloom/pull/554)** · *Frontend perf* — rows were paginated but every column was rendered, so a 300-column CSV at 100 rows per page put 30,000 `<td>`s in the DOM. Above 50 columns, `Table` now renders only the columns in the horizontal viewport, with spacer cells for alignment and absolute indices preserved, so sort, drag-reorder and cell edits work unchanged.
- **DataLoom [#488](https://github.com/c2siorg/dataloom/pull/488)** · *Feature* — a `standardizeDates` transformation for columns with mixed date formats, wired through the transform service, the Quality view, and a new form with tests.
- **TensorMap [#367](https://github.com/c2siorg/tensormap/pull/367)** · *Fix* — dataset preview 500'd on any CSV with empty cells, because NaN isn't valid JSON. Switched to pandas `.to_json()` so missing values serialize as `null`.
- **DataLoom [#410](https://github.com/c2siorg/dataloom/pull/410)** · *Data workflow* — preview-before-apply across all 12 transformation forms (GroupBy, Pivot, Melt, Sort, Cast, FillEmpty and the rest) through a shared `usePreviewSave` hook.
- **DataLoom [#383](https://github.com/c2siorg/dataloom/pull/383)** · *Data workflow* — a preview step for row-reducing transforms (Sample, Filter, Drop Duplicates). Every Apply used to permanently overwrite the working CSV, so sampling 300→100 rows and then trying 120 failed silently.
- **DataLoom [#348](https://github.com/c2siorg/dataloom/pull/348)** · *The first merge* — case-insensitive string filtering, so `=`, `!=` and `contains` stop missing "Cash" when you filter for "cash"; consistent signatures for the filter ops, plus a test.
- **DataLoom [#472](https://github.com/c2siorg/dataloom/pull/472)** — kebab menu with Edit and Delete on the Homescreen dataset cards; Edit opens a pre-filled "Edit Project" modal wired to `PATCH /projects/{id}`
- **DataLoom [#470](https://github.com/c2siorg/dataloom/pull/470)** — export charts and the correlation heatmap as images through a reusable `DownloadImageButton`, with tests
- **DataLoom [#467](https://github.com/c2siorg/dataloom/pull/467)** — reusable live password-strength meter on the sign-up, reset-password and change-password forms
- **DataLoom [#465](https://github.com/c2siorg/dataloom/pull/465)** — the `+` on the workspace tab bar opens a dropdown of the Profiling views; also fixes tooltip theming and popovers rendering under sticky table headers
- **DataLoom [#460](https://github.com/c2siorg/dataloom/pull/460)** — sun/moon theme toggle in the top navbar
- **DataLoom [#458](https://github.com/c2siorg/dataloom/pull/458)** — instant custom tooltips in `MenuNavbar`, replacing slow native ones
- **DataLoom [#453](https://github.com/c2siorg/dataloom/pull/453)** — real dark mode on the Quality tab, swapping hardcoded Tailwind light classes for semantic theme tokens
- **DataLoom [#408](https://github.com/c2siorg/dataloom/pull/408)** — hover tooltips on the toolbar icons (the first one — small on purpose)

</details>

### Next, upstream

- **[RFC #601](https://github.com/c2siorg/dataloom/issues/601)** · *open scope check* — rule-based operation suggestions on top of DataLoom's existing profiling (no ML), peak-memory tracking with an 8 GB laptop as the target, and an opt-in local model that maps plain-language questions onto validated operations. Code waits until the maintainers confirm scope.
- **Scaling** — the rest of [#537](https://github.com/c2siorg/dataloom/issues/537) and [#533](https://github.com/c2siorg/dataloom/issues/533), and Parquet as the working copy so dtypes are inferred once ([#524](https://github.com/c2siorg/dataloom/issues/524)).

<br/>

## Built end to end

The AI in all five runs on your own machine.

- **[Veracity](https://github.com/YADUNANDAN-SINGH/Veracity)** · 🥈 *2nd place, CT BuildFest 2.0* — scores a live phone call on the caller's number, voice and words; if they ask for money, PayPal holds the payment until the call checks out. 164 tests, real-call tests, known failures documented. I was the team's only developer. <sub>`python` `react` `whisper` `paypal`</sub>
- **[Any Agent](https://github.com/YADUNANDAN-SINGH/Any-Agent)** — a Chrome side-panel agent on a local Ollama model: browses, fills forms, reads documents, writes PDFs. Built for 8–12B models; it corrects their broken JSON and wrong field picks instead of failing the run. <sub>`typescript` `react` `ollama`</sub>
- **[Prism-RAG](https://github.com/YADUNANDAN-SINGH/Prism-RAG)** — a hand-written Rust vector store, and a UI that shows every retrieved chunk, its score and the exact prompt before the local model answers. Runs on a seeded corpus; no document ingestion yet. <sub>`rust` `python` `react` `ollama`</sub>
- **[GlassBox](https://github.com/YADUNANDAN-SINGH/glassbox)** — an SVD recommender in pure Rust, no linear-algebra crates, compiled to WebAssembly with its database and UI. Runs entirely in your browser tab. <sub>`rust` `wasm` `surrealdb`</sub>
- **[Neuro-Symbolic Solver](https://github.com/YADUNANDAN-SINGH/Neuro-Symbolic-Solver)** — a CNN reads a handwritten expression; plain code does the arithmetic, so the answer can't be hallucinated. <sub>`tensorflow` `fastapi` `react`</sub>

<details>
<summary><b>Also built</b></summary>
<br/>

- **[exam-paper-predictor](https://github.com/YADUNANDAN-SINGH/exam-paper-predictor)** — a Claude skill that predicts an exam paper from 5–20 years of real past papers, backtesting itself on past years first. First real test, CBSE Class 12 Maths 2026: the question types it ranked highest carried 90.8% of the paper's marks; the exact section-by-section match was 32.5%.
- **[Lara](https://yadunandan-singh.pages.dev/)** — the agent on my portfolio site: answers questions about my projects from a grounded knowledge base, and drafts a résumé for a role you name.

</details>

<br/>

## Rebuilt by hand

- `np.linalg.svd` → **[Image Compressor from Scratch](https://github.com/YADUNANDAN-SINGH/svd-image-compression)**: SVD from the eigendecomposition of AᵀA; drag the rank `k` and watch the image change.
- The password field → **[FaceAuth Notes](https://github.com/YADUNANDAN-SINGH/django-svd-face-auth)**: Eigenfaces from the linear algebra up, used as the actual login for a Django notes app.
- Wallet SDKs → **[bitcoin-wallet-rs](https://github.com/YADUNANDAN-SINGH/bitcoin-wallet-rs)**: keys, UTXO discovery and unsigned transactions on signet, straight from `secp256k1` and `rust-bitcoin`.

<details>
<summary><b>The archive</b> — earlier builds, same habit</summary>
<br/>

- **[YouTube Recommender with SVD](https://github.com/YADUNANDAN-SINGH/YouTube-video-recommendation-model-with-SVD)** — TF-IDF + TruncatedSVD taste vectors built from videos you liked *and* the ones you hated. CLI + Flask UI. The project that started the whole SVD obsession.
- **[Geometric Transformation Visualizer](https://github.com/YADUNANDAN-SINGH/geometric-transformation-visualizer)** — type any 2×2 matrix, watch the plane move. Built to make MIT 18.06 tangible; determinant-as-area included.
- **[AI Image Recognition Webapp](https://github.com/YADUNANDAN-SINGH/AI-Image-Recognition-webapp)** — a custom CNN trained on CIFAR-10 behind a drag-and-drop Flask frontend, with top-3 confidence scores.

</details>

<br/>

## Writing

- **[30 Hours, One Laptop, and a Scam-Call Detector](https://medium.com/@yadunandan-ai-dev/30-hours-one-laptop-and-a-scam-call-detector-e915bce8dbb9)** — my first hackathon, and how Veracity came together
- **[Three Signals, One Verdict, and a Payment That Waits](https://medium.com/@yadunandan-ai-dev/three-signals-one-verdict-and-a-payment-that-waits-e754d7fc4a31)** — how Veracity scores a call and holds the payment
- **[How to Find Your First Open Source Project in One Evening](https://medium.com/@yadunandan-ai-dev/how-to-find-your-first-open-source-project-in-one-evening-ab936a19d9dd)** — the method behind the merged PRs above

<details>
<summary>More articles</summary>
<br/>

- **[ChatGPT Failed My Sister's Live Class, So I Built a Neuro-Symbolic Math Solver](https://medium.com/@yadunandan-ai-dev/chatgpt-failed-my-sisters-live-class-so-i-built-a-neuro-symbolic-ai-math-solver-from-scratch-38a809494b67)** — why perception and arithmetic should be two different components
- **[Stop Vibe Coding](https://medium.com/@yadunandan-ai-dev/stop-vibe-coding-how-elite-developers-actually-use-ai-backed-by-real-research-da32f95c0039)** — spec-driven AI development, and what separates it from trusting the output because it *felt* right
- **[Don't Be a Dumb Computer: SVD Explained](https://medium.com/@yadunandan-ai-dev/dont-be-a-dumb-computer-svd-explained-subtitle-i-built-a-youtube-recommender-to-finally-155eabfbcbe6)** — the recommender that finally made SVD click. Built first, written after.
- **[How I Built a Face Authentication App in Django](https://medium.com/@yadunandan-ai-dev/how-i-built-a-face-authentication-app-in-django-a-complete-ml-learning-journey-6ea0a8bbc2ef)** — the write-up behind FaceAuth Notes
- **[Should You Be Scared of Agentic AI?](https://medium.com/@yadunandan-ai-dev/should-you-be-scared-of-agentic-ai-heres-the-reality-189088104d78)** — where agents actually stand, minus the panic
- **[I Stopped Solving Problems and Built a Tool Instead](https://medium.com/@yadunandan-ai-dev/i-stopped-solving-problems-and-built-a-tool-instead-how-a-visualizer-taught-me-real-linear-algebra-17ea52847001)** — how shipping a visualizer taught more linear algebra than the problem sets
- **[Build Your Own Image Recognition Model](https://medium.com/@yadunandan-ai-dev/how-can-you-build-your-own-image-recognition-model-as-your-first-ai-project-090a20c3ec3e)** — a first AI project, end to end

</details>
