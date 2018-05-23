# YOLT #

## You Only Look Twice: Rapid Multi-Scale Object Detection In Satellite Imagery

YOLT is an extension of the YOLO framework (https://pjreddie.com/darknet/yolov2/) that can evaluation satellite images of arbitrary size, and runs at ~50 frames per second.

References:

arxiv

https://medium.com/the-downlinq/you-only-look-twice-multi-scale-object-detection-in-satellite-imagery-with-convolutional-neural-38dad1cf7571

https://medium.com/the-downlinq/you-only-look-twice-multi-scale-object-detection-in-satellite-imagery-with-convolutional-neural-34f72f659588

https://medium.com/the-downlinq/building-extraction-with-yolt2-and-spacenet-data-a926f9ffac4f

https://medium.com/the-downlinq/car-localization-and-counting-with-overhead-imagery-an-interactive-exploration-9d5a029a596b

https://medium.com/the-downlinq/the-satellite-utility-manifold-object-detection-accuracy-as-a-function-of-image-resolution-ebb982310e8c

https://medium.com/the-downlinq/panchromatic-to-multispectral-object-detection-performance-as-a-function-of-imaging-bands-51ecaaa3dc56

If you plan on using YOLT in your work, please consider citing YOLO (https://arxiv.org/abs/1612.08242) and YOLT (arxiv)



The YOLT code alters a number of the files in src/*.c to allow further functionality.  We also build a python wrapper around the C functions to improve flexibility.  We utililize the default data format of YOLO, which places images and labels in different forlders.  
An example training data and image: 

        /data/images/train1.tif
        /data/labels/train1.txt
        
Each line of the .txt file has the format

    <object-class> <x> <y> <width> <height>
    
Where x, y, width, and height are relative to the image's width and height.

Labels can be created with https://github.com/tzutalin/labelImg, and converted to the appropriate format with the /yolt/scripts/convert.py script.  Also see https://github.com/Guanghan/darknet for an example of YOLO modifications.


## Execution #

##############################################
### HELP
python yolt2.py --help

##############################################
### COMPILE (gpu machine)
python yolt2.py \
--mode compile


##############################################
### TRAIN (gpu_machine)


\# e.g.: boats and planes
python ../scripts/yolt2.py \
--mode train \
--outname 3class_boat_plane \
--object_labels_str  boat,boat_harbor,airplane \
--cfg_file ave_13x13.cfg  \
--nbands 3 \
--weight_file ave_13x13_boats_planes_voc.weights \
--train_images_list_file boat_airplane_all.txt \
--single_gpu_machine 0 \
--keep_valid_slices False \
--max_batches 60000 \
--gpu 1

##############################################
### VALIDATE (gpu_machine)

\# test on all: boats, cars, and airplanes with new model
cd /raid/local/src/yolt2/results/
python ../scripts/yolt2.py \
--mode valid \
--outname qgis_labels_all_boats_planes_cars_buffer \
--object_labels_str airplane,airport,boat,boat_harbor,car \
--cfg_file ave_26x26.cfg \
--valid_weight_dir train_cowc_cars_qgis_boats_planes_cfg=ave_26x26_2017_11_28_23-11-36 \
--weight_file ave_26x26_30000_tmp.weights \
--valid_testims_dir qgis_validation/all \
--keep_valid_slices False \
--valid_make_pngs True \
--valid_make_legend_and_title False \
--edge_buffer_valid 1 \
--valid_box_rescale_frac 1 \
--plot_thresh_str 0.4 \
--slice_sizes_str 416 \
--slice_overlap 0.2 \
--gpu 2


##############################################

## TBD:

1. Upload data preparation scripts
2. Describe multispectral data handling
3. Describle initial results with YOLOv3
4. Describle better labeling methods

