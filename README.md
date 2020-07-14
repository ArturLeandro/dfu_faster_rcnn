# dfu_faster_rcnn
Keras implementation of a Faster R-CNN approach to detect diabetes foot ulcers.


USAGE:
- Weights for DFU Detection can be found in our one drive folder https://1drv.ms/u/s!AoVbNqANaM4XgdIfpWPQmDwEtNQGZg?e=FMbVw2
- Both theano and tensorflow backends are supported. However compile times are very high in theano, and tensorflow is highly recommended.
- `train_frcnn_dfu.py` can be used to train a model. To train on Pascal VOC data, simply do:
`python train_frcnn_dfu.py -p /path/to/pascalvoc/`. 
- simple_parser.py provides an alternative way to input data, using a text file. Simply provide a text file, with each
line containing:

    `filepath,x1,y1,x2,y2,class_name`

    For example:

    /data/imgs/img_001.jpg,837,346,981,456,Ulcer
    
    /data/imgs/img_002.jpg,215,312,279,391,Ulcer

    The classes will be inferred from the file. To use the simple parser instead of the default pascal voc style parser,
    use the command line option `-o simple`. For example `python train_frcnn_dfu.py -o simple -p my_data.txt`.

- Running `train_frcnn_dfu.py` will write weights to disk to an hdf5 file, as well as all the setting of the training run to a `pickle` file. These
settings can then be loaded by `detect_frcnn_dfu.py` for any testing.

- detect_frcnn_dfu.py can be used to perform inference, given pretrained weights and a config file. Specify a path to the folder containing
images:
    `python detect_frcnn_dfu.py -p /path/to/test_data/`
- Data augmentation can be applied by specifying `--hf` for horizontal flips, `--vf` for vertical flips and `--rot` for 180 degree rotations


NOTES:
- config.py contains all settings for the train or test run. The default settings match those in the original Faster-RCNN
paper. The anchor box sizes are [64, 128, 256, 512] and the ratios are [1:1, 1:2, 2:1].
- The theano backend by default uses a 7x7 pooling region, instead of 14x14 as in the frcnn paper. This cuts down compiling time slightly.
- The tensorflow backend performs a resize on the pooling region, instead of max pooling. This is much more efficient and has little impact on results.


ISSUES:

- If you get this error:
`ValueError: There is a negative shape in the graph!`    
    than update keras to the newest version

- This repo was developed using `python3`.

- If you run out of memory, try reducing the number of ROIs that are processed simultaneously. Try passing a lower `-n` to `train_frcnn_dfu.py`. Alternatively, try reducing the image size from the default value of 480 (this setting is found in `config.py`.
