# Dance floor contact

Start with [the master notebook](Master_Notebook.ipynb) for the story from source video to the finished map. Each step includes a runnable cell and its saved output. [The research notebook](Research_Notebook.ipynb) has more detail on the earlier experiments.

[Download the project from main](https://github.com/GCM-Novosel/dance-floor-contact/archive/refs/heads/main.zip) · [Watch the 300-image replay at 0.30](Experiment/lighting_expansion_300/runs/lighting_300_visual_s17/joint_evaluation/lighting_trial_replay.mp4) · [Read the 300-image results](Experiment/lighting_expansion_300/RESULTS.md)

## Review the labels

The dataset combines the previous 200 images and 100 additions. A, B, and C each contain 100 images. [Dataset notes and counts](Experiment/lighting_expansion_300/DATASET.md)

Run this from the project folder with Python to create the three review ZIPs and the combined ZIP under `build/300-packs`:

    python tools/build_300_packs.py

Use `--batch A`, `--batch B`, `--batch C` or `--batch combined` to make a single pack. Each pack contains the images and CVAT XML. [Assignments](Experiment/lighting_expansion_300/assignments.json) · [Merged XML](Experiment/lighting_expansion_300/merged_annotations.xml) · [Training manifest](Experiment/lighting_expansion_300/training_manifest.json)

Floor Contact Labeler is a portable Windows app for editing dancer boxes, support points and floor guides. Open a pack's images folder, then import its XML from the Project menu. [Windows executable](annotator/FloorContactLabeler.exe) · [Labeler quick start](annotator/README.md)

## Run the notebook

From the project folder in Windows PowerShell with Python 3.12:

    python -m venv .venv
    .\.venv\Scripts\python.exe -m pip install -r requirements-notebooks.txt
    .\.venv\Scripts\python.exe -m jupyterlab

On macOS or Linux, use `.venv/bin/python`. Select the Python 3 kernel from that environment and run all cells in `Master_Notebook.ipynb`. The walkthrough uses saved experiment outputs and runs on CPU. The full training runs use a separate CUDA environment. [Retraining commands](Experiment/lighting_expansion_300/REPRODUCE.md)

## Results and next step

The 300-image pipeline localized 14 of 46 visible contacts within 1% of mat width and reported two of 50 hidden contacts as measured on seven reserved same-video images. The published baseline still leads at 3% tolerance. The next project step is formation detection and measurement of dancer placement. [Map coordinates for the 0.30 replay](Experiment/lighting_expansion_300/runs/lighting_300_visual_s17/joint_evaluation/map_positions.json)
