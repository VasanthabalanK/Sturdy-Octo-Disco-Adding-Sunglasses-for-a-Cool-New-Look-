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

## Program
```
import cv2
import numpy as np

# =========================================================
# 1. Load the original passport-size photo
# =========================================================

image = cv2.imread("original_passport_photo.jpg")

if image is None:
    print("Error: Original image not found.")
    exit()

# Display original image
cv2.imshow("Original Image", image)
cv2.waitKey(1000)
cv2.destroyAllWindows()


# =========================================================
# 2. Load the sunglasses image
#    IMREAD_UNCHANGED keeps the alpha channel
# =========================================================

sunglasses = cv2.imread(
    "sunglasses.png",
    cv2.IMREAD_UNCHANGED
)

if sunglasses is None:
    print("Error: Sunglasses image not found.")
    exit()

print("Original image size:", image.shape)
print("Sunglasses size:", sunglasses.shape)


# =========================================================
# 3. Identify the approximate eye coordinates
#    Coordinates are for the supplied passport photo
# =========================================================

left_eye = (219, 256)
right_eye = (319, 256)

print("Left eye:", left_eye)
print("Right eye:", right_eye)


# =========================================================
# 4. Calculate distance between the two eyes
# =========================================================

eye_distance = int(
    np.linalg.norm(
        np.array(right_eye) - np.array(left_eye)
    )
)

print("Eye distance:", eye_distance)


# =========================================================
# 5. Resize sunglasses according to eye distance
# =========================================================

target_width = int(eye_distance * 2.12)

scale = target_width / sunglasses.shape[1]

target_height = int(
    sunglasses.shape[0] * scale
)

sunglasses = cv2.resize(
    sunglasses,
    (target_width, target_height),
    interpolation=cv2.INTER_AREA
)

print("Resized sunglasses:", sunglasses.shape)


# =========================================================
# 6. Calculate the position of sunglasses
# =========================================================

center_x = int(
    (left_eye[0] + right_eye[0]) / 2
)

center_y = int(
    (left_eye[1] + right_eye[1]) / 2
)

x1 = center_x - target_width // 2
y1 = center_y - target_height // 2 + 2

x2 = x1 + target_width
y2 = y1 + target_height


# =========================================================
# 7. Check image boundaries
# =========================================================

x1_clip = max(x1, 0)
y1_clip = max(y1, 0)

x2_clip = min(x2, image.shape[1])
y2_clip = min(y2, image.shape[0])


# Coordinates inside sunglasses image

sx1 = x1_clip - x1
sy1 = y1_clip - y1

sx2 = sx1 + (x2_clip - x1_clip)
sy2 = sy1 + (y2_clip - y1_clip)


# =========================================================
# 8. Extract Region of Interest (ROI)
# =========================================================

roi = image[
    y1_clip:y2_clip,
    x1_clip:x2_clip
]

glass_roi = sunglasses[
    sy1:sy2,
    sx1:sx2
]


# =========================================================
# 9. Extract Alpha Channel
# =========================================================

alpha = glass_roi[:, :, 3]


# =========================================================
# 10. Create binary mask
# =========================================================

mask = np.where(
    alpha > 10,
    255,
    0
).astype(np.uint8)

# Inverse mask
mask_inv = cv2.bitwise_not(mask)


# =========================================================
# 11. Mask the original image
# =========================================================

background = cv2.bitwise_and(
    roi,
    roi,
    mask=mask_inv
)


# =========================================================
# 12. Mask the sunglasses
# =========================================================

foreground = cv2.bitwise_and(
    glass_roi[:, :, :3],
    glass_roi[:, :, :3],
    mask=mask
)


# =========================================================
# 13. Combine the images
#    Arithmetic operation
# =========================================================

result = cv2.add(
    background,
    foreground
)


# =========================================================
# 14. Put result back into original image
# =========================================================

image[
    y1_clip:y2_clip,
    x1_clip:x2_clip
] = result


# =========================================================
# 15. Display final image
# =========================================================

cv2.imshow(
    "Sunglasses Overlay",
    image
)

cv2.waitKey(0)
cv2.destroyAllWindows()


# =========================================================
# 16. Save final output
# =========================================================

cv2.imwrite(
    "passport_photo_with_sunglasses.jpg",
    image
)

print("======================================")
print("Sunglasses overlay completed!")
print("Output saved as:")
print("passport_photo_with_sunglasses.jpg")
print("======================================")
```
## output
<img width="413" height="531" alt="photo" src="https://github.com/user-attachments/assets/41fefbd9-95d0-433e-9373-21ce8c2f8bb2" />
<img width="413" height="531" alt="sunglass overlay" src="https://github.com/user-attachments/assets/26f1caf0-fe98-4456-9478-818a3501cf84" />



Feel free to fork, contribute, or customize this project for your creative needs!
