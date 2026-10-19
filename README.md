<!-- banner.svg lives in this same repo (YADUNANDAN-SINGH/YADUNANDAN-SINGH), next to this README -->
<div align="center">

<img src="banner.svg" width="100%" alt="Yadunandan Singh — no black boxes. rebuild first, import later."/>

<br/><br/>

### AI Engineer — retrieval, agents, data backends, and the math underneath

I build the layers most people import — vector stores, SVD, inference plumbing — and I make other people's data backends faster. · Jammu, India

[**Portfolio**](https://yadunandan-singh.pages.dev/) · [**Writing**](https://medium.com/@yadunandan-ai-dev) · [**LinkedIn**](https://www.linkedin.com/in/yadunandan-singh-ai-dev/) · [**Email**](mailto:yadunandansingh027@gmail.com)

<br/>

![Python](https://img.shields.io/badge/Python-1a1b27?style=flat-square&logo=python&logoColor=667eea)
![Rust](https://img.shields.io/badge/Rust-1a1b27?style=flat-square&logo=rust&logoColor=667eea)
![TypeScript](https://img.shields.io/badge/TypeScript-1a1b27?style=flat-square&logo=typescript&logoColor=667eea)
![pandas](https://img.shields.io/badge/pandas-1a1b27?style=flat-square&logo=pandas&logoColor=667eea)
![FastAPI](https://img.shields.io/badge/FastAPI-1a1b27?style=flat-square&logo=fastapi&logoColor=667eea)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-1a1b27?style=flat-square&logo=postgresql&logoColor=667eea)
![TensorFlow](https://img.shields.io/badge/TensorFlow-1a1b27?style=flat-square&logo=tensorflow&logoColor=667eea)
![Ollama](https://img.shields.io/badge/Ollama-1a1b27?style=flat-square&logo=ollama&logoColor=667eea)
![React](https://img.shields.io/badge/React-1a1b27?style=flat-square&logo=react&logoColor=667eea)
![Django](https://img.shields.io/badge/Django-1a1b27?style=flat-square&logo=django&logoColor=667eea)
![WebAssembly](https://img.shields.io/badge/WebAssembly-1a1b27?style=flat-square&logo=webassembly&logoColor=667eea)
![Docker](https://img.shields.io/badge/Docker-1a1b27?style=flat-square&logo=docker&logoColor=667eea)

<br/>

<sub><b>18</b> merged upstream PRs &nbsp;·&nbsp; <b>69×</b> faster dtype inference on non-date columns &nbsp;·&nbsp; <b>0</b> LangChain imports</sub>

</div>

<br/>

## Merged upstream

Anyone can push to their own repos. These went through someone else's review.

**18 merged pull requests** across two [c2siorg](https://github.com/c2siorg) projects — 17 in [DataLoom](https://github.com/c2siorg/dataloom), 1 in [TensorMap](https://github.com/c2siorg/tensormap). The recent ones are mostly backend performance and correctness work on DataLoom's pandas data layer.

### Backend & performance

- **DataLoom [#517](https://github.com/c2siorg/dataloom/pull/517)** · *Perf* — dtype inference ran on every dataset read, walking every value of every text column through regex checks. Added a seeded, one-sided sampled pre-gate that can only *reject* a column, never convert one — conversion still goes through the full path, so correctness is unchanged. On 1M-row columns: free text **0.481s → 0.007s**, order IDs **0.485s → 0.007s**, real dates unchanged.
- **DataLoom [#521](https://github.com/c2siorg/dataloom/pull/521)** · *Perf* — every endpoint re-parsed the working copy from disk on every request. Added a size-bounded LRU cache validated against an `(mtime_ns, size, inode)` fingerprint — plus invalidate-on-write in a `finally` block, because a fingerprint alone can't see a same-tick, same-length rewrite (a cell edit `3 → 4`) on coarse-timestamp filesystems.
- **DataLoom [#526](https://github.com/c2siorg/dataloom/pull/526)** · *Architecture* — undo used to replay the entire change log, and redo didn't exist. Replaced it with snapshot-based undo **and** redo: new DB model + migration, backend endpoints, a `useUndoState` hook, and a ~670-line test suite plus an E2E spec.
- **DataLoom [#507](https://github.com/c2siorg/dataloom/pull/507)** · *Perf* — pagination only existed inside the preview branch, so every write (upload, save, undo, revert, transform, append, pipeline) returned the full dataset — ~9 MB per save on a 121k-row project. Lifted into a shared `paginate_dataframe()` helper and updated the frontend to match.
- **DataLoom [#455](https://github.com/c2siorg/dataloom/pull/455)** · *CI security* — lint was blocked on every fork PR under `pull_request_target`. Split it along the trust boundary: read-only `lint.yml` on `pull_request`, and a `lint-comment.yml` on `workflow_run` that holds the write permission. Closes the pwn-request hole instead of bypassing the guard.
- **DataLoom [#488](https://github.com/c2siorg/dataloom/pull/488)** · *Feature* — a `standardizeDates` transformation for columns with mixed date formats, wired through the transform service, the Quality view, and a new form with tests.
- **TensorMap [#367](https://github.com/c2siorg/tensormap/pull/367)** · *Fix* — dataset preview 500'd on any CSV with empty cells, because NaN isn't valid JSON. Switched to pandas `.to_json()` so missing values serialize as `null`.

### Data workflow

- **DataLoom [#410](https://github.com/c2siorg/dataloom/pull/410)** — preview-before-apply across all 12 transformation forms (GroupBy, Pivot, Melt, Sort, Cast, FillEmpty and the rest) through a shared `usePreviewSave` hook.
- **DataLoom [#383](https://github.com/c2siorg/dataloom/pull/383)** — a preview step for row-reducing transforms (Sample, Filter, Drop Duplicates). Every Apply used to permanently overwrite the working CSV, so sampling 300→100 rows and then trying 120 failed silently.
- **DataLoom [#348](https://github.com/c2siorg/dataloom/pull/348)** · *The first merge* — case-insensitive string filtering, so `=`, `!=` and `contains` stop missing "Cash" when you filter for "cash"; consistent signatures for the filter ops, plus a test.

<details>
<summary><b>The other eight</b> — frontend features and fixes, all DataLoom</summary>
<br/>

- **[#472](https://github.com/c2siorg/dataloom/pull/472)** — kebab menu with Edit and Delete on the Homescreen dataset cards; Edit opens a pre-filled "Edit Project" modal wired to `PATCH /projects/{id}`
- **[#470](https://github.com/c2siorg/dataloom/pull/470)** — export charts and the correlation heatmap as images through a reusable `DownloadImageButton`, with tests
- **[#467](https://github.com/c2siorg/dataloom/pull/467)** — reusable live password-strength meter on the sign-up, reset-password and change-password forms
- **[#465](https://github.com/c2siorg/dataloom/pull/465)** — the `+` on the workspace tab bar opens a dropdown of the Profiling views; also fixes tooltip theming and popovers rendering under sticky table headers
- **[#460](https://github.com/c2siorg/dataloom/pull/460)** — sun/moon theme toggle in the top navbar
- **[#458](https://github.com/c2siorg/dataloom/pull/458)** — instant custom tooltips in `MenuNavbar`, replacing slow native ones
- **[#453](https://github.com/c2siorg/dataloom/pull/453)** — real dark mode on the Quality tab, swapping hardcoded Tailwind light classes for semantic theme tokens
- **[#408](https://github.com/c2siorg/dataloom/pull/408)** — hover tooltips on the toolbar icons (the first one — small on purpose)

</details>

<br/>

## AI systems

LLM-shaped work where the interesting part is what happens *before* the model is called — retrieval, grounding, and knowing which component is allowed to be wrong.

| | |
|:--|:--|
| **[Prism-RAG](https://github.com/YADUNANDAN-SINGH/Prism-RAG)**<br/><sub>`rust · axum` `python` `react` `ollama` `docker`</sub> | A RAG pipeline with the lid off. Hand-rolled in-memory vector store in Rust (cosine similarity over 384-d MiniLM embeddings), and a 3-panel UI that shows every retrieved chunk and the exact assembled prompt *before* a local LLM streams a single token. Built instead of importing LangChain. Runs over a seeded corpus today; document ingestion and chunking are next. |
| **[Lara — the agent on my portfolio](https://yadunandan-singh.pages.dev/)**<br/><sub>`react` `django` `python`</sub> | The site ships its own agent. Ask Lara about a project and she answers from a grounded knowledge base; ask for a résumé and she generates one targeted at the role you name. Merged-PR counts on the page are pulled live from the GitHub API, so the site can't quietly go stale. |
| **[Neuro-Symbolic Solver](https://github.com/YADUNANDAN-SINGH/Neuro-Symbolic-Solver)**<br/><sub>`tensorflow` `opencv` `fastapi` `react`</sub> | A CNN reads the handwritten expression; a deterministic evaluator computes the answer. The neural net does perception, the math does math — nothing gets to hallucinate arithmetic. |
| **[GlassBox](https://github.com/YADUNANDAN-SINGH/GlassBox-Rust-SVD-recommendation-system)**<br/><sub>`rust` `leptos` `surrealdb` `wasm`</sub> | The entire recommendation engine — SVD, database, UI — compiled to WebAssembly and running in *your* browser tab. Embedded SurrealDB over IndexedDB. Zero servers, zero telemetry, zero inference bill. |

<br/>

## Foundations, rebuilt by hand

The ML primitives, written out from the linear algebra rather than called.

| The black box | What I built instead |
|:--|:--|
| `np.linalg.svd` | **[Image Compressor from Scratch](https://github.com/YADUNANDAN-SINGH/Image_Compression_via_Low-Rank_Matrix-_Approximation)** — SVD assembled by hand from the eigendecomposition of AᵀA, then pointed at lossy image compression with a live quality slider: drag `k`, watch the rank drop.<br/><sub>`python` `numpy` `flask`</sub> |
| The password field | **[FaceAuth Notes](https://github.com/YADUNANDAN-SINGH/django-svd-face-auth)** — Eigenfaces written out from the linear algebra (mean face → centered matrix → SVD → projection weights) and wired in as the actual login for a Django notes app, with auto-augmented training shots per user.<br/><sub>`django` `opencv` `numpy` `docker`</sub> |
| Wallet SDKs | **[bitcoin-wallet-rs](https://github.com/YADUNANDAN-SINGH/bitcoin-wallet-rs)** — key generation, wallet persistence, UTXO discovery and raw unsigned-transaction construction on signet, built directly on `secp256k1` and `rust-bitcoin` primitives. Signing is next.<br/><sub>`rust` `secp256k1` `signet`</sub> |

<details>
<summary><b>The archive</b> — earlier builds, same habit</summary>
<br/>

- **[YouTube Recommender with SVD](https://github.com/YADUNANDAN-SINGH/YouTube-video-recommendation-model-with-SVD)** — TF-IDF + TruncatedSVD taste vectors built from videos you liked *and* the ones you hated. CLI + Flask UI. The project that started the whole SVD obsession.
- **[Geometric Transformation Visualizer](https://github.com/YADUNANDAN-SINGH/Geometric-Transformation-Visualizer-)** — type any 2×2 matrix, watch the plane move. Built to make MIT 18.06 tangible; determinant-as-area included.
- **[AI Image Recognition Webapp](https://github.com/YADUNANDAN-SINGH/AI-Image-Recognition-webapp)** — a custom CNN trained on CIFAR-10 behind a drag-and-drop Flask frontend, with top-3 confidence scores.
- **[Delhi-NCR Property Price Predictor](https://github.com/YADUNANDAN-SINGH/Delhi-NCR-property-price-predictor)** — auto-cleaning pipeline for messy real-estate listings plus a model bake-off, shipping the winner as a pickled artifact.

</details>

<br/>

## Now

- **DataLoom, scaling up** — running slow operations as background jobs with progress reporting ([#540](https://github.com/c2siorg/dataloom/pull/540)), and lifting the 10 MB upload cap with streamed upload and sample-based profiling ([#533](https://github.com/c2siorg/dataloom/issues/533))
- **DataLoom, keeping it fast** — a `pytest-benchmark` suite in CI that fails the build on performance regressions
- **Next up** — the signing path in `bitcoin-wallet-rs`; a signed transaction on signet is the milestone

<br/>

## Writing

Every article is downstream of something I actually shipped.

- **[Stop Vibe Coding](https://medium.com/@yadunandan-ai-dev/stop-vibe-coding-how-elite-developers-actually-use-ai-backed-by-real-research-da32f95c0039)** — spec-driven AI development, and what separates it from trusting the output because it *felt* right
- **[Should You Be Scared of Agentic AI?](https://medium.com/@yadunandan-ai-dev/should-you-be-scared-of-agentic-ai-heres-the-reality-189088104d78)** — where agents actually stand, minus the panic
- **[ChatGPT Failed My Sister's Live Class, So I Built a Neuro-Symbolic Math Solver](https://medium.com/@yadunandan-ai-dev/chatgpt-failed-my-sisters-live-class-so-i-built-a-neuro-symbolic-ai-math-solver-from-scratch-38a809494b67)** — why perception and arithmetic should be two different components
- **[How to Find Your First Open Source Project in One Evening](https://medium.com/@yadunandan-ai-dev/how-to-find-your-first-open-source-project-in-one-evening-ab936a19d9dd)** — the method behind the merged PRs above: choose on maintainer responsiveness and community tone, not stars
- **[Don't Be a Dumb Computer: SVD Explained](https://medium.com/@yadunandan-ai-dev/dont-be-a-dumb-computer-svd-explained-subtitle-i-built-a-youtube-recommender-to-finally-155eabfbcbe6)** — the recommender that finally made SVD click. Built first, written after.
- **[I Stopped Solving Problems and Built a Tool Instead](https://medium.com/@yadunandan-ai-dev/i-stopped-solving-problems-and-built-a-tool-instead-how-a-visualizer-taught-me-real-linear-algebra-17ea52847001)** — how shipping a visualizer taught more linear algebra than the problem sets
- **[Build Your Own Image Recognition Model](https://medium.com/@yadunandan-ai-dev/how-can-you-build-your-own-image-recognition-model-as-your-first-ai-project-090a20c3ec3e)** — a first AI project, end to end

<br/>

## Numbers

<div align="center">
<img src="https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=yadunandan-singh&theme=github_dark" alt="GitHub Profile Summary" width="100%"/>
<br/><br/>
<img height="165" src="stats.svg" alt="GitHub stats"/>
<img height="165" src="https://github-readme-stats.shion.dev/api/top-langs/?username=YADUNANDAN-SINGH&layout=compact&hide_border=true&bg_color=00000000&title_color=667eea&text_color=8b95c9" alt="Top languages"/>
</div>

<br/>

<div align="center">
<sub>The banner above is a hand-written SVG. Of course it is.</sub>
</div>
