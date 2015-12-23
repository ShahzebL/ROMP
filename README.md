# Introduction
ROMP/L-ROMP are novel techniques that compress JPEG images. ROMP/L-ROMP defines many more contexts (cases) than standard JPEG and compress each context individually for better compressibility.

# Install
```
 $ cd romp
  - or cd lromp for L-ROMP
 $ ./configure
 $ sudo make install
```
By default, ROMP will be installed to /opt/libjpeg-turbo, and L-ROMP will be installed to /opt/libjpeg-turbo-lossy.

# Usage
ROMP/L-ROMP needs to a special Huffman table for each defined context. Therefore, to use ROMP/L-ROMP, you need to train these tables first, using a set of training images (training step). ROMP/L-ROMP learn statistics from these training images, and then use these trained tables to compress other images.

1. Training step:
 - Prepare a set of training images, put them into one folder named, e.g., TRAIN_IMAGES;
 - Run below commands, specify the folder that contains training images, TRAIN_IMAGES, and an output folder that will be filled with generated tables, TABLES.
  ```
  $ cd training
  $ python training_romp.py TRAIN_IMAGES TABLES
  ```
   - For L-ROMP, using *training_lromp.py* instead, with two additional parameters, the *rate threshold* and the *perceptual threshold*, in float:
  ```
  $ python training_lromp.py TRAIN_IMAGES TABLES RATE_THRESHOLD PERCEPTUAL_THRESHOLD
  ```
 
2. Compress
  - Assume the filename of the JPEG image you want to compress is INPUT_IMAGE, and you want to name the output compressed image ROMP_IMAGE:
   ```
   /opt/libjpeg-turbo/bin/jpegtran -encode TABLES INPUT_IMAGE ROMP_IMAGE
   ```
  - for L-ROMP, specify those two additional parameters you used for training in float:
   ```
   /opt/libjpeg-turbo-lossy/bin/jpegtran -encode TABLES RATE_THESHOLD PERCEPTUAL_THRESHOLD INPUT_IMAGE ROMP_IMAGE 
   ```

3. Decompress
  - For decompression, you need to specify the folder contains the tables you used to compress image, TABLES, and the corresponding compressed image, ROMP_IMAGE; indicate the name you want to name the output decompressed JPEG image in OUTPUT_JPEG_IMAGE:
   ```
   /opt/libjpeg-turbo/bin/jpegtran -decode TABLES ROMP_IMAGE OUTPUT_JPEG_IMAGE
   ```
  - L-ROMP's decompression is same to that of ROMP, but to be consistent, you can use L-ROMP's executable:
   ```
  opt/libjpeg-turbo-lossy/bin/jpegtran -decode TABLES ROMP_IMAGE OUTPUT_JPEG_IMAGE
   ```
