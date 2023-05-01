---
{"dg-publish":true,"permalink":"/hallway-navigation-data-analysis/","created":"","updated":""}
---


Code on [Josh's Github repo](https://github.com/sternj98/gain_change_photometry)

Most important modules:
- `datajoint_pipeline/behavior_processing.py`
- `datajoint_pipeline/visualization.ipynb`

The columns for the behavioral file are:

```python
beha_file_header = [
	"Timestamp",
	"WheelPosition",
	"RunningSpeed",
	"Reward",
	"Gain",
	"TrialNumber"
]
```

These classes sync behavior to photometry and extract the gain change events:

```python
class PhotometrySyncBehavior(dj.Computed):

	definition = """	
	# Behavior time series on photometry clock
	-> PhotometrySession
	-> BehaviorSession
	---
	wheel_position : longblob
	running_speed : longblob
	reward : longblob
	gain : longblob
	trial_num : longblob
	"""
```

```python
class GainChangeEvents(dj.Computed):
	definition = """
	# Sample numbers of gain change events and information about them
	-> PhotometrySyncBehavior
	---
	gain_change_samples : blob
	gain_change_magnitudes : blob
	delta_position : blob
	gain_pre : blob
	gain_post : blob
	"""
```


> [!bug] Experiment-simulation discrepancy
> Their wallpaper is striped but with a sinusoidal gradient, whereas ours doesn't have such a gradient.


## To do for paper

Extract and plot traces with:

- no glitch (delta) but gain change
	- there might not be many of these trials
- no gain change but glitch
	- should be plenty
	- bin the delta position to half of the sinusoid 