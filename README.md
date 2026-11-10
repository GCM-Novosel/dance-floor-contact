# Dance floor contact

Open **Labeling_Guide.ipynb** to review the draft CVAT annotations. Open **Research_Notebook.ipynb** to see the experiments, their outputs, and why we chose the current pipeline. The final end-to-end notebook will follow after human review and retraining.

## Open the notebooks

Use Python 3.12. Download the complete ZIP from [Releases](https://github.com/GCM-Novosel/dance-floor-contact/releases/latest) for the simplest setup. A regular Git clone also includes the videos, PNGs and weights.

Windows PowerShell, from this folder:

    python -m venv .venv
    .\.venv\Scripts\python.exe -m pip install -r requirements-notebooks.txt
    .\.venv\Scripts\python.exe -m jupyterlab

On macOS/Linux, use .venv/bin/python in place of the Windows executable. Select the Python 3 kernel from this environment and use **Run All Cells**. For a non-interactive check, run the same Python with tools/run_notebooks.py. The notebooks run on CPU and do not start training.

## What is included

- media/source_choreography.mp4: original video.
- Experiment/Dance_Project_Shared/dataset/frames: 71 lossless review images and seven separate confirmation images.
- Experiment/Dance_Overnight/CVAT_Review: A/B/C starter XML, task schema, frame lists and checklist. The labeling notebook creates the three image ZIPs under build/cvat.
- Experiment/Dance_Overnight: stage comparisons, frozen experiment results, selected weights, code and caches used to reproduce the final map fit.
- tools/extract_frames.py: regenerate any source frame range with OpenCV. By default it writes all 2,424 static-camera frames to a new build folder.

## After CVAT review

Place A.xml, B.xml and C.xml in build/. From Experiment/Dance_Overnight, with the notebook environment active:

    python code/import_cvat_review.py ../../build/A.xml ../../build/A.json
    python code/import_cvat_review.py ../../build/B.xml ../../build/B.json
    python code/import_cvat_review.py ../../build/C.xml ../../build/C.json
    python code/merge_review_exports.py ../../build/combined.json ../../build/A.json ../../build/B.json ../../build/C.json
    python code/prepare_reviewed_training.py ../../build/combined.json ../../build/team_review.json --reviewer "Reviewer names and date"

The converter checks IDs, point/status consistency and split membership. Keep the seven confirmation images outside training.

## Reproduce or retrain

Use a separate Python 3.12 ML environment with the packages in Experiment/Dance_Overnight/requirements.txt. For training, use a CUDA-enabled PyTorch build for your GPU. The original versions are in environment.json. Training may fetch pretrained ResNet weights.

From Experiment/Dance_Overnight with that ML environment active:

    python code/reproduce_latest.py --output ../../build/reproduction
    python code/retrain_selected.py --variant expanded --manifest ../../build/team_review.json --name team_review_v1 --seed 17

The first command refits and scores the map from saved detector/tracker and support predictions; it does not rerun detection. It should reproduce 29 of 46 visible contacts within 3% of mat width. The second trains the support model on the reviewed revision. Use a new output/run name for later runs. Earlier experiment code is included for reference; only the selected pipeline's assets and the notebook evidence are bundled.

For the source video extraction, run python tools/extract_frames.py from the project folder using the ML environment.
