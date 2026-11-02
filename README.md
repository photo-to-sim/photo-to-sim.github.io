# Project page: From Photographs to Segmentation (WACV 2027)

Static site, no build step: `index.html` plus `assets/`. Fonts load from Google Fonts; nothing else is external.

**No 3D geometry is published.** The 3D sections are pre-rendered turntable videos (`assets/spin/*.mp4`), drawn into a canvas; dragging seeks the video. The `.glb` scenes stay private in `../project_page_preview/web3d_private/` and must never be copied into this folder.

## Preview locally

    cd project_page
    python3 -m http.server 8000

Open http://localhost:8000.

## Publish on GitHub Pages

1. Create an empty **public** repository on GitHub (for example `photo-to-sim`). No README, licence or .gitignore, so the first push is clean.
2. In this folder:

       git init -b main
       git add .
       git commit -m "Project page"
       git remote add origin https://github.com/photo-to-sim/photo-to-sim.github.io.git
       git push -u origin main

3. On GitHub: Settings → Pages → Build and deployment → Source "Deploy from a branch", branch `main`, folder `/ (root)`, Save.
   After a minute the page is at `https://photo-to-sim.github.io/`.

`.nojekyll` makes GitHub serve the folder as-is. Every file is far below GitHub's 100 MB per-file limit (largest: the robot video, 11 MB).

Updating later: replace files, then `git add -A && git commit -m "Update" && git push`; Pages redeploys automatically.

When the paper is out, turn the "Paper (soon)" entries in the header and nav into links.

## What loads when

- On open (about 4 MB): header loop, pipeline images, first gallery frame.
- Near the 3D section (within 300 px of the screen): each pack's turntable video streams, 11–14 MB each; in the pipeline, about 0.5 MB per part.
- On play: the robot video (11 MB). Gallery frames load one at a time as they are clicked.

## Files

- `assets/hero_teaser.mp4`, `hero_teaser_poster.jpg`: header loop. `../project_page_preview/tools/teaser_turntable.py` (Blender) + `teaser_compose.py`.
- `assets/pipe/`: pipeline steps. `photo_*.jpg`, `scene.jpg`, `synth_*.jpg`, `real.jpg` / `mask.jpg` (`teaser_seg.py`).
- `assets/spin/<scene>.mp4`: turntables, appearance | semantic classes side by side (1920x720). 480 angles rendered from the private `.glb` scenes with `../project_page_preview/tools/pack_turntable.py` (Blender), then `INTERP=4 spin_video.py` (motion-interpolated to 1,920 frames: one turn in 80 s at 24 fps, keyframe every 12 frames for fast seeking). Parts: `part_*.mp4`, 240 angles (`DIST=4.4 PASSES=app RES_X=480 RES_Y=480`), `INTERP=4`, 40 s per turn.
- `assets/seg/`: gallery frames (`render_gallery.py`, `render_synth.py`).
- `assets/robot_overlay_5x_720.mp4`, `video_poster.jpg`: robot clip with the robot-camera inset (`theme_caps.py`, then `make_overlay.py`, composed over IMG_2359.MOV at 5x).

Files no longer used by the page are kept in `../project_page_preview/unused_assets/`.
