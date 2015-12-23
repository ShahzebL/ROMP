# Introduction
ROMP/L-ROMP are novel techniques that compresses JPEG images. ROMP/L-ROMP defines finer-grained contexts than standard JPEG and compress each context individually.

# Install
```
 - $ cd romp (or cd lromp for L-ROMP)
 - $ ./configure
 - $ sudo make install
   - Note that, ROMP executable will be installed to /opt/libjpeg-turbo, and L-ROMP will be installed to /opt/libjpeg-turbo-lossy
```

# Usage
ROMP/L-ROMP needs to a special Huffman table for each defined context. Therefore, to use ROMP/L-ROMP, you need to train these tables first, using a set of training images (training step). ROMP/L-ROMP learn statistics from these training images, and then use these trained tables to compress other images.
```
a
b
c
```

1. Training step:
  - Prepare a set of training images, put them into one folder named, e.g., TRAIN_IMAGES
  - $ cd training
  - for ROMP: $ python training_romp.py TRAIN_IMAGES TABLES
    - TABLES is the name of the folder that will be generated with trained tables
  - for L-ROMP: $ python training_lromp.py TRAIN_IMAGES TABLES RATE_THRESHOLD PERCEPTUAL_THRESHOLD
    - L-ROMP's training takes two additional parameters, the rate threshold and the perceptual threshold, in float.
    
2. Compress
  - for ROMP: /opt/libjpeg-turbo/bin/jpegtran -encode TABLES INPUT_IMAGE ROMP_IMAGE
    - TABLES is the folder contains trained tables, INPUT_IMAGE is the JPEG image you want to compress, ROMP_IMAGE is the compressed image
  - for L-ROMP: /opt/libjpeg-turbo-lossy/bin/jpegtran -encode TABLES RATE_THESHOLD PERCEPTUAL_THRESHOLD INPUT_IMAGE ROMP_IMAGE 
    - L-ROMP's compressing takes two additional parameters, the rate threshold and the perceptual threshold, in float.
    
3. Decompress
  - for ROMP: /opt/libjpeg-turbo/bin/jpegtran -decode TABLES ROMP_IMAGE OUTPUT_JPEG_IMAGE
    - TABLES is the folder contains trained tables, ROMP_IMAGE is the compressed image, OUTPUT_JPEG_IMAGE is the decompressed JPEG image
  - for L-ROMP: /opt/libjpeg-turbo-lossy/bin/jpegtran -decode TABLES ROMP_IMAGE OUTPUT_JPEG_IMAGE
