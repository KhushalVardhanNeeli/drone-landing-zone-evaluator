# Drone landing zone evaluator

Where could a drone place a package near a requested point in a photograph? This version keeps the training and evaluation scripts at the repository root. Its [mission configuration](mission_config.json) describes the conditions used to score different terrain types.

## See how a decision is made

[model.py](model.py) predicts a terrain label for each pixel. [geometry.py](geometry.py) checks whether a footprint fits and uses depth variation to estimate roughness. [knowledge_graph.py](knowledge_graph.py) and [semantic_brain.py](semantic_brain.py) supply mission-specific terrain penalties. The evaluator combines these signals with distance to the requested pixel.

## Try it

From the repository root, install the dependencies and place trained weights at `best_model.pth`. The depth estimator may download its weights on first use.

```sh
python -m pip install -r requirements.txt
python main.py path/to/aerial-image.jpg 400 300
```

The last two arguments are the target's horizontal and vertical pixel coordinates. To train your own checkpoint, extract the [TU Graz dataset](https://cloud.tugraz.at/index.php/s/csxYfaKmie6LyqA) so `training_set/images` and `training_set/gt/semantic/label_images` exist, review the paths in [train.py](train.py), then run `python split_data.py` and `python train.py`.

![Example landing-zone analysis](https://github.com/user-attachments/assets/e61a8067-b5d3-4037-baf0-57cbece2f4e0)

## Coursework

CS F407 Artificial Intelligence, Assignment II. The [related mapping implementation](https://github.com/KhushalVardhanNeeli/drone-landing-zone-mapping) groups its source under `src/` and uses different relative data paths.
