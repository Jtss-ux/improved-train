# AMD Hardware Developer Challenge — PR Instructions & Submission

## What’s Already Done for You

### 1. **Bug research document**
`AMD_Hardware_Challenge_ROCm_Bugs.md` — Curated list of 34 ROCm bugs to fix.

### 2. **Implemented fix (ready to push)**

**build_amd.py** (`pytorch/tools/amd_build/build_amd.py`) — Fixes [#175160](https://github.com/pytorch/pytorch/issues/175160) & [#175180](https://github.com/pytorch/pytorch/issues/175180)

- Added `mslk_original.exists()` before adding to `extra_files`
- Added `mslk_move_src.exists()` before the file move block  
- Avoids `FileNotFoundError` when MSLK submodule is missing (e.g. Fedora builds)

---

## Step-by-Step: Push and Open PRs

### 1. Fork PyTorch

1. Open https://github.com/pytorch/pytorch
2. Click **Fork**
3. Select your GitHub account

### 2. Clone your fork (short path to avoid Windows path length issues)

```powershell
# Clone to a short path, e.g. C:\pytorch
cd C:\
git clone https://github.com/YOUR_USERNAME/pytorch.git
cd pytorch
git remote add upstream https://github.com/pytorch/pytorch.git
git fetch upstream
git checkout -b fix-rocm-build-mslk upstream/main
```

### 3. Apply the build_amd.py fix

Copy the modified file into your clone:

```powershell
copy "C:\Users\JTS\OneDrive\Desktop\picoclaw-main\pytorch\tools\amd_build\build_amd.py" "C:\pytorch\tools\amd_build\build_amd.py"
```

### 4. Commit and push

```powershell
cd C:\pytorch
git add tools/amd_build/build_amd.py
git commit -m "[ROCm] Check if file exists before hipifying MSLK sources

Adds Path.exists() checks before accessing MSLK submodule files to prevent
FileNotFoundError when the submodule is not initialized (e.g. Fedora packaging).

Fixes #175160
Fixes #175180"
git push origin fix-rocm-build-mslk
```

### 5. Open the PR

1. Go to your fork: `https://github.com/YOUR_USERNAME/pytorch`
2. Click **Compare & pull request**
3. Base: `pytorch/main` ← head: `YOUR_USERNAME:fix-rocm-build-mslk`
4. Title: `[ROCm] Check if file exists before hipifying MSLK sources`
5. Body (example):

```
## Description
Adds Path.exists() checks before accessing MSLK submodule files in build_amd.py.
Prevents FileNotFoundError when optional submodules (e.g. MSLK) are not initialized,
such as in Fedora packaging builds.

## Fixes
Fixes #175160
Fixes #175180

## Test plan
Verified that the script no longer raises when third_party/mslk is missing.
```

6. Submit the PR

---

## Reaching 10 Merged PRs

The build fix covers 1–2 bugs (depending on how they count). You need 10 bugs in total.

### Approach A: More ROCm fixes (recommended)

1. **TF32 tolerance** — #175161, #175162 (PRs open by others; pick different issues)
2. **Test unskips** — See `rocm-skipped-tests` in the bug doc
3. **hip_platform_files** — Add `os.path.exists()` for each file before processing (similar to build_amd.py)

### Approach B: Batch similar fixes

Group 4–5 SDPA test unskips into one PR:

- #168876, #168877, #168875, #168873 (TestSDPACudaOnly)

### Approach C: vLLM instead

Use [vLLM ROCm issues](https://github.com/vllm-project/vllm/issues?q=is%3Aissue+is%3Aopen+label%3Arocm) and apply the same workflow.

---

## Submission Template (copy when you have 10 merged PRs)

Paste this into the challenge box:

```
AMD Hardware Developer Challenge - PyTorch ROCm Bug Fixes

1. https://github.com/pytorch/pytorch/pull/XXXXX - Fixes #175160, #175180
2. https://github.com/pytorch/pytorch/pull/YYYYY - Fixes #issue2
3. https://github.com/pytorch/pytorch/pull/ZZZZZ - Fixes #issue3
4. https://github.com/pytorch/pytorch/pull/AAAAA - Fixes #issue4
5. https://github.com/pytorch/pytorch/pull/BBBBB - Fixes #issue5
6. https://github.com/pytorch/pytorch/pull/CCCCC - Fixes #issue6
7. https://github.com/pytorch/pytorch/pull/DDDDD - Fixes #issue7
8. https://github.com/pytorch/pytorch/pull/EEEEE - Fixes #issue8
9. https://github.com/pytorch/pytorch/pull/FFFFF - Fixes #issue9
10. https://github.com/pytorch/pytorch/pull/GGGGG - Fixes #issue10
```

Replace the URLs with your actual merged PR links.

---

## Reference Links

- [PyTorch CONTRIBUTING](https://github.com/pytorch/pytorch/blob/main/CONTRIBUTING.md)
- [PyTorch ROCm Project Board](https://github.com/orgs/pytorch/projects/146/views/1)
- [vLLM ROCm Issues](https://github.com/vllm-project/vllm/issues?q=is%3Aissue+is%3Aopen+label%3Arocm)
