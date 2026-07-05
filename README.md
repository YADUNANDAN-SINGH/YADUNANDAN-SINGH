<!-- banner.svg lives in this same repo (YADUNANDAN-SINGH/YADUNANDAN-SINGH), next to this README -->
<div align="center">
<svg viewBox="0 0 1200 300" width="1200" height="300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Yadunandan Singh — no black boxes. rebuild first, import later.">
  <defs>
    <linearGradient id="acc" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#667EEA"/>
      <stop offset="1" stop-color="#764BA2"/>
    </linearGradient>
    <radialGradient id="glow" cx="0.14" cy="-0.1" r="0.9">
      <stop offset="0" stop-color="#667EEA" stop-opacity="0.16"/>
      <stop offset="0.55" stop-color="#667EEA" stop-opacity="0.05"/>
      <stop offset="1" stop-color="#667EEA" stop-opacity="0"/>
    </radialGradient>
    <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
      <path d="M 40 0 H 0 V 40" fill="none" stroke="#8B95C9" stroke-opacity="0.07" stroke-width="0.6"/>
    </pattern>
  </defs>

  <style>
    .disp { font-family: 'Segoe UI','Helvetica Neue',Helvetica,Arial,sans-serif; }
    .mono { font-family: ui-monospace,'SF Mono','Cascadia Code',Menlo,Consolas,'Liberation Mono',monospace; }
    .sig  { animation: pulse 2.8s ease-in-out infinite; }
    .d1   { animation-delay: .35s; }
    .d2   { animation-delay: .7s; }
    .d3   { animation-delay: 1.05s; }
    .cur  { animation: blink 1.1s steps(2, start) infinite; }
    @keyframes pulse { 0%, 100% { opacity: .3; } 50% { opacity: 1; } }
    @keyframes blink { 0% { opacity: 1; } 50% { opacity: 0; } 100% { opacity: 1; } }
    @media (prefers-reduced-motion: reduce) {
      .sig, .cur { animation: none; opacity: 1; }
    }
  </style>

  <!-- card -->
  <rect x="1" y="1" width="1198" height="298" rx="18" fill="#0A0E1A" stroke="url(#acc)" stroke-width="1.5"/>
  <rect x="1" y="1" width="1198" height="298" rx="18" fill="url(#grid)"/>
  <rect x="1" y="1" width="1198" height="298" rx="18" fill="url(#glow)"/>

  <!-- left: identity -->
  <rect x="62" y="82" width="46" height="4" rx="2" fill="url(#acc)"/>
  <text x="60" y="142" class="disp" font-size="58" font-weight="800" letter-spacing="-1" fill="#F4F6FF">Yadunandan Singh</text>
  <text x="62" y="186" class="mono" font-size="19" fill="#A9B4E8">no black boxes. rebuild first, import later.<tspan class="cur" fill="#667EEA">&#160;&#9615;</tspan></text>

  <!-- corners -->
  <text x="62" y="266" class="mono" font-size="11.5" letter-spacing="1" fill="#59639A">est. 2007 &#183; self-taught</text>
  <text x="1138" y="266" class="mono" font-size="11.5" letter-spacing="1" fill="#59639A" text-anchor="end">32.73&#176; N, 74.87&#176; E &#183; jammu, india</text>

  <!-- right: A = U Sigma V^T -->
  <g transform="translate(782, 92)">

    <!-- A : the raw problem -->
    <g>
      <text x="29" y="-14" class="mono" font-size="15" font-style="italic" fill="#8B95C9" text-anchor="middle">A</text>
      <path d="M 10 0 H 0 V 88 H 10" fill="none" stroke="#4A5490" stroke-width="2"/>
      <path d="M 48 0 H 58 V 88 H 48" fill="none" stroke="#4A5490" stroke-width="2"/>
      <g fill="#39406B">
        <circle cx="17" cy="22" r="3"/><circle cx="29" cy="22" r="3"/><circle cx="41" cy="22" r="3"/>
        <circle cx="17" cy="44" r="3"/><circle cx="29" cy="44" r="3"/><circle cx="41" cy="44" r="3"/>
        <circle cx="17" cy="66" r="3"/><circle cx="29" cy="66" r="3"/><circle cx="41" cy="66" r="3"/>
      </g>
    </g>

    <text x="79" y="52" class="mono" font-size="26" fill="#8B95C9" text-anchor="middle">=</text>

    <!-- U : basis -->
    <g transform="translate(100, 0)">
      <text x="24" y="-14" class="mono" font-size="15" font-style="italic" fill="#8B95C9" text-anchor="middle">U</text>
      <path d="M 10 0 H 0 V 88 H 10" fill="none" stroke="#4A5490" stroke-width="2"/>
      <path d="M 38 0 H 48 V 88 H 38" fill="none" stroke="#4A5490" stroke-width="2"/>
      <line x1="15" y1="16" x2="15" y2="72" stroke="#5A63B8" stroke-width="2.5" opacity="0.85"/>
      <line x1="24" y1="16" x2="24" y2="72" stroke="#5A63B8" stroke-width="2.5" opacity="0.5"/>
      <line x1="33" y1="16" x2="33" y2="72" stroke="#5A63B8" stroke-width="2.5" opacity="0.28"/>
    </g>

    <!-- Sigma : what matters, ranked -->
    <g transform="translate(166, 0)">
      <text x="33" y="-14" class="mono" font-size="15" font-style="italic" fill="#8B95C9" text-anchor="middle">&#931;</text>
      <path d="M 10 0 H 0 V 88 H 10" fill="none" stroke="#4A5490" stroke-width="2"/>
      <path d="M 56 0 H 66 V 88 H 56" fill="none" stroke="#4A5490" stroke-width="2"/>
      <circle class="sig"    cx="17" cy="20" r="7"   fill="#F7931A"/>
      <circle class="sig d1" cx="31" cy="38" r="5.5" fill="#7C86F5"/>
      <circle class="sig d2" cx="44" cy="55" r="4"   fill="#764BA2"/>
      <circle class="sig d3" cx="55" cy="70" r="2.8" fill="#5A63B8"/>
    </g>

    <!-- V^T : how it ships -->
    <g transform="translate(250, 0)">
      <text x="24" y="-14" class="mono" font-size="15" font-style="italic" fill="#8B95C9" text-anchor="middle">V&#7488;</text>
      <path d="M 10 0 H 0 V 88 H 10" fill="none" stroke="#4A5490" stroke-width="2"/>
      <path d="M 38 0 H 48 V 88 H 38" fill="none" stroke="#4A5490" stroke-width="2"/>
      <line x1="10" y1="24" x2="38" y2="24" stroke="#5A63B8" stroke-width="2.5" opacity="0.85"/>
      <line x1="10" y1="44" x2="38" y2="44" stroke="#5A63B8" stroke-width="2.5" opacity="0.5"/>
      <line x1="10" y1="64" x2="38" y2="64" stroke="#5A63B8" stroke-width="2.5" opacity="0.28"/>
    </g>

    <text x="149" y="126" class="mono" font-size="12.5" fill="#667EEA" text-anchor="middle" opacity="0.9">keep only what carries signal</text>
  </g>
</svg>

<img src=" <img width="1200" height="300" alt="banner" src="https://github.com/user-attachments/assets/d80ff7c5-b303-4057-adff-02a8bbe92ef1" />
  " width="100%" alt="Yadunandan Singh — no black boxes. rebuild first, import later."/>

<br/><br/>

**systems · machine learning · bitcoin protocol** — self-taught, Jammu, India

[portfolio](https://yadunandan-singh.pages.dev/) · [medium](https://medium.com/@yadunandan-ai-dev) · [linkedin](https://www.linkedin.com/in/yadunandan-singh-ai-dev/) · [email](mailto:yadunandansingh105@gmail.com)

</div>

<br/>

## The rule

**No black boxes.** If I haven't built it once from first principles, I don't get to `import` it.

That one rule produced everything below.

| The black box | What I built instead |
|:--|:--|
| `np.linalg.svd` | **[Image Compressor from Scratch](https://github.com/YADUNANDAN-SINGH/Image_Compression_via_Low-Rank_Matrix-_Approximation)** — SVD assembled by hand from the eigendecomposition of AᵀA, then pointed at lossy image compression with a live quality slider: drag `k`, watch the rank drop.<br/><sub>`python` `numpy` `flask`</sub> |
| A recommender API on someone's server | **[GlassBox](https://github.com/YADUNANDAN-SINGH/GlassBox-Rust-SVD-recommendation-system)** — the entire recommendation engine (SVD, database, UI) compiled to WebAssembly and running in *your* browser tab. Embedded SurrealDB over IndexedDB; zero servers, zero telemetry.<br/><sub>`rust` `leptos` `surrealdb` `wasm`</sub> |
| LangChain | **[Prism-RAG](https://github.com/YADUNANDAN-SINGH/Prism-RAG)** — a RAG pipeline with the lid off: a hand-rolled in-memory vector database in Rust (cosine similarity over 384-d embeddings), and a 3-panel UI that shows every retrieved chunk and the exact assembled prompt before a local LLM streams a single token.<br/><sub>`rust · axum` `python` `react` `ollama` `docker`</sub> |
| "just ask an LLM to do the math" | **[Neuro-Symbolic Solver](https://github.com/YADUNANDAN-SINGH/Neuro-Symbolic-Solver)** — a CNN reads the handwritten expression; a deterministic evaluator computes the answer. The neural net does perception, the math does math — nothing gets to hallucinate arithmetic.<br/><sub>`tensorflow` `opencv` `fastapi` `react`</sub> |
| The password field | **[FaceAuth Notes](https://github.com/YADUNANDAN-SINGH/django-svd-face-auth)** — Eigenfaces written out from the linear algebra (mean face → centered matrix → SVD → projection weights) and wired in as the actual login for a Django notes app, with auto-augmented training shots per user.<br/><sub>`django` `opencv` `numpy` `docker`</sub> |
| Wallet SDKs | **[bitcoin-wallet-rs](https://github.com/YADUNANDAN-SINGH/bitcoin-wallet-rs)** — key generation, wallet persistence, UTXO discovery and raw unsigned-transaction construction on signet, built directly on `secp256k1` and `rust-bitcoin` primitives. Signing is next.<br/><sub>`rust` `secp256k1` `signet`</sub> |

## Merged upstream

Anyone can push to their own repos. These went through someone else's review.

- 🟣 **c2siorg / DataLoom** — [#383](https://github.com/c2siorg/DataLoom/pull/383) preview-before-persist flow for row-reducing transforms, so users validate output before it ever hits the pipeline
- 🟣 **c2siorg / DataLoom** — [#348](https://github.com/c2siorg/DataLoom/pull/348) fixed strict case-sensitivity in string filtering, plus NaN handling, dtype checks, and test coverage
- 🟣 **c2siorg / TensorMap** — [#367](https://github.com/c2siorg/TensorMap/pull/367) fixed a FastAPI 500 by repairing NaN → JSON serialization in dataset preview
- 🟢 **in review** — [#410](https://github.com/c2siorg/DataLoom/pull/410) extends the apply-preview workflow across DataLoom's entire transform layer (11+ modules)

<details>
<summary><b>The archive</b> — earlier builds, same habit</summary>
<br/>

- **[YouTube Recommender with SVD](https://github.com/YADUNANDAN-SINGH/YouTube-video-recommendation-model-with-SVD)** — TF-IDF + TruncatedSVD taste vectors built from videos you liked *and* the ones you hated. CLI + Flask UI. The project that started the whole SVD obsession.
- **[Geometric Transformation Visualizer](https://github.com/YADUNANDAN-SINGH/Geometric-Transformation-Visualizer-)** — type any 2×2 matrix, watch the plane move. Built to make MIT 18.06 tangible; determinant-as-area included.
- **[AI Image Recognition Webapp](https://github.com/YADUNANDAN-SINGH/AI-Image-Recognition-webapp)** — a custom CNN trained on CIFAR-10 behind a drag-and-drop Flask frontend, with top-3 confidence scores.
- **[Delhi-NCR Property Price Predictor](https://github.com/YADUNANDAN-SINGH/Delhi-NCR-property-price-predictor)** — auto-cleaning pipeline for messy real-estate listings plus a model bake-off, shipping the winner as a pickled artifact.

</details>

## Writing

- **[Should You Be Scared of Agentic AI?](https://medium.com/@yadunandan-ai-dev/should-you-be-scared-of-agentic-ai-heres-the-reality-189088104d78)** — where agents actually stand, minus the panic
- **[Don't Be a Dumb Computer: SVD Explained](https://medium.com/@yadunandan-ai-dev/dont-be-a-dumb-computer-svd-explained-subtitle-i-built-a-youtube-recommender-to-finally-155eabfbcbe6)** — the recommender that finally made SVD click. Built first, written after.
- **[I Stopped Solving Problems and Built a Tool Instead](https://medium.com/@yadunandan-ai-dev/i-stopped-solving-problems-and-built-a-tool-instead-how-a-visualizer-taught-me-real-linear-algebra-17ea52847001)** — how shipping a visualizer taught more linear algebra than the problem sets
- **[Build Your Own Image Recognition Model](https://medium.com/@yadunandan-ai-dev/how-can-you-build-your-own-image-recognition-model-as-your-first-ai-project-090a20c3ec3e)** — a first AI project, end to end

## Now

- Shipping the apply-preview workflow across DataLoom's whole transform layer — [c2siorg #410](https://github.com/c2siorg/DataLoom/pull/410), in review
- IIT Madras — BS in Data Science & Applications, qualifier July 2026
- The long game: GSoC, Summer of Bitcoin, and a signed transaction on signet

## Numbers

<div align="center">
<img height="165" src="https://github-readme-stats.vercel.app/api?username=YADUNANDAN-SINGH&show_icons=true&hide_border=true&bg_color=00000000&title_color=667eea&icon_color=764ba2&text_color=8b95c9&ring_color=667eea" alt="GitHub stats"/>
<img height="165" src="https://github-readme-stats.vercel.app/api/top-langs/?username=YADUNANDAN-SINGH&layout=compact&hide_border=true&bg_color=00000000&title_color=667eea&text_color=8b95c9" alt="Top languages"/>
</div>

<br/>

<div align="center">
<sub>The banner above is a hand-written SVG. Of course it is.</sub>
</div>
