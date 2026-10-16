## Name: SHEETAL R
## Reg no: 212223230206

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

Feel free to fork, contribute, or customize this project for your creative needs!

## Program & Output:
```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```
```
# Load the Face Image
faceImage = cv2.imread('my.jpg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
```
<img width="273" height="433" alt="download" src="https://github.com/user-attachments/assets/f4f30aa7-7061-4f3b-b1d6-138098001051" />


```
faceImage.shape
```
(1600, 1244, 3)

```
#resized_faceImage.shape
faceImage.shape
```
(1600, 1244, 3)
```
# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('glass.jpg',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```

Text(0.5, 1.0, 'glassPNG')

<img width="552" height="280" alt="download" src="https://github.com/user-attachments/assets/02cb41d9-e494-4431-b916-066bab31d359" />


```
# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(820,300))
print("image Dimension ={}".format(glassPNG.shape))
```
image Dimension =(300, 820, 3)

```
# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,:3]
print(glassPNG.shape)
```
(300, 820, 3)

```
# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```
<img width="1219" height="257" alt="download" src="https://github.com/user-attachments/assets/9d19786b-0d80-44cc-be1d-0cbf0bdf28f0" />

```
# Resize the glasses
glassBGR = cv2.resize(glassBGR, (500, 170))

# Copy the image
faceWithGlassesNaive = faceImage.copy()

# Adjust the position
x = 398    # Left/Right
y = 520    # Up/Down

# Place the glasses
faceWithGlassesNaive[y:y+170, x:x+500] = glassBGR

# Display
plt.imshow(faceWithGlassesNaive[:,:,::-1])
plt.axis("off")
plt.show()
```
<img width="367" height="636" alt="download" src="https://github.com/user-attachments/assets/bb61bf30-4e8f-4e6b-8283-0c6fddcc2995" />


```
# Resize glasses
w = 500
h = 170

glassBGR = cv2.resize(glassBGR, (w, h))

# Create mask from white background
gray = cv2.cvtColor(glassBGR, cv2.COLOR_BGR2GRAY)

_, glassMask = cv2.threshold(gray, 240, 255, cv2.THRESH_BINARY_INV)

glassMask = cv2.merge((glassMask, glassMask, glassMask))
glassMask = glassMask.astype(np.float32) / 255.0

# Copy image
faceWithGlassesArithmetic = faceImage.copy()

# Position
x = 398
y = 520

# ROI
eyeROI = faceWithGlassesArithmetic[y:y+h, x:x+w].astype(np.float32)

glassBGR = glassBGR.astype(np.float32)

# Arithmetic operations
maskedEye = eyeROI * (1 - glassMask)
maskedGlass = glassBGR * glassMask

eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display intermediate images
plt.figure(figsize=(20,8))

plt.subplot(131)
plt.imshow(np.uint8(maskedEye)[:,:,::-1])
plt.title("Masked Eye Region")

plt.subplot(132)
plt.imshow(np.uint8(maskedGlass)[:,:,::-1])
plt.title("Masked Sunglass Region")

plt.subplot(133)
plt.imshow(np.uint8(eyeRoiFinal)[:,:,::-1])
plt.title("Augmented Eye and Sunglass")

plt.show()

# Put result back into face
faceWithGlassesArithmetic[y:y+h, x:x+w] = np.uint8(eyeRoiFinal)
```
<img width="1615" height="1312" alt="download" src="https://github.com/user-attachments/assets/e20082db-7016-4973-a102-bdbd8c64c176" />


```
plt.figure(figsize=(20,20))

plt.subplot(121)
plt.imshow(faceImage[:,:,::-1])
plt.title("Original Image")

plt.subplot(122)
plt.imshow(faceWithGlassesArithmetic[:,:,::-1])
plt.title("With Sunglasses")

plt.show()
```
<img width="273" height="433" alt="download" src="https://github.com/user-attachments/assets/cff23ee5-86d4-4471-82e7-06c42151ff76" />
