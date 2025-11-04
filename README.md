# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.
 
Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program and Output:
```
import cv2
import numpy as np
import matplotlib.pyplot as plt

#Load face image
faceImage = cv2.imread('/ramya photo.jpg')
plt.imshow(faceImage[:,:,::-1]); plt.title("Face")
print("Face shape:", faceImage.shape)
```


<img width="813" height="429" alt="image" src="https://github.com/user-attachments/assets/056091f2-be87-4b90-acaf-d1342cfd764b" />


```
glassJPG = cv2.imread('/content/Screenshot 2025-09-22 212725.png')
plt.imshow(glassJPG[:,:,::-1]); plt.title("glassJPG")
print("Glass shape:", glassJPG.shape)
```


<img width="650" height="409" alt="image" src="https://github.com/user-attachments/assets/98427015-76bc-411e-bcb2-872a233ec757" />


```
glassBGR = glassJPG[:,:,0:3]
glassGray = cv2.cvtColor(glassBGR, cv2.COLOR_BGR2GRAY)
_, glassMask1 = cv2.threshold(glassGray, 240, 255, cv2.THRESH_BINARY_INV)  # detect non-white

plt.figure(figsize=[15,15])
#Show sunglasses color channels
plt.subplot(121)
plt.imshow(glassBGR[:,:,::-1])  # BGR → RGB
plt.title('Sunglass Color channels')
```

<img width="856" height="459" alt="image" src="https://github.com/user-attachments/assets/74e9fd48-19c2-4e50-9334-8677d444e670" />




```#Show generated mask
plt.subplot(122)
plt.imshow(glassMask1, cmap='gray')
plt.title('Sunglass Mask (generated)')
```


<img width="384" height="269" alt="image" src="https://github.com/user-attachments/assets/1450aac7-3dcb-467b-b6c4-fd53947295d1" />




```
import cv2
import numpy as np
import matplotlib.pyplot as plt

#Load images
faceImage = cv2.imread('/content/Screenshot 2025-09-11 114206.png')
glassJPG = cv2.imread('/content/images.jpg')

#Check if images loaded correctly
if faceImage is None or glassJPG is None:
    print("Error: Check your file paths!")
else:
    face_h, face_w, _ = faceImage.shape

    #Resize glasses to ~50% of face width
    new_w = int(face_w * 0.5)
    new_h = int(new_w * glassJPG.shape[0] / glassJPG.shape[1])
    glass_resized = cv2.resize(glassJPG, (new_w, new_h))

    #Create mask
    glass_gray = cv2.cvtColor(glass_resized, cv2.COLOR_BGR2GRAY)
    _, mask = cv2.threshold(glass_gray, 240, 255, cv2.THRESH_BINARY_INV)
    mask_inv = cv2.bitwise_not(mask)

    # Adjusted position to place glasses on eyes
    x = int(face_w * 0.27)   # x offset (centered)
    y = int(face_h * 0.09)   # y offset (move up from nose to eyes)

    #ROI on face
    roi = faceImage[y:y+new_h, x:x+new_w]

    if roi.shape[0] > 0 and roi.shape[1] > 0:
        bg = cv2.bitwise_and(roi, roi, mask=mask_inv)
        fg = cv2.bitwise_and(glass_resized, glass_resized, mask=mask)
        combined = cv2.add(bg, fg)
        faceImage[y:y+new_h, x:x+new_w] = combined

    #Show result
    plt.figure(figsize=[10,10])
    plt.imshow(cv2.cvtColor(faceImage, cv2.COLOR_BGR2RGB))
    plt.title("Face with Sunglasses")
    plt.axis("off")
    plt.show()
```


<img width="920" height="535" alt="image" src="https://github.com/user-attachments/assets/d26949ab-d025-4997-903e-eabd66729a49" />






