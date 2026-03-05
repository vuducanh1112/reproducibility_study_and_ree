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

<!-- SLIDE: Cover -->
<!--
Good morning everyone. I'm going to talk about reproducibility in scientific software repositories and about one part of our work here at NFDIxCS: the Reusable Execution Environment.
-->

---
layout: default
---

# Sharing Code ≠ Sharing Science

<div class="flex flex-col h-full gap-4">

<div class="grid grid-cols-3 gap-4 mt-2">

<div class="border border-red-300 rounded-lg p-4 bg-red-50">
<h3 class="font-bold text-red-600 mb-1">Software Decay</h3>
<p class="text-sm text-gray-700">Code that worked at publication often fails 6 months later due to silent breakage as the ecosystem evolves around it.</p>
</div>

<div class="border border-yellow-300 rounded-lg p-4 bg-yellow-50">
<h3 class="font-bold text-yellow-600 mb-1">Undocumented Environments</h3>
<p class="text-sm text-gray-700">The environment, OS version, compiler flags, libraries, runtime configuration and hardware are often undocumented.</p>
</div>

<div class="border border-orange-300 rounded-lg p-4 bg-orange-50">
<h3 class="font-bold text-orange-600 mb-1">Non-Determinism</h3>
<p class="text-sm text-gray-700">Sources of randomness like parallel executions lead to non-determinism. The <strong>same code may produce different results</strong> across runs.</p>
</div>

</div>

<div class="p-4 rounded-lg border border-blue-200 bg-blue-50">
  <p class="text-sm font-bold text-blue-800 mb-1">"No Data Without Software" — NFDIxCS</p>
  <p class="text-sm text-gray-700">The <strong>FAIR principles</strong> of research data need to also apply to the software that generates and interprets it. Research management must treat the <strong>software and its execution environment as a primary research artifact</strong>, not an afterthought.</p>
</div>

</div>

<!-- SLIDE: Sharing Code != Sharing Science -->
<!--
"Works on my machine" — an expression we probably are all familiar with. The effects of that are especially felt in research, where code is routinely written under deadline pressure, published, and then abandoned.

So simply sharing code is not the same as sharing science, as the reproducibility may suffer from a multitude of reasons.

First, even if the code doesn't change — the world around it does. Libraries release breaking changes, and so, code that ran perfectly at submission may be completely broken six months later with no changes to the repository.

Second, undocumented environments. A paper may describe every equation and hyperparameter, but then say nothing about the OS, the compiler flags, the library versions, or the hardware. That context lives implicitly on the author's machine and almost never makes it into the repository.

And third, non-determinism. Parallel operations like GPU matrix multiplications have non-deterministic execution order. Seeds for random number generators need to be fixed. The same code on the same machine can produce completely different results across runs.

One of the NFDIxCS mottos is "No Data Without Software." The FAIR principles were designed for research data, but data does not exist without the software that produced it. Research management needs to treat the execution environment as a primary research artifact.
-->



---
layout: default
---

# The Reproducibility Crisis — By The Numbers
<div class="flex flex-col h-full gap-4 mt-2">
<div class="grid grid-cols-2 gap-4">

<div class="rounded-lg p-4 text-center border border-red-200 bg-red-50">
<div class="text-4xl font-bold text-red-600">70%</div>
<p class="text-sm mt-1 text-gray-700">of 1,576 researchers <strong>failed to reproduce</strong> a published result</p>
<p class="text-xs text-gray-400 mt-2">Nature survey — Baker 2016</p>
</div>

<div class="rounded-lg p-4 text-center border border-orange-200 bg-orange-50">
<div class="text-4xl font-bold text-orange-600">13.7%</div>
<p class="text-sm mt-1 text-gray-700">of Jupyter notebooks <strong>declared their dependencies</strong> — installation failure rates were <strong>61–67%</strong></p>
<p class="text-xs text-gray-400 mt-2">Pimentel et al., MSR 2019</p>
</div>

</div>

<div class="border border-gray-200 rounded-lg p-4 text-sm bg-gray-50">
<p class="text-gray-700"><strong class="text-red-600">87.6%</strong> of biomedical notebooks threw exceptions on re-execution. The format designed for reproducibility fails in practice.</p>
<p class="text-gray-400 text-xs mt-1">Samuel & Mietchen, GigaScience 2024</p>
</div>

</div>

<!-- SLIDE: Reproducibility Crisis -->
<!--
The numbers look concerning across different studies.

Baker's survey in 2016 of over 1,500 researchers found that 70% had failed to reproduce a published result.

Computational notebooks were supposed to be the gold standard of executable, shareable research. However, in a study in 2019, less than 14% declared their dependencies at all. And for those that did, installation failed more than 60% of the time. In a more recent study, nearly 90% of biomedical notebooks crashed on re-execution with import or file errors.

This seems widespread and it sets up the core question: what does it take to actually guarantee a reproducible environment?
-->

---

# Dependency Management in Scientific Software Github Repositories

<div class="flex flex-col h-full gap-2">

<div class="text-sm leading-relaxed">

- Conducted a **GitHub search** spanning **2020–2025** for research software projects
- Targeted Python repositories from popular CS conferences: **ICSE, ICML, NeurIPS, ASE**, and others
- Retrieved using the query `conf_name + year + language:python`

</div>

<div class="flex flex-row flex-1 gap-6 items-start justify-center mt-1">
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


<!-- SLIDE: Dependency Management -->
<!--
We also did our own little study, so we searched GitHub for research Python repositories and looked how they manage their software dependencies.Specifically we looked for repos associated with papers from major Computer Science conferences — ICSE, ICML, NeurIPS, ASE, and others — spanning 2020 to 2025.

The left chart shows the stats on specifying the versions of the dependencies. We still see a lot of grey — too many repositories declare no dependencies at all in a typical configuration file. The blue and gold segments are relatively balanced; there is much room for improvements for researchers to accurately specify their dependencies.

The right chart shows lockfile and Dockerfile availability. Here it looks even worse. Lockfiles and Dockerfiles are essentially absent across hundreds of repositories from top-tier venues.

The problem seems consistent.
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
  <div class="text-gray-600">Long-term archive of packages, hardware environment, sources of non-determinism.</div>
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
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">Declarative</span><span class="text-gray-600">The entire environment is code and can be version-controlled with your project</span></div>
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">No conflicts</span><span class="text-gray-600">Multiple versions of the same tool coexist without clashing</span></div>
    <div class="flex gap-2"><span class="text-teal-600 font-semibold w-28 shrink-0">Cross-platform</span><span class="text-gray-600"><code>eachDefaultSystem</code> handles different architectures automatically</span></div>
  </div>
</div>

</div>

</div>

</div>

<!-- SLIDE: Reproducibility Levels -->
<!--
Ensuring full reproducibility is not an easy task at all. There are levels to it however, so incremental improvements can be achieved. We tried to come up with such reproducibility levels.

[CLICK — LVL 01]
At the bottom: natural language only. A README that says "this project requires Python 3.10 and numpy." Better than nothing, but imprecise and impossible to automate. An improvement is simple: create a formal dependency file that can be used by tools.

[CLICK — LVL 02]
A manifest file like a requirements.txt is a step forward — it names the required packages. But without specifying which version is required, you get whatever PyPI serves today. Run the same install command six months apart and you likely get different packages. So, we should pin the versions.

[CLICK — LVL 03]
Pinning the top-level versions is better, but it leaves transitive dependencies floating. Pandas depends on numpy, which depends on other things. Those are all still unspecified.

[CLICK — lockfile overlay]
This is the hidden problem with pinned requirements. You declare four packages, but pipdeptree reveals the actual install graph. Django pulls in these two packages; pandas pulls in python-dateutil, which pulls in six. None of these are pinned. A lockfile captures the complete dependency graph with cryptographic hashes, making the install more reproducible and verifiable.

[CLICK — LVL 04]
A lockfile typically only covers the language specific packages. It says nothing about system-level dependencies — libblas, glibc, CUDA. These are outside the Python ecosystem, but equally impact the behaviour of the code. Containers or VMs can help with this.

[CLICK — LVL 05]
Docker is probably the most popluar tool. But Docker is not reproducible by default.

[CLICK — Docker overlay]
"FROM python:3.9" is a moving tag — it gets silently updated every time Python releases a patch. "apt-get install libblas-dev" again has no version specified. And of course using pip without a lockfile leaves transitive dependencies floating. These things are fixable — pin to an immutable digest, pin apt package versions, use a lockfile — but it requires deliberate effort.

[CLICK — LVL 06]
Better reproducibility are promised by declarative approaches like nix. Nix is a purely functional package manager — every package lives in its own isolated path, identified by a hash of its inputs. Nix files produce identical environments across different machines without version drifts in the packages.

[CLICK — Nix overlay]
Here is an example of a nix file shown here that declares the exact toolchain — git, docker, node, uv and kubectl — which is easily replicated with all their upstream dependencies. Additionally there are utils to handle cross-platform builds automatically. This nix file can easily version controlled, and others just need to run nix develop to get an identical environment.

[CLICK — BEYOND]
But even Nix doesn't solve everything. The long-term availability of package source code is an open problem — if a package is removed from its registry, even a perfectly specified environment can't be rebuilt. The answer there is long-term archival through the Software Heritage Foundation. And there are additional sources of non-determinism that go beyond package management: environment variables, random seeds, hardware-specific behaviour. These remain open research problems.

Most research software today sits at the lower levels, but luckily there are tools improve on this.
-->

---
layout: default
---
# NFDIxCS — Research Data Management Container & Reusable Execution Environment

<div class="grid grid-cols-2 gap-8 mt-2 h-full">

<div class="flex flex-col gap-4">
<div class="flex gap-4 items-start">
  <img src="/RDMC.png" class="w-36 h-auto rounded shadow-sm shrink-0"/>
  <div class="flex flex-col gap-2">
    <p class="text-xs text-gray-400 uppercase tracking-wide font-semibold">Research Data Management Container (RDMC)</p>
    <p class="text-sm text-gray-700"><strong>Time capsules</strong> for research data and software — bundling data, code, and metadata into a single archived artifact.</p>
  </div>
</div>
<div class="flex flex-col gap-1.5 text-sm">
  <div class="flex gap-2"><span class="text-blue-400 shrink-0">→</span><span class="text-gray-700">FAIR by design — metadata always openly accessible</span></div>
  <div class="flex gap-2"><span class="text-blue-400 shrink-0">→</span><span class="text-gray-700">Manages access control and workflows</span></div>
  <div class="flex gap-2"><span class="text-blue-400 shrink-0">→</span><span class="text-gray-700">Archived for long-term availability</span></div>
</div>
</div>

<div class="flex flex-col gap-4 justify-start">
  <div class="p-3 rounded border border-blue-200 bg-blue-50 text-sm text-gray-700">
    The <strong>Reusable Execution Environment (REE)</strong> is the component inside the RDMC responsible for the executable artifact — unpacked from the container and rebuilt on demand.
  </div>
  <img src="/RDMC_and_REE.png" class="w-full h-[50%] rounded shadow-sm"/>
</div>
</div>

<!-- SLIDE: NFDIxCS -->
<!--
At NFDIxCS this is one of our major concerns we work to address. The Research Data Management Container is our main concept — a time capsule that bundles data, software, and metadata into a single archived object to ensure FAIR research artifacts by design.

The Reusable Execution Environment is a component inside the RDMC that handles the execution of the stored software. The goal is to make it easy to rerun and verify the computational results.

Let me show you the model we have designed for the REE itself.
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

<!-- SLIDE: REE Model -->
<!--
The REE model is a structured metadata object that describe everything needed to rebuild and verify a research execution environment.

[CLICK — swhid]
The software hash id is a persistent identifier by the Software Heritage Foundation. It uniquely describes and identifies the source code.

[CLICK — source_repository]
The source repository is the live URL — GitHub, GitLab, or any host.

[CLICK — runtime]
The runtime field is optional — a pre-built artifact like a Docker image tarball for users who want to skip the build step.

[CLICK — build_runtime_script]
The build script must reconstruct the environment entirely from scratch — here using docker build to create a runnable image.

[CLICK — sbom]
The Software Bill of Materials is a machine-readable inventory of every package in the environment — name, version, and a cryptographic checksum per entry. The SBOM is the ground truth and can be used to validate the reproducibility of the environment. 

[CLICK — hardware_description]
The hardware description field should document the used hardware so that anyone trying to reproduce results knows what they are targeting and can flag if hardware differences may explain discrepancies.

[CLICK — validation_script]
The validation script should verify that the runtime is runnable — here verifying the container starts successfully. The key design point is structural: reproducibility is not a claim made in a README, it is a check that must pass.

This REE model shifts reproducibility from a best practise — "you should document your environment" — to a machine checkable structural requirement.
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

<div class="rounded-lg bg-blue-600 p-4 flex items-center justify-center">
  <p class="text-sm font-bold text-white text-center">Create REE</p>
</div>

<div class="rounded-lg bg-purple-600 p-4 flex items-center justify-center">
  <p class="text-sm font-bold text-white text-center">Evaluate Reproducibility Level</p>
</div>

<div class="rounded-lg bg-amber-500 p-4 flex items-center justify-center">
  <p class="text-sm font-bold text-white text-center">Build Runtime</p>
</div>

<div class="rounded-lg bg-green-600 p-4 flex items-center justify-center">
  <p class="text-sm font-bold text-white text-center">Validate Runtime</p>
</div>

<div class="rounded-lg bg-slate-600 p-4 flex items-center justify-center col-span-2">
  <p class="text-sm font-bold text-white text-center">Upload to Software Heritage</p>
</div>

</div>
</div>

</div>

<!-- SLIDE: REE Services -->
<!--
To that end envision a handful of functionality to manage REEs.

Create REE registers the object and validates the fields and may suggest values in case the user does not have the required information. Evaluate Reproducibility Level inspects the code and the metadata and automatically scores it against our six-level taxonomy — giving researchers immediate feedback on what is missing. Build Runtime executes the build script from scratch. Validate Runtime runs the validation script and returns a pass or fail — reproducibility as a check, not a claim.

Upload to Software Heritage closes the archival loop — depositing the source code long-term and obtaining a SWHID.

Together these operations help create new reproducible environments, and also validate existing REEs.
-->

---
layout: default
---

# Conclusion

<div class="flex flex-col gap-4 h-[85%]">

  <p class="text-xs text-gray-400 uppercase tracking-wide font-semibold">Summary</p>
  <div class="flex gap-4 min-h-0" style="max-height: 55%">
    <img src="/slides/github-study.png" class="flex-1 min-w-0 min-h-0 object-contain rounded shadow-sm border border-gray-100"/>
    <img src="/slides/repro-levels.png" class="flex-1 min-w-0 min-h-0 object-contain rounded shadow-sm border border-gray-100"/>
    <img src="/slides/ree-services.png" class="flex-1 min-w-0 min-h-0 object-contain rounded shadow-sm border border-gray-100"/>
  </div>

  <div class="grid grid-cols-2 gap-6">
    <div class="flex flex-col gap-1.5">
      <p class="text-xs text-gray-400 uppercase tracking-wide font-semibold">Further Open Challenges</p>
      <div class="flex flex-col gap-1 text-xs text-gray-700">
        <div class="flex gap-2"><span class="text-orange-400 shrink-0">→</span><span>Validating execution environments against hardware requirements</span></div>
        <div class="flex gap-2"><span class="text-orange-400 shrink-0">→</span><span>Automatically improving reproducibility levels from underspecified environments</span></div>
        <div class="flex gap-2"><span class="text-orange-400 shrink-0">→</span><span>Build and validation provenance tracking for REE runs</span></div>
      </div>
    </div>
    <div class="flex flex-col gap-1.5">
      <p class="text-xs text-gray-400 tracking-wide font-semibold">VISIT NFDIxCS AND THE SERVICES</p>
      <div class="flex gap-3 items-center">
        <img src="/qrcode-nfdixcs-services.png" class="w-20 h-20 shrink-0 rounded"/>
        <div class="flex flex-col gap-1">
          <p class="text-xs font-mono font-bold text-blue-700">nfdixcs.org/services</p>
        </div>
      </div>
    </div>

  </div>

</div>

<!-- SLIDE: Conclusion -->
<!--
To wrap up — we investigated scientific repositories on their handling of dependencies which strongly indicates their reproducibility, which did not look very promising.

We looked at different levels of reproducibility from a READMEs to a declarative environment management, each one addressing a specific failure mode the level below leaves open. This gives us and researchers a concrete vocabulary to evaluate reproduciblity.

Our model of a Reusable Execution Environment and its services aims to improve this.

There are a lot of challenges that remain open: validating environments against hardware requirements, automatically improving reproducibility levels from underspecified inputs, and tracking provenance across build and validation runs.

You can visit us at nfdixcs.org and check out our services. Thank you very much.
-->