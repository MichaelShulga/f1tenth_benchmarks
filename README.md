# F1TENTH Benchmarks — Reproduction Archive

This fork provides a locked, executable reproduction of the experiments from
[Unifying F1TENTH Autonomous Racing: Survey, Methods and Benchmarks](https://arxiv.org/abs/2402.18558).

- Original repository: [BDEvan5/f1tenth_benchmarks](https://github.com/BDEvan5/f1tenth_benchmarks)
- Verified release: [`reproduction-v1.0.1`](https://github.com/MichaelShulga/f1tenth_benchmarks/releases/tag/reproduction-v1.0.1)
- Supported reproduction platform: Linux x86_64
- Full runtime: approximately 1–2 hours

For descriptions of the simulator, planners and evaluation methodology, refer to the
[original paper](https://arxiv.org/abs/2402.18558) and
[upstream repository](https://github.com/BDEvan5/f1tenth_benchmarks).

## Local reproduction

### 1. Clone the repository

```bash
git clone --recurse-submodules \
  https://github.com/MichaelShulga/f1tenth_benchmarks.git
cd f1tenth_benchmarks
git submodule update --init --recursive
```

### 2. Install Pixi 0.77.0

```bash
mkdir -p "$HOME/.local/bin"
curl -fsSL \
  https://github.com/prefix-dev/pixi/releases/download/v0.77.0/pixi-x86_64-unknown-linux-musl.tar.gz \
  | tar -xz -C "$HOME/.local/bin"
export PATH="$HOME/.local/bin:$PATH"
pixi --version
```

### 3. Install the locked environment

```bash
pixi install --frozen
```

The Git submodule is imported directly from its checked-out source through the
`PYTHONPATH` configured in `pixi.toml`; a separate `pip install` is not required.

### 4. Run all experiments

```bash
pixi run env MPLBACKEND=Agg python -m \
  f1tenth_benchmarks.benchmark_results.generate_benchmark_results
```

### 5. Execute the existing report notebook

```bash
pixi run env MPLBACKEND=module://matplotlib_inline.backend_inline \
  jupyter nbconvert \
  --to notebook \
  --execute \
  --inplace \
  --ExecutePreprocessor.timeout=-1 \
  f1tenth_benchmarks/benchmark_results/benchmark_result_plots.ipynb
```

## Outputs

```text
Logs/Full.csv
Logs/Summary.csv
Logs/*/Results_*.csv
Logs/*/RawData_*/
Data/racelines/mu90/
Data/BenchmarkArticle/
f1tenth_benchmarks/benchmark_results/benchmark_result_plots.ipynb
```

The notebook naturally generates `Data/BenchmarkArticle/benchmark_laptimes.tex`
along with the article plots.

## GitHub Actions reproduction

Push a new reproduction tag:

```bash
git tag reproduction-v1.0.1
git push origin reproduction-v1.0.1
```

The tag starts the **Full reproduction and release** workflow. It runs the existing
benchmark entry point, executes the existing notebook and publishes the resulting logs
and article artifacts as a GitHub Release.

## Reproduction note

The original trained End-to-end model and raw logs were not included upstream. The
workflow retrains that model from the recorded configuration, so its lap times may differ
from the values printed in the paper. Classical results reproduce exactly or with small
solver-dependent numerical differences.

## License

Apache License 2.0. See [`LICENSE.md`](LICENSE.md).
