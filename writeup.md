#writeup

## network architecture flow as

inputs shape  (?, 160, 160, 3)
encoder1 shape  (?, 80, 80, 16)
encoder2 shape  (?, 40, 40, 64)
encoder2x shape  (?, 20, 20, 128)
encoder2xx shape  (?, 10, 10, 512)
1x1 conv shape  (?, 10, 10, 256)
decoder4xx shape  (?, 20, 20, 128)
decoder4x shape  (?, 40, 40, 64)
decoder4 shape  (?, 80, 80, 32)
decoder5 shape  (?, 160, 160, 16)
outputs shape  (?, 160, 160, 3)

## concept
* fully connected layer is just for classification, useless for getting mask of segmentation , replaced here by the decoder layers
* encoder and decoder architecture, encoding the images featues by multiple filters, decoding with the help from previous encoder layers raw output(skip connection)
* 1x1 conv to do dimension reduction for computation efficiency



## Hyper-parameters

* learning rate for optimization speed
* each epoch run through all image set, split by batch_size

at first I am using rate from 0.01 to do 10 epochs, then decrease it to 0.005 for more than 100 epochs,

learning_rate = 0.01
batch_size = 64
num_epochs = 10
steps_per_epoch = 70
validation_steps = 50
workers = 2

learning_rate = 0.005
batch_size = 80
num_epochs = 100
steps_per_epoch = 55
validation_steps = 50
workers = 2



## tunning process
* inital last encoder just using 256 kernel
* i have used the image size as 256 changed from 160, but my 4G gpu memory can't stand up to it, back to 160
* using more deeper network than now , but no final score improved, when it is arround 0.38
* add layer encoder2xx shape  (?, 10, 10, 512), and epochs changed from 50 to 100, score be around 0.41
* in the training, I will continue  run the cell from previous training, or interupt the cell, or reload the weights after restarting jupyter

## data 
* here I just use the given dataset, and do flipping on some images
* I am trying to collect the images from simulator, targeting to score 0.45
* if we need to track other kind of object, collecting data should be targeted to such object, followed by another training


* followme
# using the trained weights, the simulator can successfully track target
