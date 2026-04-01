This is the readme file for the code and sample data we provided for reproducibility.
We provide the following:
1. A trained 3D-GS checkpoint that can used to reproduce the rendering results. For example, you can reproduce the rendering results using the following command: `python render.py -m data/lion/output` 
2. The event stream captured during data collection is available in: `data/lion/event.npz`
3. The dark frames captured in low-light conditions can be found in: `data/lion/low_light_frames`
4. The groundtruth frames captured in bright-light conditions can be found in: `data/lion/gt_bright_frames`
5. Camera pose and coordinate data are stored in: `data/lion/sparse`
