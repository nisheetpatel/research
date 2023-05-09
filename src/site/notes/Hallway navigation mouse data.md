---
{"dg-publish":true,"permalink":"/hallway-navigation-mouse-data/","created":"","updated":""}
---



> [!todo] To do
> - [ ] Make the same plots for individual mice
> - [ ] Super-sample "None" trials to make it flatter
> - [ ] Add data columns for trial, mouse, session to show "session start" and "session end" (SPE) signals
> - [ ] Get data from Josh & combine to produce plots with all 4 mice

## Figure

1.  Effect of overall "surprise" (multi-modal)
	- Major = Big gain change AND Big phase shift
	- Minor = Either but not both big + Either but not both 0
	- None = both 0
2.  Effect of glitch type
	- Both position and speed glitch
	- Position glitch
	- Speed glitch
	- None
3. Effect of gain change only
4. Effect of phase shift only

![Figure.png](/img/user/images/Figure.png)


## Archive

#### Making sense of the code

Code on [Josh's Github repo](https://github.com/sternj98/gain_change_photometry)
- Now also on [our repo](https://github.com/FelixHub/serotonin_model) (check the `data` branch in not yet pulled to `main`/`master`)

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