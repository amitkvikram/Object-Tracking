<h1 align = "middle">Object Tracking</h1>

### Aim
Repo contains a notebook in which I have implemented a basic object tracking model. Goal is to track an object based on its colour information using openCv.

### Procedure
First we will record a video, while recording we draw a rectangle around the object 
to be tracked. Then we press Esc key.After that a new window will open 
which will show the selected area. Then pressing Esc key closes current window and 
Three new window will open. One window is current recording, window named "mask" is our created
mask, third window named "Tracking" will show our tracking result.

### Theory

* The feature which we will use for tracking is color information. 
* When we select the object to be tracked(we will call it as roi(region of interest)) it is in 
  BGR. First we convert roi from BGR to HSV colorspace. We will discuss later why we convert
  colorspace to hsv, till then think it same as BGR.
* Next task is to find the color range to be tracked. For this we calculate mean and standard
  deviation of each channel(H,S,V) individually for roi. Then our color range is simply 
  ( [H_mean-H_std, H_mean+H_std], [S_mean-S_std, S_mean+S_std], [V_mean-V_std, V-mean+V_std] ).
* Let us define 
                lower_bound = (H_mean - H_std, S_mean - S_std, V_mean - V_std) and 
                upper_bound = (H_mean + H_std, S_mean + S_std, V_mean + V_std)
* Now to create mask we will use cv2.inRange function available in openCv library. cv2.inRange   function takes frame(current frame in video), lower_bound , upper_bound. It return an array 
  of same size as frame in which a pixel value in 255 if corresponding pixel value in frame 
  is in range [lower_bound,upper_bound] otherwise pixel value is zero.
* After creating mask, to track the object we take bitwise_and of frame and mask. When we take 
  bitwise and of frame and mask in our ouput we will get pixel value 0 where pixel value of
  mask is 0.Reason behind it is simple as each pixel is a 8-bit unsigned int, 0 means all 8 
  bits are zero, now when we take bitwise and of 0 with any bit result is zero.So all 8 bits
  in ouput, hence the pixel value will be 0. AND output pixel value will be same as pixel value 
  of frame where pixel value of mask is 255. Reason is same.
* In the window named "Tracking" we can see our tracking result.
* Why we need to change colorspace from BGR to HSV(or YCrCb)
    * Problem with BGR is mixing of chrominance(Color related information) and luminance( 
      Intensity related information) data. Suppose you have an image of a single-color plane with a shadow on it. In RGB colorspace, the shadow part will most likely have very different characteristics than the part without shadows. In HSV colorspace, the hue component of both patches is more likely to be similar. Since we are tracking object based on its color(and intensity) information, we should be able to distinguish color information from luminance.
    * In HSV three channel represents Hue(Primary Color or dominant wavelength), Saturation( shades of the color ) and value(intensity).So it separates the image intensity from the color information. Since we are trying to track object base on its color range this is really useful.YCrCb colorspace is also useful for same.
    
### Conclusion
HSV and YCrCb both colorspace perform almost same. Also we don't see any significant improvement of changing BGR to HSV or YCrCb.
 
