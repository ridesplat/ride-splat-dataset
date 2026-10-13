# RIDE-Splat Dataset

A 3D reconstruction benchmark for removing windshield reflections from driving video.

> **Preliminary release (v0.1).** This release contains the three scenes evaluated in the paper,
> rs1 to rs3. Additional captured scenes will follow in a later release; we intend to keep the names
> and train/eval splits of these three scenes unchanged.

Project page: https://ridesplat.github.io

**Download:** `ride-splat-dataset-v0.1.zip` (676 MB) from the
[v0.1-preliminary release](https://github.com/ridesplat/ride-splat-dataset/releases/tag/v0.1-preliminary).

Existing benchmarks force a trade-off: datasets with exact non-reflective targets use synthetic
reflections, while datasets with real windshield reflections have no clean target to score against.
This dataset pairs **real** driving scenes carrying **physical** windshield reflections with a
**separately captured non-reflective traversal of the same route**, so a reconstruction can be scored
against actual reflection-free images.

## Capture

For each scene, we recorded two traversals minutes apart using the same rigidly mounted dashcam.
Reflection-inducing materials remained fixed on the dashboard during the reflective traversal and
were removed for the non-reflective traversal; the camera mount was unchanged. We jointly registered
both traversals with COLMAP in a common coordinate frame. Footage was captured with a Garmin S35
dashcam at 1920x1080 and 29.97 FPS under clear daytime conditions. We removed the 40-pixel-tall
metadata banner, then undistorted and cropped the released frames to 1900x1025.

The reflective objects differ between scenes but stay fixed within a traversal, so each scene's
reflection is scene-specific and approximately fixed in image coordinates.

## Scenes

| Scene | Setting | FPS | Reflective training frames | Held-out non-reflective frames |
|---|---|---|---|---|
| rs1 | historic town center | 8 | 134 | 30 |
| rs2 | tree-lined institutional district | 6 | 82 | 30 |
| rs3 | suburban residential streets | 8 | 52 | 30 |

## Layout

```
rs1/
  train/        reflective traversal — the only frames a model may train on
  eval/         held-out frames from the non-reflective traversal
  sparse/0/     COLMAP cameras.bin, images.bin, points3D.ply (both traversals, one coordinate frame)
  meta.json     counts, fps, resolution
rs2/ ...
rs3/ ...
```

## Protocol

Train only on `train/`. Score renders at the `eval/` camera poses against the `eval/` images.
Evaluation frames are uniformly sampled along the non-reflective traversal.

**The references are approximate.** The fixed mount, the short interval between traversals, and
sequence selection keep the two drives close, but traffic, illumination, and the exact trajectory
cannot be matched. Treat the evaluation frames as close references rather than pixel-exact targets.

## Privacy

Faces and licence plates are blurred with EgoBlur. If you find a face or plate that is still
identifiable, please open an issue on the dataset repository and we will fix it in the next release.

## License

Released under the Creative Commons Attribution 4.0 International License (CC BY 4.0),
https://creativecommons.org/licenses/by/4.0/.

## Citation

Citation information will be added when the paper is public.
