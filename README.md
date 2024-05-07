## bounding boxes
import os
import csv
from PIL import Image, ImageDraw


csv_file = "/home/anusha-baswaraju/Downloads/7622202030987_bounding_box.csv"
image_dir = "/home/anusha-baswaraju/Downloads/7622202030987"
output_dir = "/home/anusha-baswaraju/Downloads/7622202030987_with_boxes"


os.makedirs(output_dir, exist_ok=True)


def draw_boxes(image, boxes):
    draw = ImageDraw.Draw(image)
    for box in boxes:
        left = int(box['left'])
        top = int(box['top'])
        right = int(box['right'])
        bottom = int(box['bottom'])
        draw.rectangle([left, top, right, bottom], outline="red")
    return image


def crop_image(image, boxes):
    cropped_images = []
    for box in boxes:
        left = int(box['left'])
        top = int(box['top'])
        right = int(box['right'])
        bottom = int(box['bottom'])
        cropped_img = image.crop((left, top, right, bottom))
        cropped_images.append(cropped_img)
    return cropped_images


with open(csv_file, 'r') as file:
    csv_reader = csv.DictReader(file)
    for row in csv_reader:
        image_name = row['filename']
        image_path = os.path.join(image_dir, image_name)
        output_path = os.path.join(output_dir, image_name)
        image = Image.open(image_path)
        boxes = [{'left': row['xmin'], 'top': row['ymin'], 'right': row['xmax'], 'bottom': row['ymax']}]
        cropped_images = crop_image(image, boxes)
        for i, cropped_img in enumerate(cropped_images):
            cropped_img.save(os.path.join(output_dir, f"{i}_{image_name}"))  
        full_image_with_boxes = draw_boxes(image, boxes)
        full_image_with_boxes.save(os.path.join(output_dir, f"full_{image_name}"))
        
## histrogram

##import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt
 
img = cv.imread('/home/anusha-baswaraju/Downloads/deepikap.jpg')
cv.imwrite("/home/anusha-baswaraju/Downloads/ranver.jpg",img)
assert img is not None, "file could not be read, check with os.path.exists()"
color = ('b','g','r')
for i,col in enumerate(color):
 histr = cv.calcHist([img],[i],None,[256],[0,256])
 plt.plot(histr,color = col)
 plt.xlim([0,256])
plt.show()

## tasksum 

def print_sum_of_previous_and_current():
    previous_number = 0
    for i in range(1, 11):
        current_number = i
        sum_of_previous_and_current = previous_number + current_number
        print(f"Current Number: {current_number}, Previous Number: {previous_number}, Sum: {sum_of_previous_and_current}")
        previous_number = current_number

print_sum_of_previous_and_current()
    
## video
import cv2

def capture_video():
    # Open the default camera (usually webcam)
    cap = cv2.VideoCapture(0)

    # Check if camera opened successfully
    if not cap.isOpened():
        print("Error: Couldn't open camera")
        return

    while True:
        # Capture frame-by-frame
        ret, frame = cap.read()

        # Display the resulting frame
        cv2.imshow('Video Capture', frame)

        # Wait for 'q' key to stop the video capture
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    # Release the capture
    cap.release()
    cv2.destroyAllWindows()

capture_video()
