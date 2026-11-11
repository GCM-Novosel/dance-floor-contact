# Dance floor contact

Open [Master_Notebook.ipynb](Master_Notebook.ipynb) for the team walkthrough and [Research_Notebook.ipynb](Research_Notebook.ipynb) for the experiments, CVAT examples, training results, and display sweep. [Labeling_Guide.ipynb](Labeling_Guide.ipynb) covers the earlier A/B/C review tasks.

The published baseline localized 29 of 46 visible contacts within 3% of mat width. The 200-image revision localized 26 of 46 and reported fewer hidden contacts as measured (3 of 50 versus 5 of 50). The 0.30 replay is the team demo. Formation detection is pending.

## Open the notebooks

Use Python 3.12. Extract the team ZIP or open this clone. From the project folder in PowerShell:

    python -m venv .venv
    .\.venv\Scripts\python.exe -m pip install -r requirements-notebooks.txt
    .\.venv\Scripts\python.exe -m jupyterlab

On macOS or Linux, use .venv/bin/python. Select the Python 3 kernel from that environment. The notebooks include saved outputs and can be run on CPU. Run tools/run_notebooks.py to execute all three without opening JupyterLab.

## Team demo and training data

- [0.30 replay MP4](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/display_threshold_0.3/lighting_trial_replay.mp4) and [per-frame map positions](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/display_threshold_0.3/map_positions.json)
- [200 training PNGs](Experiment/lighting_expansion_200/frames), [exact training manifest](Experiment/lighting_expansion_200/visual_relabel/training_manifest.json), and [CVAT export](Experiment/lighting_expansion_200/visual_relabel/cvat_visual_revision_export.zip). The original 11 validation PNGs are included under Experiment/Dance_Project_Shared/dataset/frames.
- [Annotation and training report](Experiment/lighting_expansion_200/visual_relabel/RESULTS.md), [CVAT transfer check](Experiment/lighting_expansion_200/visual_relabel/cvat_roundtrip_check.json), [frozen comparison](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/evaluation/results.json), and [threshold metrics](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/display_sweep.json)
- [Validation-selected checkpoint](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/best_joint.pt), [training history](Experiment/lighting_expansion_200/runs/lighting_200_visual_s17/history.json), and [original choreography video](media/source_choreography.mp4)

The labels are approximate assistant visual estimates. The CVAT check verifies transfer. The four uncertain labels were excluded from training crops.

## Reproduce the results

Use the separate Python 3.12 ML environment described in [the training report](Experiment/lighting_expansion_200/visual_relabel/RESULTS.md) and [the ML requirements](Experiment/Dance_Overnight/requirements.txt). Use CUDA-enabled PyTorch for GPU training and fresh checkpoint inference. From the project folder, the cached predictions can be used to recompute the display metrics and render 0.30:

    python Experiment/lighting_expansion_200/measure_display_sweep.py
    python Experiment/lighting_expansion_200/render_contact_threshold.py --threshold 0.30

To reproduce the published baseline from cached data:

    cd Experiment/Dance_Overnight
    python code/reproduce_latest.py --output ../../build/reproduction

Use [tools/extract_frames.py](tools/extract_frames.py) to extract other frames from the source video. The seven reserved confirmation images stay outside training.
