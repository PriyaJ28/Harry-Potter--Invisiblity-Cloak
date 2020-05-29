# Harry-Potter--Invisiblity-Cloak
 ‘Invisibility Cloak’ using Basic computer vision techniques in OpenCV.
 
 
## Introduction:
If you are a Harry Potter fan like me, you would know what an Invisibility Cloak is. 
Yes! It’s the cloak which Harry Potter uses to become invisible. Of course, we all know that an invisibility cloak is not real — it’s all graphics trickery.

## How does it work ?
The algorithm is very similar in principle to green screening. But unlike green screening where we remove the background, in this application, we remove the foreground!

The basic idea is given below:

>1. Capture and store the background frame. (stay away from the source first so that background is perfectly captured.
>2. Detect the red colored cloth using color detection algorithm.
>3. Segment out the red colored cloth by generating a mask.
>4. Generate the final augmented output to create the magical effect.

* **Key Idea** : To replace the current frame pixels corresponding to the cloth with the background pixels to generate the effect of an invisibility cloak.

### 1. Capture and store the background frame.
```
cap = cv2.VideoCapture(0)
time.sleep(3)

backgrd = 0

for i in range(30):
    _, backgrd = cap.read()

#Laterally flip the image
backgrd = np.flip(backgrd,axis = 1)

```
Averaging over multiple frames also reduces noise.

### 2. Detect the red colored cloth using color detection algorithm.
We have an RGB (Red-Green-Blue) image and it is tempting to simply threshold the R channel and get our mask. It turns out that this will not work effectively since the RGB values are highly sensitive to illumination. Hence even though the cloak is of red color there might be some areas where, due-to shadow, Red channel values of the corresponding pixels are quite low.

The right approach is to transform the color space of our image from RGB to HSV (Hue – Saturation – Value).

```
hsv = cv2.cvtColor(img,cv2.COLOR_BGR2HSV)

```
you can change color bound according to the color you want to mask. The color range for HSV can be accessed from ![here](http://colorizer.org/).

```
    #range for lower red
    lower_red = np.array([0,120,70])
    upper_red = np.array([10,255,255])
    mask1 = cv2.inRange(hsv, lower_red,upper_red)

    #range for lower red
    lower_red = np.array([170,120,70])
    upper_red = np.array([180,255,255])
    mask2 = cv2.inRange(hsv, lower_red,upper_red)

    #Generation of final mask to detect red color
    mask = mask1+mask2

```
The inRange function simply returns a binary mask.

### 3: Segmenting out the detected red colored cloth

In the previous step, we generated a mask to determine the region in the frame corresponding to the detected color. We refine this mask and then use it for segmenting out the cloth from the frame. 
```
mask1 = cv2.morphologyEx(mask, cv2.MORPH_OPEN, np.ones((3, 3), np.uint8)) 
mask1 = cv2.morphologyEx(mask, cv2.MORPH_DILATE, np.ones((3,3),np.uint8)) 
mask2 = cv2.bitwise_not(mask1) 

```
### 4: Generating the final augmented output to create a magical effect.

Finally, we replace the pixel values of the detected red color region with corresponding pixel values of the static background and finally generate an augmented output which creates the magical effect, converting our cloth into an invisibility cloak.

To do this we use bitwise_and operation first to create an image with pixel values, corresponding to the detected region, equal to the pixel values of the static background and then add the output to the image (res1) from which we had segmented out the red cloth.

```
res1 = cv2.bitwise_and(img,img,mask=mask2)
res2 = cv2.bitwise_and(backgrd, backgrd, mask = mask1)

final_output = cv2.addWeighted(res1, 1, res2, 1, 0) 
cv2.imshow("INVISIBLE MAN", final_output) 
```

So now we are all ready to create your own invisibility cloak. ENJOY!!!! :)

## References:

1. https://docs.opencv.org/master/d0/d86/tutorial_py_image_arithmetics.html
2. https://pysource.com/2019/02/15/detecting-colors-hsv-color-space-opencv-with-python/





