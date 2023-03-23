# ML 2: Mysterious Learnings
> "One day the archeologist came across a strange metal plate covered in uncommon hieroglyphics. It looked like blueprints for some kind of alien technology. "What kind of magic is this?" He studied the plate more closely and was amazed by the advanced technology and incredible engineering they were using at a time like this. This could only lead him in him wanting to learn more...
"


#### Difficulty: easy

We are given a zip file that contains a file: `alien.h5`. From some basic Googling, this is an HDF5 file and it can be opened using something like HDFView or loaded as a model using keras.

On a new Google CoLab notebook, I uploaded `alien.h5` and imported keras and loaded the model:

```
from tensorflow import keras
model = keras.models.load_model('alien.h5')
```

Loading the model gave this output:
`SFRCe24wdF9zb`

To know more about the model, I tried checking the summary, which contained:

```
Model: "19oNHJkX3RvX3V"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 conv2d_3 (Conv2D)           (None, 30, 30, 32)        896       
                                                                 
 max_pooling2d_2 (MaxPooling  (None, 15, 15, 32)       0         
 2D)                                                             
                                                                 
 conv2d_4 (Conv2D)           (None, 13, 13, 64)        18496     
                                                                 
 max_pooling2d_3 (MaxPooling  (None, 6, 6, 64)         0         
 2D)                                                             
                                                                 
 conv2d_5 (Conv2D)           (None, 4, 4, 64)          36928     
                                                                 
 uZDNyc3Q0bmR9 (Lambda)      (None, 4, 4, 64)          0         
                                                                 
 flatten_1 (Flatten)         (None, 1024)              0         
                                                                 
 dense_2 (Dense)             (None, 64)                65600     
                                                                 
 dense_3 (Dense)             (None, 10)                650       
                                                                 
 lambda_1 (Lambda)           (None, 10)                0         
                                                                 
=================================================================
Total params: 122,570
Trainable params: 122,570
Non-trainable params: 0
_________________________________________________________________
```

Upon noticing that I now have 3 gibberish-looking strings (`SFRCe24wdF9zb`, `19oNHJkX3RvX3V`, `uZDNyc3Q0bmR9`) I decided to concatenate them and convert to base64 afterwards, which gave me the flag:

`HTB{n0t_so_h4rd_to_und3rst4nd}`