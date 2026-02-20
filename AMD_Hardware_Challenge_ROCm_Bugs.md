# AMD Hardware Developer Challenge — Deep ROCm Bug Investigation

**Target Project:** PyTorch  
**Prize:** HP Strix Halo 128GB Laptop (20 units)  
**Task:** Fix 10 bugs in the PyTorch ROCm backlog, get PRs merged, and submit links for AMD verification.

---

## My Independent Research Methodology

I conducted a comprehensive, multi-dimensional investigation of the PyTorch ROCm backlog using systematic search strategies across different issue categories, labels, and timeframes. This document represents my original findings.

### Research Approach

#### Phase 1: Multi-Label Search Strategy
I searched across multiple module labels to find ROCm-specific issues:

| Search Query | Purpose | Results Found |
|--------------|---------|---------------|
| `is:issue is:open label:"module: rocm"` | Core ROCm issues | 15+ issues |
| `is:issue is:open label:rocm-skipped-tests` | Disabled/skipped tests | 11+ issues |
| `is:issue is:open rocm label:"module: inductor"` | Inductor/compile issues | 12+ issues |
| `is:issue is:open rocm label:"module: build"` | Build system issues | 8+ issues |
| `is:issue is:open rocm label:"module: crash"` | Crash/segfault issues | 10+ issues |
| `is:issue is:open rocm label:"module: performance"` | Performance regressions | 6+ issues |
| `is:issue is:open rocm label:"module: autograd"` | Autograd/backward issues | 8+ issues |
| `is:issue is:open rocm label:"oncall: pt2"` | PyTorch 2.0 compile issues | 10+ issues |
| `is:issue is:open rocm created:>2025-01-01` | Recent issues (2025) | 15+ issues |

#### Phase 2: Filtering Criteria
- **Excluded:** Issues marked `wontfix`, `duplicate`, or with active PRs
- **Prioritized:** Issues with `triaged` label (clear scope)
- **Focused on:** ROCm-specific bugs (not generic CUDA issues)
- **Verified:** Each issue is currently open and actionable

#### Phase 3: Cross-Reference Verification
- Cross-checked against PyTorch ROCm project board
- Verified issue creation dates and last activity
- Confirmed no duplicate entries across different searches

---

## Research Findings: ROCm-Specific Bugs Identified

### Category 1: ROCm Test Unskips (High Priority)

These are tests disabled specifically for ROCm that can be fixed and re-enabled:

| # | Issue | Link | Module | Status |
|---|-------|------|--------|--------|
| 1 | **DISABLED test_index** (DistTensorOpsTest) | [#171119](https://github.com/pytorch/pytorch/issues/171119) | distributed | triaged |
| 2 | **TestLinalg.test_tensorinv** | [#174913](https://github.com/pytorch/pytorch/issues/174913) | linear algebra | triaged |
| 3 | **DISABLED test_decompose_k_custom_op_autotune_dynamic_config_for_input_shape** | [#171519](https://github.com/pytorch/pytorch/issues/171519) | inductor | triaged |
| 4 | **DISABLED test_raw_amdsmi_device_uuids** | [#169881](https://github.com/pytorch/pytorch/issues/169881) | tests | triaged |
| 5 | **DISABLED test_uuid_visible_devices** | [#169878](https://github.com/pytorch/pytorch/issues/169878) | tests | triaged |
| 6 | **DISABLED test_fuzzer_issue_164185** | [#170259](https://github.com/pytorch/pytorch/issues/170259) | fuzzer | triaged |
| 7 | **DISABLED test_opaque_obj_training_ir_to_decomp_nonstrict** | [#170302](https://github.com/pytorch/pytorch/issues/170302) | export | triaged |
| 8 | **DISABLED test_comprehensive_nn_functional_linear_cuda_float32** | [#175368](https://github.com/pytorch/pytorch/issues/175368) | inductor | high priority |
| 9 | **DISABLED test_two_layer_fully_shard_cudagraph** | [#174282](https://github.com/pytorch/pytorch/issues/174282) | distributed | triaged |
| 10 | **DISABLED test_sparse_mul_sparse_cuda_float64** | [#174389](https://github.com/pytorch/pytorch/issues/174389) | sparse | triaged |

### Category 2: ROCm-Specific Crashes & Memory Issues

| # | Issue | Link | Module | Severity |
|---|-------|------|--------|----------|
| 11 | **triu_tril_kernel HSA_STATUS_ERROR_MEMORY_APERTURE_VIOLATION** (MI210/MI300X FP16) | [#174313](https://github.com/pytorch/pytorch/issues/174313) | rocm | triaged |
| 12 | **Segfault on AMD Strix Halo (Radeon 8060S)** with PyTorch ROCm 7.1 / Fedora 43 | [#173707](https://github.com/pytorch/pytorch/issues/173707) | rocm | triaged |
| 13 | **Inductor BF16 training crashes** with autograd INTERNAL ASSERT during backward | [#174884](https://github.com/pytorch/pytorch/issues/174884) | inductor/autograd | triaged |
| 14 | **F.embedding_bag segfaults** with float64 weight and empty offsets | [#175370](https://github.com/pytorch/pytorch/issues/175370) | embedding | triaged |
| 15 | **F.embedding_bag segfaults** when intermediate offsets exceed indices length | [#175372](https://github.com/pytorch/pytorch/issues/175372) | embedding | triaged |

### Category 3: ROCm Build System Issues

| # | Issue | Link | Module | Type |
|---|-------|------|--------|------|
| 16 | **Check if file exists before hipifying** | [#175160](https://github.com/pytorch/pytorch/issues/175160) | build | triaged |
| 17 | **[rocm] Fix build_amd.py** when MSLK submodule is missing | [#175180](https://github.com/pytorch/pytorch/issues/175180) | build | triaged |
| 18 | **[Build Success / ROCm 7.2] Workaround** for undefined symbol errors (const_data_ptr / mutable_data_ptr) | [#173761](https://github.com/pytorch/pytorch/issues/173761) | build | triaged |

### Category 4: ROCm Inductor/Compile Issues

| # | Issue | Link | Module | Priority |
|---|-------|------|--------|----------|
| 19 | **[release 2.11][triton] trunk / linux-jammy-rocm-py3.10** test failures | [#174379](https://github.com/pytorch/pytorch/issues/174379) | inductor | triaged |
| 20 | **[Inductor][PyTorch 2.10] RuntimeError** in native_layer_norm_backward with dynamic shapes | [#174386](https://github.com/pytorch/pytorch/issues/174386) | inductor | triaged |
| 21 | **Incorrect storage offsets propagation** in inductor with as_strided | [#175354](https://github.com/pytorch/pytorch/issues/175354) | inductor | triaged |
| 22 | **Triton OutofMemoryError** when fusing multiple ops | [#175325](https://github.com/pytorch/pytorch/issues/175325) | inductor | triaged |
| 23 | **[Inductor] Missing host-side synchronization** after non-blocking D2H copies | [#174695](https://github.com/pytorch/pytorch/issues/174695) | inductor | high priority |
| 24 | **torch.compile VRAM usage regression** between 2.9.1 and 2.10.0 | [#175064](https://github.com/pytorch/pytorch/issues/175064) | inductor | high priority |

### Category 5: ROCm Performance & Accuracy Issues

| # | Issue | Link | Module | Type |
|---|-------|------|--------|------|
| 25 | **Massive performance regression** on ROCm with certain Conv2d | [#173314](https://github.com/pytorch/pytorch/issues/173314) | performance | triaged |
| 26 | **hipblaslt insufficient MI300 TF32 accuracy** for certain pytorch unit tests | [#169392](https://github.com/pytorch/pytorch/issues/169392) | rocm | triaged |
| 27 | **Increase test_Linear TF32 tolerance** for ROCm | [#175161](https://github.com/pytorch/pytorch/issues/175161) | rocm | triaged |
| 28 | **Increase TransformerEncoderLayer gelu TF32 tolerance** for ROCm | [#175162](https://github.com/pytorch/pytorch/issues/175162) | rocm | triaged |
| 29 | **[ROCm][RDNA3/GFX1100] MIOpen Conv3d** catastrophically fails on large batches with bfloat16 | [#167420](https://github.com/pytorch/pytorch/issues/167420) | performance | triaged |

### Category 6: ROCm Testing Infrastructure

| # | Issue | Link | Module | Type |
|---|-------|------|--------|------|
| 30 | **CUDA/ROCm/Accelerator testing** should replace get_device_capability() with feature queries | [#175250](https://github.com/pytorch/pytorch/issues/175250) | tests | triaged |
| 31 | **TestSDPACudaOnly.test_fused_kernels_nested_broadcasting** | [#168876](https://github.com/pytorch/pytorch/issues/168876) | sdpa | triaged |
| 32 | **TestSDPACudaOnly.test_fused_kernels_nested_broadcasting_query_dense** | [#168877](https://github.com/pytorch/pytorch/issues/168877) | sdpa | triaged |
| 33 | **TestSDPACudaOnly.test_fused_backwards_throws_determinism_warning** | [#168875](https://github.com/pytorch/pytorch/issues/168875) | sdpa | triaged |
| 34 | **TestSDPACudaOnly.test_scaled_dot_product_attention_cudnn_nested** | [#168873](https://github.com/pytorch/pytorch/issues/168873) | sdpa | triaged |

---

## Recommended Strategy for 10 Bug Fixes

Based on my analysis, I recommend focusing on **Category 1 (Test Unskips)** as the primary path:

1. **Batch similar fixes** — Group related test unskips (e.g., all SDPA tests) into single PRs
2. **Start with triaged issues** — These have clear scope and maintainer approval
3. **Mix categories** — Combine 6-7 test unskips with 3-4 build/crash fixes for variety

### Example Fix Plan:
- **PR 1:** Unskip 4 SDPA tests (#168876, #168877, #168875, #168873) → 4 bugs
- **PR 2:** Unskip 3 test issues (#171119, #174913, #171519) → 3 bugs  
- **PR 3:** Fix 2 build issues (#175160, #175180) → 2 bugs
- **PR 4:** Fix 1 crash issue (#174313) → 1 bug
- **Total: 10 bugs across 4 PRs**

---

## Contest Rules

| Rule | Details |
|------|---------|
| **Count** | 10 bugs fixed |
| **Format** | Merged PRs that close issues (use `Fixes #XXXXX` in PR body) |
| **Backlog** | [PyTorch ROCm Project](https://github.com/orgs/pytorch/projects/146/views/1) |
| **Submission** | Paste 10 merged PR links in the challenge submission form |
| **Verification** | AMD Technical expert verifies before laptop claim |

---

## Research Verification

| Date | Research Phase | Methodology | Results |
|------|----------------|-------------|---------|
| Investigation | Multi-label search | Searched 9 different label combinations | 60+ ROCm issues found |
| Filtering | Applied criteria | Excluded duplicates, focused on triaged | 34 actionable bugs identified |
| Categorization | Grouped by type | Organized into 6 categories | Clear fix strategy emerged |
| Prioritization | Analyzed scope | Selected 10+ high-priority candidates | Ready for implementation |

**Research Note:** All issues listed are publicly documented in the PyTorch GitHub repository. This research represents independent identification, filtering, and curation of viable candidates through systematic investigation—not discovery of previously unknown bugs.

---

## Reference Links

- [PyTorch ROCm issues (module: rocm)](https://github.com/pytorch/pytorch/issues?q=is%3Aissue+is%3Aopen+label%3A%22module%3A+rocm%22)
- [PyTorch rocm-skipped-tests](https://github.com/pytorch/pytorch/issues?q=is%3Aissue+is%3Aopen+label%3Arocm-skipped-tests)
- [PyTorch ROCm Project Board](https://github.com/orgs/pytorch/projects/146/views/1)
- [PyTorch CONTRIBUTING guide](https://github.com/pytorch/pytorch/blob/main/CONTRIBUTING.md)

---

*AMD Hardware Developer Challenge — Independent deep investigation completed Feb 20, 2026*
