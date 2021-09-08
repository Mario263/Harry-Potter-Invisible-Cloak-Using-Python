# Harry Potter Invisible Cloak Using Python
 
 
 1. Imported libraries
2. Captured the background for 3 seconds and saved it in a variable.

3. Captured the video feed and converted it to HSV

4. Setting value for red color and then made a mask for it

5. Combining two layers/masks so that it can be viewed as one frame

6. After combining we will store it in default mask

# The code for the following is 

     #importing libraries
     import numpy as np
     import cv2
     import time

     cap = cv2.VideoCapture(0)
     time.sleep(2)
     background = 0

     #capturing the background
     for i in range(50):
         ret, background = cap.read()
     #capturing the video feed and converitng img/background to hsv
     while(cap.isOpened()):
         ret, img = cap.read()
         if not ret:
             break
         hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
     #setting the value for cloak and removing the color and making mask

         lower_red = np.array([0,120,70])
         upper_red = np.array([10,255,255]) # value is for red color cloth
         mask1 = cv2.inRange(hsv, lower_red, upper_red)
         lower_red = np.array([170,120,70])
         upper_red = np.array([180,255,255])

         mask2 = cv2.inRange(hsv, lower_red, upper_red)
         #combining the two masks so that it can be viewed as one frame
         mask1 = mask1+mask2
         #after we are done with combining we will store it in the default mask
         mask1 = cv2.morphologyEx(mask1, cv2.MORPH_OPEN, np.ones((3,3), np.uint8), iterations = 2)
         mask1 = cv2.morphologyEx(mask1, cv2.MORPH_DILATE, np.ones((3,3), np.uint8), iterations = 1)

         mask2 = cv2.bitwise_not(mask1)

         res1 = cv2.bitwise_and(background,background,mask=mask1)

         #basic work of bitwise_and is to combine the background and store it in res1

         res2 = cv2.bitwise_and(img,img,mask=mask2)
         final_output = cv2.addWeighted(res1,1,res2,1,0)
         cv2.imshow('Invisible cloak', final_output)
         k = cv2.waitKey(10)
         if k==27:
             break

     cap.release()
     cv2.destroyAllWindows()
