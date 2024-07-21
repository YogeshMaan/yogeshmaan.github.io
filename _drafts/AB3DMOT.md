## Steps to install

1. mkdir CSE598_Project 
2. cd CSE598_Project
3. gitclone AB3DMOT 
4. virtualenv AB3DMOT/
5. cd AB3DMOT
6. . bin/activate
7. (AB3DMOT) yogesh@dell
8. pip3 install -r requirements.txt
9. 

## To-Do's
- [ ] CUDA installation
- [ ] python 3.7 in venv


# SETUP for SOL supercomputer

#### Connect to cisco vrpn client
- After launching Cisco Secure Client, run this command to launch vrpn gui
``` ~bash
gtk-launch com.cisco.secureclient.gui
```
- Connect to 
```
sslvpn.asu.edu/2fa
```
**NOTE:** Use 'push' as second password, to get a DUO notification

### Required resources
1. CPU cores
2. RAM
3. GPU




# Presentation

## Team 1
- Abstract
- Introduction
- **Methods**: Model-free RL, teacher policy, student policy, 
- Table of baseline
- Experimental setup
- Visual observation
- Result
- 

# Debugging

### Create symbolic link
```bash
ln -s <source-folder> <link-folder>
```

### KeyError
```bash
Traceback (most recent call last):
  File "main.py", line 140, in <module>
    main(args)
  File "main.py", line 129, in main
    ID_start = main_per_cat(cfg, cat, log, ID_start)
  File "main.py", line 48, in main_per_cat
    tracker, frame_list = initialize(cfg, trk_root, save_dir, subfolder, seq_name, c, ID_start, hw, log)
  File "/home/yogesh/CSE598_Project/AB3DMOT/AB3DMOT_libs/utils.py", line 93, in initlize
    calib = Calibration(calib)
  File "/home/yogesh/CSE598_Project/AB3DMOT/AB3DMOT_libs/kitti_calib.py", line 64, i__init__
    self.V2C = calibs['Tr_velo_to_cam']
KeyError: 'Tr_velo_to_cam'
Segmentation fault (core dumped)

```
### generate video from folder error
```bash
Traceback (most recent call last):
  File "scripts/post_processing/visualization.py", line 121, in <module>
    vis(args)
  File "scripts/post_processing/visualization.py", line 115, in vis
    generate_video_from_folder(save_3d_bbox_dir, video_file, framerate=framerate)
  File "/home/yogesh/CSE598_Project/AB3DMOT/Xinshuo_PyToolbox/xinshuo_video/video_processing.py", line 123, in generate_video_from_folder
    generate_video_from_list(image_list, save_path, framerate=framerate, downsample=downsample, display=display, warning=warning, debug=debug)
  File "/home/yogesh/CSE598_Project/AB3DMOT/Xinshuo_PyToolbox/xinshuo_video/video_processing.py", line 97, in generate_video_from_list
    video_writer = FFmpegWriter(save_path, inputdict=inputdict, outputdict=outputdict)
  File "/home/yogesh/CSE598_Project/AB3DMOT/lib/python3.7/site-packages/skvideo/io/ffmpeg.py", line 87, in __init__
    assert _HAS_FFMPEG, "Cannot find installation of real FFmpeg (which comes with ffprobe)."
AssertionError: Cannot find installation of real FFmpeg (which comes with ffprobe).
Segmentation fault (core dumped)

```
# Visualizing Point Cloud Data


