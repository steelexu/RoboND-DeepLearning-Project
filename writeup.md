# writeup

## network architecture flow as

* inputs shape  (?, 160, 160, 3)
* encoder1 shape  (?, 80, 80, 16)
* encoder2 shape  (?, 40, 40, 64)
* encoder2x shape  (?, 20, 20, 128)
* encoder2xx shape  (?, 10, 10, 512)
* 1x1 conv shape  (?, 10, 10, 256)
* decoder4xx shape  (?, 20, 20, 128)
* decoder4x shape  (?, 40, 40, 64)
* decoder4 shape  (?, 80, 80, 32)
* decoder5 shape  (?, 160, 160, 16)
* outputs shape  (?, 160, 160, 3)

[image_0]: ./fcn-arch.png
![see diagram][image_0] 


## concept
* mask image as training objective(batch_y), extracted feature help to segment the image in different resolution
* fully connected layer is just for classification, useless for getting mask of segmentation , replaced here by the decoder layers
* encoder and decoder architecture, encoding the images featues by multiple filters, decoding with the help from previous encoder layers raw output(skip connection)
  * encoder using series of convolution layers for extracting features, reduced to a deeper 1x1 convolution layer
  * decoder using transposed convolution layers to upsample the image(recovering to original size)
  * skip connection used  for adding infomation from multiple resolution	
  * 1x1 conv to do dimension reduction for computation efficiency, also has the effect of preserving spacial information from the image.



## Hyper-parameters

* learning rate for optimization speed
* each epoch run through all image set, split by batch_size

at first I am using rate from 0.01 to do 2x10 epochs,later change batch_size, then decrease it to 0.005 , recoding see [./runs-3]

learning_rate = 0.01
batch_size = 64
num_epochs = 20
steps_per_epoch = 70
validation_steps = 50
workers = 2

learning_rate = 0.01
batch_size = 80
num_epochs = 10
steps_per_epoch = 60
validation_steps = 50
workers = 2


learning_rate = 0.005
batch_size = 80
num_epochs = 10
steps_per_epoch = 60
validation_steps = 50
workers = 2



## tunning process
* inital last encoder just using 256 kernel
* i have used the image size as 256 changed from 160, but my 4G gpu memory can't stand up to it, back to 160
* using more deeper network than now , but no final score improved, when it is arround 0.38
* add layer encoder2xx shape  (?, 10, 10, 512), and epochs changed from 50 to 100, score be around 0.41
* in the training, I will rerun the fit_generator cell from previous training, or interupt the cell, or reload the weights after restarting jupyter
* re-run the training for the [recording](./runs-3)

## data and improvement
* here I just use the given dataset, and do flipping on some images
* I am trying to collect the images from simulator, targeting to score 0.45
* if we need to track other kind of object, collecting data should be targeted to such object, followed by another training
* Todo: collect more far-away-target image, now number-false-negatives seems too large
* Todo, use less skip than now , need an experiment

## followme
* using the trained weights, the simulator can successfully track target
