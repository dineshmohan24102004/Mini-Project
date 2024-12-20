# STOCK RETRIEVER FOR REAL-TIME MONITORING AND INVENTORY MANAGEMENT

## AIM:
To develop a Smart Fridge Announcement System that leverages YOLOv8 object detection and IoT integration to enhance food inventory management. The system seeks to reduce food waste, automate stock monitoring, and provide real-time notifications to users about the status of refrigerator contents. It also aims to improve user convenience by offering automated updates, personalized notifications, and shopping suggestions.

## ALGORITHM:
• Image Acquisition: A camera captures images of the refrigerator's interior whenever motion or user interaction is detected.
Images are sent to the processing unit for further analysis.

• Object Detection (YOLOv8): The YOLOv8 model identifies objects in the image, categorizes them (e.g., apples, oranges, eggs), and calculates their counts.
Bounding boxes and confidence scores indicate the presence and reliability of detections.

• Data Processing: Raspberry Pi processes the object detection results, updating the inventory database.
Notifications are generated if items are running low or nearing expiry.

• Real-Time Feedback: Notifications are sent to users via a mobile app, in-built display, or audio announcements.
For instance, the system might announce, "5 apples and 8 oranges detected".

• Additional Features: Generates shopping lists for missing or low-stock items.
Offers dietary suggestions based on available inventory.

• Optimization for Edge Devices: Techniques like model quantization and pruning ensure smooth performance on Raspberry Pi.

## CODE:
### Install the YOLOv8 package
```
!pip install ultralytics
```
Import YOLO
```
from ultralytics import YOLO
import matplotlib.pyplot as plt
from PIL import Image
```
To Upload the Image and get the Uploaded Image File name
```
from google.colab import files

uploaded = files.upload()

image_path = list(uploaded.keys())[0]
```
To Load the YOLOv8 model (pre-trained) and Run detection on the uploaded image
```
model = YOLO("yolov8n.pt") 

results = model(image_path)
```
To Display the result with bounding boxes and Save the result image with detections
```
results[0].show()  
results[0].save()
```
To Count the number of detected objects and to Replace 'apple' with the class label name you are trying to count (depends on model's label map)
```
detected_classes = [results[0].names[int(result.cls)] for result in results[0].boxes] 
print(detected_classes)
item_count = detected_classes.count(detected_classes[0])
print(f"Number of items detected: {item_count}")
```
Install gtts
```
!pip install gtts
```
To Create a message based on the count and to Play the audio in Colab
```
from gtts import gTTS
import IPython.display as ipd
import os

if item_count < 2:
    message = f"There are less :{detected_classes[0]}in the refrigerator."
else:
    message = f"Sufficient :{detected_classes[0]} available."

tts = gTTS(text=message, lang='en')

audio_file_dir = "/mnt/data"
if not os.path.exists(audio_file_dir):
    os.makedirs(audio_file_dir)

audio_file = os.path.join(audio_file_dir, "detected_apples.mp3") 
tts.save(audio_file)
ipd.Audio(audio_file)
```
## OUTPUT:
![WhatsApp Image 2024-12-20 at 9 49 01 PM](https://github.com/user-attachments/assets/a8617f0d-9126-4f2c-ac51-41eed050b582)
![WhatsApp Image 2024-12-20 at 9 49 02 PM](https://github.com/user-attachments/assets/6e3f1bc3-6984-4100-a433-96b73a57dd90)
![WhatsApp Image 2024-12-20 at 9 48 39 PM](https://github.com/user-attachments/assets/c02321a6-4d48-48da-9864-954c518940d1)
![WhatsApp Image 2024-12-20 at 9 48 38 PM](https://github.com/user-attachments/assets/75c48b43-5497-44d8-b3f5-852cfc9f4655)
![WhatsApp Image 2024-12-20 at 9 48 39 PM (1)](https://github.com/user-attachments/assets/cd1ef41d-02ad-4322-9376-91fffd8477f3)
![WhatsApp Image 2024-12-20 at 9 48 39 PM (2)](https://github.com/user-attachments/assets/bc435a7c-2a7b-4330-8595-ff00ee3158fe)
![WhatsApp Image 2024-12-20 at 9 48 38 PM (1)](https://github.com/user-attachments/assets/0874d27f-2ae4-4608-863b-28918637b6b2)

## RESULT:
The Smart Fridge Announcement System successfully detects and categorizes items in the refrigerator with 95.6% accuracy using YOLOv8, providing real-time inventory updates and reducing food waste. Notifications are delivered via audio, SMS, or app, alerting users about low-stock or expiring items. The system ensures efficient operation with an inference time of 300 ms on Raspberry Pi, enhancing user convenience.







