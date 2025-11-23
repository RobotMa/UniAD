# Repository Guidelines

## Project Structure & Module Organization
- `projects/configs/`: Training/eval configs for stage1 (perception), stage2 (end-to-end), and BEVFormer backbones; clone configs rather than editing base files when iterating.
- `projects/mmdet3d_plugin/uniad/`: Core detectors, dense heads, and task modules; use this layout when adding new tasks.
- `tools/`: Distributed and Slurm launchers (`uniad_dist_train.sh`, `uniad_dist_eval.sh`), visualization helpers under `analysis_tools/`.
- `docs/`: Installation, data prep, and train/eval guides; keep updates synchronized with script/config changes.
- `ckpts/` for released weights, `data/` for dataset links, `docker/` for workspace containers, `sources/` for assets (figures/slides).

## Build, Test, and Development Commands
- Environment: `python -m venv venv && source venv/bin/activate` then `pip install -r requirements.txt` (adds torch/mmdet3d dependencies; see `docs/INSTALL.md` for CUDA notes).
- Train: `./tools/uniad_dist_train.sh ./projects/configs/stage1_track_map/base_track_map.py 8` (replace config/GPU count as needed).
- End-to-end stage2: same launcher with `projects/configs/stage2_e2e/base_e2e.py`.
- Eval (regression check): `./tools/uniad_dist_eval.sh ./projects/configs/stage1_track_map/base_track_map.py ./ckpts/uniad_base_track_map.pth 8`.
- BEVFormer-only: `./tools/uniad_dist_train.sh ./projects/configs/bevformer/bevformer_base.py 8`.
- Visualization sanity check: `python ./tools/analysis_tools/visualize/run.py --predroot RESULT.pkl --out_folder ./vis_out --demo_video demo.avi --project_to_cam True`.

## Coding Style & Naming Conventions
- Python with 4-space indentation; mirror MMDet3D style (CamelCase classes, snake_case functions/vars). Config files stay snake_case and keep stage identifiers (`stage1_track_map`, `stage2_e2e`).
- Use `yapf` for formatting (`pip install yapf`; optional: `yapf -ir projects/mmdet3d_plugin/uniad/your_file.py`); avoid mixing tabs/spaces.
- Keep shell scripts POSIX-friendly and executable (`chmod +x`), and guard params (`set -e` already used in launchers).

## Testing Guidelines
- Treat `uniad_dist_eval.sh` as the primary regression test; record GPU count because metrics vary slightly when not using 8 GPUs.
- For quick checks, run eval on a small subset or single GPU before full multi-GPU jobs; capture logs in `work_dirs/<experiment>/`.
- When changing visualization or post-processing, compare generated videos/plots against a known checkpoint output.

## Commit & Pull Request Guidelines
- Follow repo history pattern: short, imperative subjects (`Update README for ...`); keep under ~72 chars. Emojis are used sparingly and are optional.
- In PRs, include: summary of changes, configs/commands run, dataset split used, and links to relevant issues. Attach qualitative outputs (screenshots/videos) for visualization changes.
- Ensure new configs/scripts are documented in `docs/` and referenced from README if user-facing. Avoid committing large data; point to download links or scripts instead.
