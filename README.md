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

### 1. Capture and store the background frame.
> cap = cv2.VideoCapture(0)
