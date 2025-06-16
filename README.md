# Optimizing SpMV: A Comparative Chunk_Size Study

This repository presents a comprehensive performance study of Sparse Matrix-Vector Multiplication (SpMV) optimized through chunk size variation and compiler flags. Conducted as part of the High-Performance Computing (HPC) module at IATIC-5, this project explores the effect of compiler optimizations and MAQAO recommendations on execution efficiency.

## 🧠 Project Summary
Sparse Matrix-Vector Multiplication (SpMV) is a fundamental kernel in scientific computing. This study investigates how varying `CHUNK_SIZE` and using different compilers—GCC, Intel OneAPI (ICX), and AMD AOCC—impacts execution time, memory efficiency, and thread stability.

The project combines:
- Advanced compiler flags: `-O3`, `-Ofast`
- MAQAO profiling and optimization suggestions
- SIMD (AVX2) vectorization
- Loop unrolling
- Memory alignment techniques

## 📊 Key Findings
- **Best Performance:** AOCC with `CHUNK_SIZE = 1200`.
- **Worst Performance:** ICX with `-Ofast` on AMD hardware.
- **Optimal Configuration:** `CHUNK_SIZE = 1200` across most tests.
- **MAQAO Improvements:** SIMD + unrolling reduced `spmv_csr()` time by ~17%.

## 🛠️ Environment
- **OS:** Ubuntu 24.10 (Kernel 6.11)
- **CPU:** AMD 12-core with NUMA (L1: 192KiB, L2: 3MiB/core, L3: 16MiB shared)
- **Compilers:**
  - GCC 14.2.0
  - Intel oneAPI ICPX 2025.0.4
  - AMD AOCC 17.0.6
- **Profiler:** MAQAO 2.21

## 📂 Repository Structure
```
├── src/                   # Source code for SpMV with OpenMP
├── maqao/                 # MAQAO profiling reports (HTML)
├── results/               # Execution outputs & plots
├── README.md              # You are here
├── report.pdf             # Full HPC report
└── scripts/               # Setup, run and analysis scripts
```

## 🚀 Getting Started
Clone the repository:
```bash
git clone https://github.com/yourusername/spmv-chunksize-study.git
cd spmv-chunksize-study
```

Compile the source using different compilers:
```bash
# GCC
make CC=gcc FLAGS="-O3 -fopenmp"

# Intel oneAPI
make CC=icx FLAGS="-O3 -fopenmp"

# AMD AOCC
make CC=clang FLAGS="-O3 -fopenmp"
```

Run benchmarks:
```bash
OMP_PLACES=cores OMP_PROC_BIND=close ./spmv_exec 1200
```

## 🧪 Optimization Techniques
MAQAO suggested and we implemented:
- **Memory alignment:**
  ```c
  posix_memalign((void**)&x, 32, sizeof(double) * n);
  ```
- **Loop unrolling:**
  ```c
  for (int i = 0; i < n; i += 4) { ... }
  ```
- **SIMD vectorization (AVX2):**
  ```c
  __m256d x_vals = _mm256_load_pd(...);
  ```

## 📈 Results & Plots
Plots comparing GFlops/s, execution time, affinity stability, and array access efficiency for:
- CHUNK_SIZE = 600, 1200, 2000
- Compilers: GCC, ICX, AOCC

See the `/results` and `/report.pdf` for details.

## 🧾 Citation
If you use this project or report in your research or academic work:
```
@project{SPMV,
  title={Optimizing SpMV: A Comparative Chunk\_Size Study},
  author={Rochdi Dardor},
  institution={Université Paris-Saclay, IATIC-5},
  year={2025},
  note={Supervised by M. William Jalby}
}
```

## ⚠️ Important Notice

> **The full source code is not publicly available in this repository** for academic and licensing reasons.  
> However, if you are interested in the **full source code** or the **complete PDF report**, feel free to **contact me directly via email**:

📩 **rochdi.dardor@ens.uvsq.fr**


## 👨‍💻 Author
**Rochdi Dardor**  
Final Year Engineer @ Université Paris-Saclay  
Email: rochdi.dardor@ens.uvsq.fr

## 📃 License
This project is for academic use only. For other use cases, please contact the author.

