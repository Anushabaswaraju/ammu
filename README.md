## bounding boxes
```


import os
import csv
from PIL import Image, ImageDraw
The script imports necessary modules: os for interacting with the operating system, csv for reading CSV files, and Image and ImageDraw from the PIL library for image processing.

csv_file = "/home/anusha-baswaraju/Downloads/7622202030987_bounding_box.csv"
image_dir = "/home/anusha-baswaraju/Downloads/7622202030987"
output_dir = "/home/anusha-baswaraju/Downloads/7622202030987_with_boxes"
These variables store the paths to the CSV file containing bounding box coordinates, the directory containing the images, and the directory where the output images with drawn boxes will be saved.

os.makedirs(output_dir, exist_ok=True)
This line creates the output directory if it doesn't exist already..

def draw_boxes(image, boxes):
    draw = ImageDraw.Draw(image)
    for box in boxes:
        left = int(box['left'])
        top = int(box['top'])
        right = int(box['right'])
        bottom = int(box['bottom'])
        draw.rectangle([left, top, right, bottom], outline="red")
    return image
This function takes an image and a list of bounding boxes as input and draws rectangles around each box on the image using the ImageDraw module.

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
This function crops the image based on the bounding box coordinates and returns a list of cropped images.

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
```
This block of code reads the CSV file using csv.DictReader, iterates over each row, opens the corresponding image using Image.open, draws boxes around the objects in the image, and saves the modified image with the drawn boxes in the output directory. 

## histrogram
```

##import numpy as np
import cv2 as cv
from matplotlib import pyplot as plt
 The script imports necessary modules: cv2 from OpenCV for image processing and pyplot from Matplotlib for plotting.

img = cv.imread('/home/anusha-baswaraju/Downloads/deepikap.jpg')
cv.imwrite("/home/anusha-baswaraju/Downloads/ranver.jpg",img)
This line ensures that the image has been read successfully. If the image is not read, it raises an AssertionError with the provided message.
assert img is not None, "file could not be read, check with os.path.exists()"

color = ('b','g','r')
for i,col in enumerate(color):
 histr = cv.calcHist([img],[i],None,[256],[0,256])
 plt.plot(histr,color = col)
 plt.xlim([0,256])
plt.show()
This loop calculates and plots the histograms for each color channel (blue, green, and red) of the image. It uses the cv.calcHist() function to compute the histogram for each channel. Then, it plots the histograms using Matplotlib. The plt.xlim([0,256]) line sets the x-axis limit to range from 0 to 256 (the possible pixel intensity values). Finally, plt.show() displays the plotted histograms.```

## tasksum 
```
This function iterates over a range of numbers from 1 to 10 (inclusive) using a for loop.

def print_sum_of_previous_and_current():
    previous_number = 0
    for i in range(1, 11):
        current_number = i
        
        Within each iteration of the loop, it assigns the current value of the loop variable i to the variable current_number
        
        sum_of_previous_and_current = previous_number + current_number
        It then calculates the sum of the previous number (initialized to 0 in the first iteration) and the current number.
        The function prints out the current number, the previous number, and the sum of the two numbers in a formatted string.
        
        print(f"Current Number: {current_number}, Previous Number: {previous_number}, Sum: {sum_of_previous_and_current}")
        previous_number = current_number

print_sum_of_previous_and_current()
This line calls the function to execute the defined logic.
```
    
## Video
```
import cv2
The script imports the OpenCV module, which provides functions for video capture and processing.
def capture_video():
    # Open the default camera (usually webcam)
    cap = cv2.VideoCapture(0)
    
This function capture_video() starts by creating a VideoCapture object cap, which opens the default camera (index 0, usually the webcam). If you have multiple cameras, you can specify a different index to select a different camera.

    # Check if camera opened successfully
    if not cap.isOpened():
        print("Error: Couldn't open camera")
        return
This conditional statement checks if the camera is opened successfully. If not, it prints an error message and exits the function using return.
    while True:
        # Capture frame-by-frame
        ret, frame = cap.read()
Within a continuous loop (while True), the script captures frames from the camera one by one using the cap.read() function. The function returns a boolean value ret indicating whether a frame was successfully captured and the actual frame frame.

        # Display the resulting frame
  Each captured frame is displayed in a window titled 'Video Capture' using cv2.imshow().      cv2.imshow('Video Capture', frame)

        # Wait for 'q' key to stop the video capture
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
The script waits for the user to press the 'q' key. If the 'q' key is pressed, the loop breaks, and the video capture stops.

    # Release the capture
    cap.release()
    cv2.destroyAllWindows()
After exiting the loop, the script releases the camera capture using cap.release() and closes all OpenCV windows using cv2.destroyAllWindows().


capture_video()
Finally, this line calls the capture_video() function to start the video capture process.
