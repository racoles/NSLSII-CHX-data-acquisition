# NSLSII-CHX-data-acquisition
Retrieve data from CHX (NSLSII Hard X-ray beamline) BlueSky servers.

Title: CHX-axis-labels-and-cropping<br/>
Author: Rebecca Coles<br/>
Updated on Aug 08, 2019<br/>

## Required Python Packages
Required Python Packages:<br/>
	os<br/>
	datetime (datetime)<br/>
	matplotlib.pyplot<br/>
	numpy<br/>
	h5py<br/>
	itertools<br/>
	PIL (Image)<br/>
	warnings<br/>

External CHX package:<br/>
	pyCHX.chx_xpcs_xsvs_jupyter_V1:<br/>
    https://github.com/NSLS-II-CHX/pyCHX/blob/master/pyCHX/chx_xpcs_xsvs_jupyter_V1.py

## Useful Default Variables
y_crd: horizontal cut at row<br/>
x_crd: vertical cut at column<br/>
x1, x2, y1, y2: crop image at given columns and rows (Example: x1, x2, y1, y2 = 900, 1650, 750, 1400)<br/>
dpi: dpi of images<br/>
eiger_size_per_pixel: eiger is 75 um per pixel so typically eiger_size_per_pixel = 0.075<br/>

## Functions

### save_hdf5
Access BlueSky HDF5 binary data from CHX measurement.<br/>
`def save_hdf5(data, filename='data.h5', dataset='dataset')`
        
param data: HDF5 binary data from CHX measurement.
param filename='data.h5': HDF5 filename. Default is contained in the header file.
param dataset='dataset': Creates dataset type. Default is dataset.

return: string status of dataset creation.

### plot_profile_horiz
Show plot of intensity versus horizontal position.
`def plot_profile_horiz(data, uid, y_crd, clim=(0, 200), cmap='afmhot', line_color='red', linestyles=None)`

param data: HDF5 binary data from CHX measurement.
param uid: unique ID automatically assigned to a CHX measurement.
param y_crd: add a horizontal line across the axis at a given location on the image.
param clim=(0, 200): sets the color limits of the current image.
param cmap='afmhot': color map (https://matplotlib.org/examples/color/colormaps_reference.html)
param line_color='red': color of line that will show the cut location.

### plot_profile_vert
Show plot of intensity versus vertical position.
`def plot_profile_vert(data, uid, x_crd, clim=(0, 200), cmap='afmhot', line_color='red', linestyles=None)`

param data: HDF5 binary data from CHX measurement.
param uid: unique ID automatically assigned to a CHX measurement.
param x_crd: 
param clim=(0, 200): sets the color limits of the current image.
param cmap='afmhot': color map (https://matplotlib.org/examples/color/colormaps_reference.html) 
param line_color='red': color of line that will show the cut location. 
param linestyles=None: custom linestyles.

### display_image_in_actual_size
Display CHX eiger image in full size. Will save the plot as a TIFF.
`def display_image_in_actual_size(im, uid, clim, cmap='gist_stern')`

param im: eiger detector image.
param uid: unique ID automatically assigned to a CHX measurement.
param cmap='gist_stern': color map (https://matplotlib.org/examples/color/colormaps_reference.html) 
param clim: sets the color limits of the current image.

### display_cropped_image
Display CHX eiger image cropped to user specifications. Will save the plot as a TIFF. 
`def display_cropped_image(im, uid, x1, x2, y1, y2, clim, cmap='gist_stern')`

param im: eiger detector image. 
param uid: unique ID automatically assigned to a CHX measurement.
param x1: x-axis stating location (columns)
param x2: x-axis final location (columns) 
param y1: y-axis stating location (rows) 
param y2: y-axis final location (rows)  
param cmap='gist_stern': color map (https://matplotlib.org/examples/color/colormaps_reference.html) 
param clim: sets the color limits of the current image.

### plot_eiger
Display CHX eiger image: fullsize, cropped to user specifications, and with horizontal and vertical cuts. Will save the plot as a TIFF. 
`def plot_eiger(uid, det='eiger4m_single_image', cmap='afmhot', clim=(0, 100), mean=False, frame_num=0, grid=False)`

param uid: unique ID automatically assigned to a CHX measurement.
param det='eiger4m_single_image': which eiger dector.
param cmap='gist_stern': color map (https://matplotlib.org/examples/color/colormaps_reference.html) 
param clim=(0, 200): sets the color limits of the current image.
param mean=False: mean of combined images along axis 0.
param frame_num=0: which image to use.
param grid=False: grid on the image.

## Example Output

### get_meta_data
Using the get_meta_data from the CHX package:
`get_meta_data('1b9ff7',verbose=True)`
where 1b9ff7 is the UID for the CHX measurement, gives the output:
```{'suid': '1b9ff7',
 'filename': '/XF11ID/data/2017/10/24/98e7508f-61f3-4c03-909a_4806_master.h5',
 'detector': 'eiger4m_single_image',
 'beam_center_x': 1098.0,
 'beam_center_y': 1225.0,
 'wavelength': 1.2846771478652954,
 'det_distance': 10.038560260000002,
 'cam_acquire_time': 60.0,
 'cam_acquire_period': 60.0000114440918,
 'cam_num_images': 1,
 'threshold_energy': 4825.5,
 'photon_energy': 9651.0,
 'detectors': ['eiger4m_single'],
 'num': 1,
 'time': 1508886690.4590404,
 'uid': '1b9ff785-e508-4438-875c-6d04123bd9b3',
 'scan_id': 8289,
 'hints': {'dimensions': [[['time'], 'primary']]},
 'run': '2017-3',
 'user': 'Chubar',
 'scatterer': 'R5',
 'Measurement': 'R5 - 60s exposure, MBS:0.05x0.4',
 'beamline_id': 'CHX',
 'MBS': '0.05x0.4',
 'plan_type': 'generator',
 'num_intervals': 0,
 'plan_name': 'count',
 'num_points': 1,
 'sample': 'Litho 4 - Julien',
 'owner': 'xf11id',
 'start_time': '2017-10-24 19:11:30',
 'stop_time': '2017-10-24 19:12:32',
 'img_shape': [2167, 2070],
 'verbose': True}```
 
 ### plot_eiger
 Using the plot_eiger definition:
 `plot_eiger('1b9ff7', cmap='gist_stern', frame_num=0)`
 where 1b9ff7 is the UID for the CHX measurement, gives the output:
```
{'eiger4m_single_stats4_total', 'eiger4m_single_stats2_total', 'eiger4m_single_image', 'eiger4m_single_stats3_total', 'eiger4m_single_stats1_total', 'eiger4m_single_stats5_total'}
(1, 1, 2167, 2070)
min: 0, max: 4294967295
<Figure size 432x288 with 0 Axes>
```
![][1b9ff7.tif]
![][1b9ff7_cropped.tif]
`Horizontal cut at row 1200`
![][1b9ff7_profile_horiz_image.tif]
![][1b9ff7_profile_horiz_intensity.tif]
`Vertical cut at column 1100`
![][1b9ff7_profile_vert_image.tif]
![][1b9ff7_profile_vert_intensity.tif]
```
[<Frames>
 Length: 1 frames
 Frame Shape: 2167 x 2070
 Pixel Datatype: uint32]
 ```