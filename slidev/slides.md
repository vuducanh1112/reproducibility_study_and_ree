--- 
theme: seriph
title: Reproducibility in Scientific Software Repositories and the Reusable Execution Environment
author: Vu, Blessing, Dragoljic, Goedicke
layout: center
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 35min
---

# Reproducibility in Scientific Software Repositories and the Reusable Execution Environment 

Anh Duc Vu, Christoph Blessing, Ana Dragoljic, Michael Goedicke

<!--
Welcome everyone. Today I want to talk about a problem that sits at the intersection of software engineering and scientific methodology — the reproducibility crisis in research software.

The premise is simple and somewhat uncomfortable: having access to the source code of a scientific experiment is often not enough to actually re-run it. We'll look at why that is, how widespread the problem is, and what the landscape looks like in practice.
-->

---
layout: default
---

# "Works on My Machine"

<div class="flex flex-col h-full gap-4">

<div class="grid grid-cols-3 gap-4 mt-2">

<div class="border border-red-300 rounded-lg p-4 bg-red-50">
<h3 class="font-bold text-red-600 mb-1">Software Decay</h3>
<p class="text-sm text-gray-700">Code that worked at publication often fails 6 months later due to <strong>bit rot</strong> — silent breakage as the ecosystem evolves underneath it.</p>
</div>

<div class="border border-yellow-300 rounded-lg p-4 bg-yellow-50">
<h3 class="font-bold text-yellow-600 mb-1">The Missing Environment</h3>
<p class="text-sm text-gray-700">Papers document the <strong>math</strong>, rarely the <strong>environment</strong> — OS version, compiler flags, shared libraries, and runtime configuration are invisible.</p>
</div>

<div class="border border-orange-300 rounded-lg p-4 bg-orange-50">
<h3 class="font-bold text-orange-600 mb-1">Numerical Non-Determinism</h3>
<p class="text-sm text-gray-700">Small changes in C-libraries like <code>glibc</code> or <code>MKL</code> can lead to <strong>divergent scientific conclusions</strong> from identical source code.</p>
</div>

</div>

<div class="p-4 rounded-lg border border-blue-200 bg-blue-50">
  <p class="text-sm font-bold text-blue-800 mb-1">"No Data Without Software" — NFDIxCS, Berlin Science Week 2024</p>
  <p class="text-sm text-gray-700">The <strong>FAIR principles</strong> were designed for data — but data does not exist without the software that generates and interprets it. Research management must treat the <strong>execution environment as a primary research artifact</strong>, not an afterthought.</p>
</div>

</div>

<!--
The title "Works on My Machine" is something every developer recognises immediately — it is almost a joke in software engineering. But in research software it stops being funny, because there is no shared machine, and often no way to recreate one.

There are three core failure modes that explain why having the code is not enough.

The first is software decay — also called bit rot. The codebase itself does not change, but the world around it does. Libraries release breaking changes, Python versions move on, APIs get deprecated. Code that ran perfectly at submission may be completely broken six months later with no changes to the repository.

The second is the missing environment. A paper documents every equation, every hyperparameter, every architectural decision — but says nothing about the OS, the compiler, or which version of BLAS was linked against. That information lives implicitly on the author's machine and almost never makes it into the repository.

The third is numerical non-determinism — subtle but dangerous. Switching from one version of glibc or MKL to another can change floating point behaviour in ways that silently alter results. Same code, different numbers, potentially different scientific conclusions.

The callout at the bottom connects this to a broader policy and research management context. At Berlin Science Week 2024, NFDIxCS hosted a panel under the title "No Data Without Software" — and the core argument is exactly this: the FAIR principles were originally conceived for research data, but software is equally subject to them. A dataset without its analysis software is incomplete. You cannot make data findable or reusable if the software that produced or interprets it has rotted away. Research management, funding bodies, and publication venues all need to start treating the execution environment as a first-class research artifact — not something you mention in a footnote if you happen to remember.
-->


---
layout: default
---

# How Bad Is It? The Numbers

<div class="flex flex-col h-full gap-4 mt-2">

<div class="grid grid-cols-3 gap-4">

  <div class="rounded-lg p-4 text-center border border-red-200 bg-red-50">
    <div class="text-4xl font-bold text-red-600">70%</div>
    <p class="text-sm mt-1 text-gray-700">of 1,576 researchers <strong>failed to reproduce</strong> a published result</p>
    <p class="text-xs text-gray-400 mt-2">Nature survey — Baker 2016</p>
  </div>

  <div class="rounded-lg p-4 text-center border border-orange-200 bg-orange-50">
    <div class="text-4xl font-bold text-orange-600">70%</div>
    <p class="text-sm mt-1 text-gray-700">of <strong>AI research specifically</strong> estimated to be irreproducible</p>
    <p class="text-xs text-gray-400 mt-2">Ioannidis 2022</p>
  </div>

  <div class="rounded-lg p-4 text-center border border-yellow-200 bg-yellow-50">
    <div class="text-4xl font-bold text-yellow-600">0%</div>
    <p class="text-sm mt-1 text-gray-700">of 400 AI papers <strong>documented all variables</strong> needed for reproduction</p>
    <p class="text-xs text-gray-400 mt-2">Gundersen & Kjensmo, AAAI 2018</p>
  </div>

</div>

<div class="border border-gray-200 rounded-lg p-4 text-sm bg-gray-50">
  <h3 class="font-bold mb-3 text-gray-700">Jupyter Notebooks — A Case Study</h3>
  <div class="grid grid-cols-2 gap-6">
    <div>
      <p class="text-gray-700">Only <strong class="text-red-600">13.7%</strong> of notebooks declared their dependencies — and of those, installation failure rates were <strong class="text-red-600">61–67%</strong>.</p>
      <p class="text-gray-400 text-xs mt-1">Pimentel et al., MSR 2019</p>
    </div>
    <div>
      <p class="text-gray-700"><strong class="text-red-600">87.6%</strong> of biomedical Jupyter notebooks resulted in exceptions when re-executed — <code>ModuleNotFoundError</code>, <code>FileNotFoundError</code>, <code>ImportError</code>.</p>
      <p class="text-gray-400 text-xs mt-1">Samuel & Mietchen, GigaScience 2024</p>
    </div>
  </div>
</div>

</div>

<!--
This is not a theoretical concern — the data is quite damning, and it holds across different types of studies and different ways of measuring the problem.

The first number comes from Baker's 2016 Nature survey of over 1,500 researchers across disciplines — more than 70% had failed to reproduce someone else's published result. More than half had failed to reproduce their own. This is the most widely cited figure in the reproducibility literature and it has held up well over time.

The second is specific to AI. Ioannidis in 2022 estimated that 70% of AI research is irreproducible, attributing it in part to the relative immaturity of the field compared to disciplines like physics that have had decades to develop rigorous experimental norms. This is a striking claim — and it comes from one of the most prominent researchers in the reproducibility space.

The third is perhaps the most concrete. Gundersen and Kjensmo surveyed 400 papers from AAAI and IJCAI — the top AI conferences — and found that not a single paper documented all the variables needed to reproduce its results. Only 20 to 30 percent of the required variables were documented on average. Only 6 percent explicitly stated which research questions they were answering. These are papers that passed peer review at top venues.

The Jupyter notebook studies ground this in something tangible. Notebooks are supposed to be the gold standard of shareable, executable research — the format literally designed for reproducibility. And yet less than 14% declared their dependencies at all, installation failed more than 60% of the time even when they did, and nearly 9 in 10 biomedical notebooks crashed immediately on re-execution with import or file errors.

The pattern is consistent: reproducibility is treated as an afterthought across venues, fields, and years.
-->

---
layout: default
---

# What Does Reproducible Mean?

<div class="flex flex-col gap-6 mt-2">

<div class="grid grid-cols-3 gap-4 text-xs">

  <div class="flex flex-col gap-2 p-4 rounded border border-gray-200 bg-gray-50">
    <span class="font-mono font-bold text-gray-400 text-[10px] uppercase tracking-wide">Reproducible</span>
    <div class="flex gap-3 text-[11px]">
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">Same data</span>
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">Same code</span>
    </div>
    <p class="text-gray-600 leading-relaxed">Re-run the exact same analysis and get the exact same results. The baseline of scientific integrity.</p>
  </div>

  <div class="flex flex-col gap-2 p-4 rounded border border-gray-200 bg-gray-50">
    <span class="font-mono font-bold text-gray-400 text-[10px] uppercase tracking-wide">Replicable</span>
    <div class="flex gap-3 text-[11px]">
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">New data</span>
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">Same code</span>
    </div>
    <p class="text-gray-600 leading-relaxed">Apply the same method to new data and see if findings hold. Tests generalisability.</p>
  </div>

  <div class="flex flex-col gap-2 p-4 rounded border border-gray-200 bg-gray-50">
    <span class="font-mono font-bold text-gray-400 text-[10px] uppercase tracking-wide">Robust</span>
    <div class="flex gap-3 text-[11px]">
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">Same data</span>
      <span class="bg-gray-200 text-gray-600 rounded px-1.5 py-0.5 font-mono">New code</span>
    </div>
    <p class="text-gray-600 leading-relaxed">Re-implement the analysis independently. Confirms findings are not artifacts of one tool or library.</p>
  </div>

</div>

<div class="p-4 rounded-lg border border-blue-200 bg-blue-50">
  <p class="text-xs text-blue-600 font-semibold uppercase tracking-wide mb-3">Our Focus: Computational Reproducibility</p>
  <div class="flex flex-col gap-1.5 text-sm">
    <div class="flex gap-2">
      <span class="text-blue-400 shrink-0">→</span>
      <span class="text-gray-700">Before asking whether results <em>replicate</em> or are <em>robust</em>, we need to be able to <strong>re-run the code at all</strong></span>
    </div>
    <div class="flex gap-2">
      <span class="text-blue-400 shrink-0">→</span>
      <span class="text-gray-700">The <strong>runtime environment</strong> is the prerequisite. OS, libraries, compiler, hardware all affect results.</span>
    </div>
    <div class="flex gap-2">
      <span class="text-blue-400 shrink-0">→</span>
      <span class="text-gray-700">"Same code" must actually behave as "same code" <strong>across machines and years</strong></span>
    </div>
  </div>
</div>

</div>

<!--
Before we dive into the technical details, let's align on terminology — because "reproducibility" is one of those words that gets used to mean three quite different things, and the distinction matters.

The first is reproducibility in the strict sense — same data, same code, same results. This is the absolute baseline. If I give you my dataset and my code and you cannot get the same numbers I published, something is fundamentally broken. This is what we are primarily concerned with today.

The second is replicability — same code, new data. The method travels, but the data doesn't. This is about whether your approach generalises beyond the specific dataset you used. Important scientifically, but it assumes you can already run the code in the first place.

The third is robustness — same data, but a completely independent reimplementation. Someone else writes the analysis from scratch and checks whether they reach the same conclusions. This is the gold standard of scientific validation, but again — it presupposes that the original result is at minimum reproducible.

The key point is this: reproducibility is the prerequisite for everything else. You cannot meaningfully ask whether a result replicates or is robust if you cannot even re-run the original code. That is the problem we are focused on — and it turns out that just being able to re-run the code is already a significant unsolved challenge in practice.

The runtime environment is the hidden variable. Same code on a different machine, a different OS, a different version of numpy or glibc — and you may get different results, or no results at all. Our focus is on making the environment itself a first-class, reproducible artifact.
-->

---
layout: default
---

# Reproducibility Levels

<div class="flex flex-col gap-1.5 text-[11px] leading-tight">

<!-- Level 1 -->
<div v-click="1" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-red-50 border border-red-200">
  <span class="font-mono font-bold text-red-600 text-[10px] bg-red-100 border border-red-300 rounded px-1.5 py-0.5 w-fit">LVL 01</span>
  <div class="font-semibold text-gray-800">Natural language only <span class="text-gray-400 font-normal">(README)</span></div>
  <code class="text-gray-500">"This project requires python 3.10 and numpy."</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ Likely unprecise and incomplete.</span>
    <span class="text-green-700">✅ Create a formal dependency file.</span>
  </div>
</div>

<!-- Level 2 -->
<div v-click="2" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-orange-50 border border-orange-200">
  <span class="font-mono font-bold text-orange-600 text-[10px] bg-orange-100 border border-orange-300 rounded px-1.5 py-0.5 w-fit">LVL 02</span>
  <div class="font-semibold text-gray-800">Manifest file <span class="text-gray-400 font-normal">(requirements.txt)</span></div>
  <code class="text-gray-500">requirements.txt:<br/>pandas</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ Installs different versions on different days.</span>
    <span class="text-green-700">✅ Pin required versions (<code>pandas==2.1.0</code>).</span>
  </div>
</div>

<!-- Level 3 -->
<div v-click="3" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-yellow-50 border border-yellow-200">
  <span class="font-mono font-bold text-yellow-700 text-[10px] bg-yellow-100 border border-yellow-300 rounded px-1.5 py-0.5 w-fit">LVL 03</span>
  <div class="font-semibold text-gray-800">Top-level versions pinned</div>
  <code class="text-gray-500">pandas==2.1.0</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ Transitive dependencies still float.</span>
    <span class="text-green-700">✅ Use a <strong>Lockfile</strong> (<code>poetry.lock</code>).</span>
  </div>
</div>

<!-- Level 4 -->
<div v-click="5" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-blue-50 border border-blue-200">
  <span class="font-mono font-bold text-blue-600 text-[10px] bg-blue-100 border border-blue-300 rounded px-1.5 py-0.5 w-fit">LVL 04</span>
  <div class="font-semibold text-gray-800">Dependencies locked</div>
  <code class="text-gray-500">poetry.lock</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ No system deps (<code>libblas</code>, <code>glibc</code>, CUDA).</span>
    <span class="text-green-700">✅ Add <strong>Containers</strong> or <strong>VMs</strong>.</span>
  </div>
</div>

<!-- Level 5 -->
<div v-click="6" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-emerald-50 border border-emerald-200">
  <span class="font-mono font-bold text-emerald-700 text-[10px] bg-emerald-100 border border-emerald-300 rounded px-1.5 py-0.5 w-fit">LVL 05</span>
  <div class="font-semibold text-gray-800">Container environments <span class="text-gray-400 font-normal">(Docker)</span></div>
  <code class="text-gray-500">FROM python:3.9</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ Base image and apt-get are non-deterministic.</span>
    <span class="text-green-700">✅ Use declarative specs e.g. <strong>nix</strong>.</span>
  </div>
</div>

<!-- Level 6 -->
<div v-click="8" class="grid grid-cols-[90px_1fr_1fr_1fr] gap-3 items-start rounded px-3 py-2 bg-teal-50 border border-teal-200">
  <span class="font-mono font-bold text-teal-700 text-[10px] bg-teal-100 border border-teal-300 rounded px-1.5 py-0.5 w-fit">LVL 06</span>
  <div class="font-semibold text-gray-800">Declarative system env <span class="text-gray-400 font-normal">(nix)</span></div>
  <code class="text-gray-500">buildInputs = [<br/>  pythonEnv pkgs.git<br/>  pkgs.zlib<br/>];</code>
  <div class="flex flex-col gap-0.5">
    <span class="text-red-600">❌ Availability of source code of packages.</span>
    <span class="text-green-700">✅ Long-term archives: <strong>Software Heritage Foundation</strong>.</span>
  </div>
</div>

<!-- Beyond -->
<div v-click="10" class="grid grid-cols-[90px_1fr] gap-3 items-center rounded px-3 py-2 bg-purple-50 border border-purple-200">
  <span class="font-mono font-bold text-purple-700 text-[10px] bg-purple-100 border border-purple-300 rounded px-1.5 py-0.5 w-fit">BEYOND</span>
  <div class="text-gray-600">Long-term archive of packages + hardware environment. Also env vars and sources of non-determinism like random number seeds.</div>
</div>

</div>

<!-- SCREEN OVERLAYS FOR DETAILS-->
<div v-if="$clicks===4 || $clicks===7 || $clicks===9" 
     class="absolute inset-0 m-auto w-[90%] h-[85%] z-50 p-8 shadow-2xl rounded-xl border-t-8 flex flex-col items-center justify-center bg-white dark:bg-gray-800"
     :class="{
       'border-yellow-500': $clicks === 4,
       'border-green-500': $clicks === 7,
       'border-green-600': $clicks === 9
     }">

  <!-- Content for Level 3 -->
  <div v-if="$clicks === 4" class="grid grid-cols-2 gap-4">

  <div>
  requirements.txt — what is declared
```text
  django==6.0.2
  numpy==2.4.2
  pandas==3.0.1
  pipdeptree==2.31.0
```
  pipdeptree — what actually gets installed
```text
  Django==6.0.2
  ├── asgiref [required: >=3.9.1, installed: 3.11.1]
  └── sqlparse [required: >=0.5.0, installed: 0.5.5]
  pandas==3.0.1
  ├── numpy [required: >=1.26.0, installed: 2.4.2]
  └── python-dateutil [required: >=2.8.2, installed: 2.9.0.post0]
    └── six [required: >=1.5, installed: 1.17.0]
  pipdeptree==2.31.0
  ├── packaging [required: >=26, installed: 26.0]
  └── pip [required: >=25.2, installed: 26.0.1]
```
  </div>

  <div>
  uv.lock — what a lockfile captures
```text
...
[[package]]
name = "asgiref"
version = "3.11.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/...", hash = "sha256:5f18...", size = 38550 }
wheels = [
  { url = "https://files.pythonhosted.org/...", hash = "sha256:e866...", size = 24345 },
]
[[package]]
name = "django"
version = "6.0.2"
source = { registry = "https://pypi.org/simple" }
dependencies = [
  { name = "asgiref" },
  { name = "sqlparse" },
  { name = "tzdata", marker = "sys_platform == 'win32'" },
]
sdist = { url = "https://files.pythonhosted.org/...", hash = "sha256:3046...", size = 10886874 }
wheels = [
  { url = "https://files.pythonhosted.org/...", hash = "sha256:610d...", size = 8339381 },
]
...
```
</div>
  </div>
  
  <!-- Content for Level 5 -->
  <div v-if="$clicks === 7" class="flex flex-col gap-1 w-full h-full">

  <div>
    <h2 class="text-lg font-bold text-gray-800">Dockerfiles are not reproducible by default</h2>
  </div>

  <div class="grid grid-cols-2 gap-6 flex-1 text-xs">
    <div class="flex flex-col gap-3">
      <p class="font-semibold text-red-600 uppercase tracking-wide text-[10px]">Problematic Dockerfile</p>
```dockerfile
FROM python:3.9
RUN apt-get update && apt-get install -y libblas-dev
COPY . .
RUN pip install pandas==2.1.0
```
      <div class="flex flex-col gap-1.5">
        <div class="flex gap-2 p-2 rounded border border-red-200 bg-red-50 text-red-800">
          <span class="shrink-0 font-mono font-bold">FROM</span>
          <span><code>python:3.9</code> is a moving tag — it gets updated silently every time Python releases a patch</span>
        </div>
        <div class="flex gap-2 p-2 rounded border border-red-200 bg-red-50 text-red-800">
          <span class="shrink-0 font-mono font-bold">RUN</span>
          <span><code>apt-get install libblas-dev</code> with no version fetches whatever Debian serves today</span>
        </div>
        <div class="flex gap-2 p-2 rounded border border-red-200 bg-red-50 text-red-800">
          <span class="shrink-0 font-mono font-bold">pip</span>
          <span>No lockfile — transitive dependencies float freely</span>
        </div>
      </div>
    </div>
    <div class="flex flex-col gap-3">
      <p class="font-semibold text-green-700 uppercase tracking-wide text-[10px]">Improved Dockerfile</p>
```dockerfile
FROM python:3.9.18@sha256:c3b1...
RUN apt-get update && apt-get install -y \
    libblas-dev=3.11.0
COPY poetry.lock pyproject.toml ./
RUN poetry install --no-root
```
      <div class="flex flex-col gap-1.5">
        <div class="flex gap-2 p-2 rounded border border-green-200 bg-green-50 text-green-800">
          <span class="shrink-0 font-mono font-bold">FROM</span>
          <span>Pinned to an immutable digest — this exact image, forever</span>
        </div>
        <div class="flex gap-2 p-2 rounded border border-green-200 bg-green-50 text-green-800">
          <span class="shrink-0 font-mono font-bold">RUN</span>
          <span><code>libblas-dev=3.11.0</code> — explicit system package version</span>
        </div>
        <div class="flex gap-2 p-2 rounded border border-green-200 bg-green-50 text-green-800">
          <span class="shrink-0 font-mono font-bold">poetry</span>
          <span>Installs from a lockfile — every transitive dependency is pinned and verified</span>
        </div>
      </div>
    </div>

  </div>

  <div class="p-2 rounded border border-gray-200 bg-gray-50 text-gray-600 text-xs">
    Docker can be made more reproducible, but it requires deliberate effort.
  </div>

</div>
  
  <!-- Content for Level 5 -->
  <div v-if="$clicks === 9" class="grid grid-cols-2 gap-6 h-full">

<div>
```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };
  outputs = { self, nixpkgs, flake-utils }:
    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = import nixpkgs { inherit system; };
      in {
        devShells.default = pkgs.mkShell {
          buildInputs = with pkgs; [
            git
            docker-client
            nodejs_25
            python313Packages.uv
            kubectl
          ];
        };
      });
}
```

</div>

<div class="flex flex-col justify-center gap-4 text-sm">

<div>
  <h3 class="text-base font-bold text-gray-800 mb-1">Declarative System Environment with Nix</h3>
  <p class="text-gray-600"><strong>Nix</strong> is a purely functional package manager. Instead of mutating global state, every package lives in its own isolated path in <code>/nix/store</code>.</p>
</div>

<div>
  <h4 class="font-semibold text-gray-700 mb-2 uppercase tracking-wide text-xs">Advantages</h4>
  <div class="flex flex-col gap-1.5">
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">Reproducible</span><span class="text-gray-600">The same nix file produces identical environments on any machine</span></div>
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">Declarative</span><span class="text-gray-600">The entire dev environment is code, version-controlled with your project</span></div>
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">No conflicts</span><span class="text-gray-600">Multiple versions of the same tool coexist without clashing</span></div>
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">Cross-platform</span><span class="text-gray-600"><code>eachDefaultSystem</code> handles Linux & macOS automatically</span></div>
  </div>
</div>

</div>

</div>

</div>

<!--
This slide walks through the reproducibility ladder — six levels, each one addressing a specific failure mode that the previous level leaves open. Let's go through them one by one.

[CLICK - LVL 01]
The lowest level is natural language. A README that says "this project requires Python 3.10 and numpy" is better than nothing, but it is not executable — it relies on the reader correctly interpreting and manually recreating the environment. What version of numpy? Which OS? What happens when numpy releases a new major version next month? This is where most research code still lives.

[CLICK - LVL 02]
A step up is a manifest file — a requirements.txt or similar. Now installation is at least automated. But if you just list "pandas" without a version, pip will grab whatever is latest on the day someone runs it. That may be a completely different version than what you used, and it can silently change behaviour.

[CLICK - LVL 03]
Pinning top-level versions is better — pandas==2.1.0 means you always get pandas 2.1.0. But pandas itself has dependencies, and those are not pinned. The transitive graph — everything that pandas pulls in — still floats freely.

[CLICK - show lockfile detail overlay]
This is what a lockfile solves. On the left you can see a simple requirements.txt with four packages. Run pipdeptree on it and you see the full dependency graph — asgiref, sqlparse, numpy, python-dateutil, six, and more — none of which were in the original file. On the right is a uv.lock — it captures every package in the graph, its exact version, its source registry, and a cryptographic hash. If the hash doesn't match, installation fails. This is reproducibility at the Python level.

[CLICK - LVL 04]
Level 4 is locked dependencies. The full graph is pinned and verified. This is a significant improvement — but it only covers Python packages. System-level dependencies like libblas, glibc, or CUDA are completely outside the picture. Those are installed on the host machine, vary between systems, and can absolutely affect numerical results.

[CLICK - LVL 05]
Containers like Docker address this. You package not just the Python environment but the entire OS userspace — the system libraries, the runtime, everything. In principle this should give you a reproducible environment anywhere Docker runs.

[CLICK - show Docker detail overlay]
In practice, Dockerfiles require careful discipline. A naive Dockerfile with "FROM python:3.9" actually moves — that tag gets updated every time Python releases a patch. apt-get install without pinning fetches whatever Debian has today. pip install without a lockfile floats. The improved version on the right pins the base image to an immutable digest, pins the apt package version, and uses a lockfile for Python dependencies. It works, but it is fragile by default — you have to know to do all of this.

[CLICK - LVL 06]
Nix takes a fundamentally different approach. Instead of imperatively building an environment that can drift, you declare it as a pure function. Every package in the Nix store is identified by a cryptographic hash of everything that produced it — the source, the compiler, the flags. Nothing is ever mutated. The same flake always produces the same environment, on any machine, on any day.

[CLICK - show Nix detail overlay]
The flake on the left is a complete, reproducible dev environment. You declare your inputs — a pinned nixpkgs commit — and describe your shell. Anyone runs `nix develop` and gets exactly this: same git, same Node, same Python tooling, same system libraries. The key properties are reproducibility across machines, no conflicts because nothing is global, and a fully declarative environment that lives in version control alongside the code. The environment is the code.

The remaining open problem is the long-term availability of the packages themselves. Nixpkgs pins to a specific commit, but if upstream source repositories disappear, you still cannot rebuild. That is what the next level — long-term archiving — addresses.

[CLICK - BEYOND]
Beyond the technical environment, there are further sources of non-determinism that even Nix does not solve: hardware differences, environment variables, and non-deterministic algorithms — particularly relevant in ML where random seeds, GPU floating point behaviour, and distributed training can all introduce variance. Full reproducibility ultimately also requires hardware documentation and careful seed management.
-->

---

# Dependency Management in Scientific Software Github Repositories

<div class="flex flex-col h-full gap-2">

<div class="text-sm leading-relaxed">

- Conducted a **GitHub search** spanning **2020–2025** for research software projects
- Targeted Python repositories from popular CS conferences: **ICSE, ICML, NeurIPS, ASE**, and others
- Retrieved using the query `conf_name + year + language:python`

</div>

<div class="flex flex-row flex-1 gap-6 items-start justify-center mt-2">
  <img
    src="/dependency_pinning_all_conferences.png"
    class="flex-1 min-w-0 h-auto max-h-72 object-contain rounded shadow-md"
  />
  <img
    src="/lockfile_dockerfile_availability.png"
    class="flex-1 min-w-0 h-auto max-h-72 object-contain rounded shadow-md"
  />
</div>

</div>


<!--
To understand the state of reproducibility in scientific software, we searched GitHub for Python repositories linked to papers from major CS conferences between 2020 and 2025.

The left chart shows that a surprisingly high share of these repos have either undeclared or unpinned dependencies. Undeclared means there is no dependency specification at all — no requirements.txt, no pyproject.toml, nothing. Unpinned means dependencies are listed but without version constraints, so anyone running the code tomorrow may get a completely different version of a library than the one the authors used. Both cases make it essentially impossible to guarantee the same execution environment.

The right chart is even more telling — virtually no repositories use lockfiles or Dockerfiles. These are the two most accessible tools for guaranteeing a reproducible environment. A lockfile freezes the exact resolved versions of every dependency in the tree. A Dockerfile packages the entire runtime. The near-zero adoption across thousands of repos from top-tier venues is a striking result.

What is perhaps most interesting is that this is not a niche or obscure set of repositories — these are codebases associated with peer-reviewed papers at ICSE, ICML, NeurIPS, ASE, and similar venues. These are researchers who presumably care about scientific rigour, yet the software artifacts they publish are largely not reproducibly executable.

We can also break these numbers down per conference and per year — which reveals whether certain venues or time periods are better or worse, and whether the situation has improved at all over the five years we studied. Spoiler: the trend is not particularly encouraging.

The takeaway from these charts is simple: dependency mismanagement is the norm, not the exception, in scientific software — and that has direct consequences for the reproducibility of scientific results.
-->


---

# NFDIxCS - The Research Data Management Container (RDMC)

<div class="grid grid-cols-2 gap-8 mt-4">
<div class="flex flex-col justify-center space-y-3">

**Central object: Research Data Management Containers (RDMC)** — time capsules for research data and software

- Bundles software, data, and metadata
- FAIR by design – metadata is openly available
- Manages access and workflows
- Container will be archived

</div>
<div class="flex items-center justify-center">
<img src="/RDMC.png" class="w-64 h-auto rounded shadow-md"/>
</div>
</div>

<!--
This talk sits within the NFDIxCS project — the National Research Data Infrastructure for Computer Science. The central concept in NFDIxCS is the Research Data Management Container, or RDMC.

The RDMC is designed as a time capsule — a structured artifact that bundles together everything needed to understand and reuse a piece of research: the data, the software, and the metadata that describes both. The idea is that when a researcher publishes results, they do not just share a paper or a GitHub link — they deposit an RDMC that captures the full research context.

FAIR by design means that the metadata layer of the RDMC is always openly accessible, even when the underlying data or software may be restricted. This separates discoverability from access — you can find out what exists and how to request it, even if the content itself is not publicly available. This is critical for sensitive research domains like medical data or proprietary datasets.

The container also manages access control and workflows — who can see what, under which conditions, and how the data or software can be used. And crucially, the container is archived — it is not just a live repository link that can disappear, but a preserved artifact intended to remain accessible over the long term.

The image on the right shows the layered structure: data at the core, wrapped in software and context, with access control and workflow management on top.
-->

---

# Reusable Execution Environment (REE)

<img src="/RDMC_and_REE.png"/>

<!--
This slide zooms into one specific component of the RDMC — the Reusable Execution Environment, or REE.

The diagram shows the relationship between the RDMC and the REE. The RDMC lives on the NFDIxCS platform and archives — it is the top-level container that holds everything. When a researcher wants to actually work with the research artifacts, they download the RDMC and unpack its content. That unpacking process produces a Reusable Execution Environment — the concrete, runnable environment in which the research software can be executed.

The REE is referenced from within the RDMC rather than embedded directly — this is an important design decision. The execution environment can be large, it may need to be rebuilt on different hardware, and it may evolve independently of the data. By keeping it as a reference with a build script and validation script, the RDMC stays lightweight and declarative while the REE remains reproducible on demand.

The example referenced at the bottom — Blessing et al., 2025, on WiKoDa — is a concrete case study of this in practice: a research dataset with a fully specified REE that can be built and validated by anyone with access to the RDMC.

This is the context in which everything else in this talk sits. The reproducibility levels, the dependency management problems, the Nix and Docker approaches — all of these are about what it takes to actually make a REE that works. The RDMC provides the archival and metadata framework; the REE provides the executable artifact inside it.
-->

---
layout: default
---

# A Model for Reusable Execution Environments

<style>
.field-grid { display: grid; grid-template-columns: 160px 1fr; gap: 0.5rem; align-items: start; }
.example-grid { display: grid; grid-template-columns: 150px 1fr; gap: 0.5rem; align-items: start; }
</style>

<div class="grid grid-cols-2 gap-6 mt-2">

<div>
<p class="text-xs text-gray-400 uppercase tracking-wide font-semibold mb-2">Fields</p>
<div class="flex flex-col gap-1.5 text-xs font-mono">

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">swhid</span>
<span class="text-gray-500 font-sans">SoftWare Hash IDentifier — persistent archive reference of the source code</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">source_repository</span>
<span class="text-gray-500 font-sans">URL of the source code repository</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">runtime</span>
<span class="text-gray-500 font-sans">Path to a pre-built runtime artifact, if available</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-amber-50 border border-amber-300">
<span class="font-bold text-amber-800">build_runtime_script</span>
<span class="text-gray-700 font-sans">Builds the environment from scratch</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">sbom</span>
<span class="text-gray-500 font-sans">Software Bill of Materials — full package inventory</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">hardware_description</span>
<span class="text-gray-500 font-sans">CPU, GPU, memory — documents hardware environment</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-amber-50 border border-amber-300">
<span class="font-bold text-amber-800">validation_script</span>
<span class="text-gray-700 font-sans">Verifies rebuilt environment is runnable</span>
</div>

</div>
</div>

<div>
<p class="text-xs text-gray-400 uppercase tracking-wide font-semibold mb-2">Examples</p>
<div class="flex flex-col gap-1.5 text-xs font-mono overflow-y-auto" style="max-height: 340px;">

<div v-click="1" class="example-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-400">swhid</span>
<span class="text-gray-700">swh:1:dir:4a6b3c...</span>
</div>

<div v-click="2" class="example-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-400">source_repository</span>
<span class="text-gray-700">github.com/org/my-research-software</span>
</div>

<div v-click="3" class="example-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-400">runtime</span>
<span class="text-gray-700">./envs/docker-image.tar.gz</span>
</div>

<div v-click="4" class="px-2 py-1.5 rounded bg-amber-50 border border-amber-300">
<span class="font-bold text-amber-700 font-mono">build_runtime_script</span>
<pre class="mt-1 text-gray-700 font-mono text-[10px] bg-white border border-amber-100 rounded p-2 leading-relaxed">#!/bin/bash
docker build \
  -t my-research-env:1.0.0 \
  -f ./Dockerfile \
  --no-cache .</pre>
</div>

<div v-click="5" class="px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-400 font-mono">sbom</span>
<pre class="mt-1 text-gray-700 font-mono text-[10px] bg-white border border-gray-100 rounded p-2 leading-relaxed">{
  "spdxVersion": "SPDX-2.3",
  "packages": [
    {
      "name": "numpy",
      "versionInfo": "1.26.4",
      "checksums": [
        { "algorithm": "SHA256",
          "checksumValue": "a3f4e2..." }
      ]
    }
  ]
}</pre>
</div>

<div v-click="6" class="example-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-400">hardware_description</span>
<span class="text-gray-700 font-sans leading-relaxed">
  <span class="block">cpu: Intel Xeon E5-2690 v4</span>
  <span class="block">gpu: NVIDIA A100 80GB</span>
</span>
</div>

<div v-click="7" class="px-2 py-1.5 rounded bg-amber-50 border border-amber-300">
<span class="font-bold text-amber-700 font-mono">validation_script</span>
<pre class="mt-1 text-gray-700 font-mono text-[10px] bg-white border border-amber-100 rounded p-2 leading-relaxed">#!/bin/bash
docker load -i ./envs/docker-image.tar.gz
docker run --rm my-research-env:1.0.0
if [ &#36;? -ne 0 ]; then
  echo "FAIL: runtime could not be reproduced"
  exit 1
fi
echo "OK: runtime validated successfully"</pre>
</div>

</div>
</div>

</div>

<!--
So far we have diagnosed the problem — most scientific software lacks the environment information needed to reproduce it. This slide proposes a concrete, minimal data model to address that.

The idea is simple: a structured metadata object that captures everything needed to describe, rebuild, and validate a research execution environment. Let's walk through the fields.

[CLICK - swhid]
The SWHID — SoftWare Hash IDentifier — is a persistent reference provided by the Software Heritage Foundation, which archives all public source code. Unlike a GitHub URL, which can disappear or change, a SWHID is a content-addressed identifier that will resolve to the exact same code permanently. This solves the long-term availability problem we saw at the top of the reproducibility ladder.

[CLICK - source_repository]
The source repository is the live URL where the code lives — GitHub, GitLab, or any other host. Combined with the SWHID, you have both a convenient current reference and a permanent archival one.

[CLICK - runtime]
The runtime field is optional — it points to a pre-built artifact like a Docker image tarball if one is available. This is a convenience for users who want to skip the build step, but it is not required.

[CLICK - build_runtime_script]
This is one of the two required fields. The build script must be able to reconstruct the environment entirely from scratch — here using docker build with --no-cache to ensure no cached state bleeds in. The key design decision is that this field is not optional. You cannot instantiate this object without providing a way to rebuild the environment.

[CLICK - sbom]
The Software Bill of Materials is a machine-readable inventory of every package in the environment — name, version, and a cryptographic checksum for each one. The SPDX format shown here is an open standard generated by tools like syft. The SBOM is what makes validation possible — it is the ground truth the environment must match.

[CLICK - hardware_description]
Hardware matters more than people realise — particularly for GPU workloads and numerical computation. This field documents the CPU, GPU, and memory so that anyone trying to reproduce results knows what they are targeting, and can flag if hardware differences may explain any discrepancies.

[CLICK - validation_script]
The second required field. The validation script loads the runtime artifact and runs it — here just verifying the container starts successfully. In practice this could be extended to run a small smoke test or benchmark. The point is that reproducibility is not just a claim — it is a check that must pass. The object enforces this structurally.

The broader idea is that by making these two scripts mandatory, we shift reproducibility from a social norm — "you should document your environment" — to a structural requirement. If you cannot build and validate the environment, the object is incomplete.
-->

---
layout: default
---

# Reusable Execution Environment — Services

<style>
.field-grid { display: grid; grid-template-columns: 160px 1fr; gap: 0.5rem; align-items: start; }
</style>

<div class="grid grid-cols-2 gap-6 mt-2">

<div>
<p class="text-xs text-gray-400 uppercase tracking-wide font-semibold mb-2">Model</p>
<div class="flex flex-col gap-1.5 text-xs font-mono">

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">swhid</span>
<span class="text-gray-500 font-sans">swh:1:dir:4a6b3c...</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">source_repository</span>
<span class="text-gray-500 font-sans">github.com/org/my-research-software</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">runtime</span>
<span class="text-gray-500 font-sans">./envs/docker-image.tar.gz</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">build_runtime_script</span>
<span class="text-gray-500 font-sans">./scripts/build_env.sh</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">sbom</span>
<span class="text-gray-500 font-sans">./sbom.spdx.json</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">hardware_description</span>
<span class="text-gray-500 font-sans">cpu: Xeon E5 / gpu: A100</span>
</div>

<div class="field-grid px-2 py-1.5 rounded bg-gray-50 border border-gray-200">
<span class="font-bold text-gray-800">validation_script</span>
<span class="text-gray-500 font-sans">./scripts/validate_env.sh</span>
</div>

</div>
</div>

<div>
<p class="text-xs text-gray-400 uppercase tracking-wide font-semibold mb-2">Operations</p>
<div class="grid grid-cols-2 gap-3">

<div v-click="1" class="rounded-lg bg-blue-600 p-4 flex items-center justify-center cursor-pointer">
  <p class="text-sm font-bold text-white text-center">Create REE</p>
</div>

<div v-click="2" class="rounded-lg bg-purple-600 p-4 flex items-center justify-center cursor-pointer">
  <p class="text-sm font-bold text-white text-center">Evaluate Reproducibility Level</p>
</div>

<div v-click="3" class="rounded-lg bg-amber-500 p-4 flex items-center justify-center cursor-pointer">
  <p class="text-sm font-bold text-white text-center">Build Runtime</p>
</div>

<div v-click="4" class="rounded-lg bg-green-600 p-4 flex items-center justify-center cursor-pointer">
  <p class="text-sm font-bold text-white text-center">Validate Runtime</p>
</div>

<div v-click="6" class="rounded-lg bg-slate-500 p-4 flex items-center justify-center cursor-pointer">
  <p class="text-sm font-bold text-white text-center">Upload to Software Heritage</p>
</div>

</div>
</div>

</div>

<!--
This slide shows what a concrete interface on top of the REE model could look like. On the left is the model we just defined — populated with real values. On the right are the operations a researcher or tool would perform against it.

[CLICK - Create REE]
The starting point — a researcher fills in the metadata fields and registers a new REE object. This is the moment the environment becomes a first-class artifact in the system.

[CLICK - Evaluate Reproducibility Level]
Given the fields that are present, the system can automatically assess which level of the reproducibility ladder this environment reaches — does it have a lockfile? A container? A declarative spec? This gives researchers immediate, actionable feedback on what is missing.

[CLICK - Build Runtime]
Triggers the build_runtime_script — constructing the environment from scratch. The system executes the script and returns build logs and the resulting runtime artifact.

[CLICK - Validate Runtime]
Runs the validation_script against the built runtime and returns a pass or fail. This is the key enforcement step — reproducibility is not a claim, it is a check.

[CLICK - Upload to Software Heritage]
Triggers an archival deposit of the source code directly to Software Heritage, ensuring long-term availability even if the original repository is deleted or moved.

The overall idea is that these six operations cover the full lifecycle — from registration, through assessment and building, to validation and permanent archival. Each one maps directly onto a field or requirement in the model.
-->

---

# Conclusion