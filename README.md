
# License Plate Detection

Automatic number-plate recognition is a technology that uses optical character recognition on images to read vehicle registration plates. This project uses Azure Cognitive Services to develop such a solution. License number plate recognition is capable to deter constant traffic offences. It is very helpful to security like inspections, investigations, and legal matters. Number plate recognition very helpful especially in a parking lot or at places where there are heavy traffics. The test image database contains over 500 images of the rear views of various vehicles (cars, trucks, busses), taken under various lighting conditions (sunny, cloudy, rainy, twilight, night light). This data will be used to train a custom vision object detection model and perform OCR. A typical pipeline would involve steps to: 1 Load and pre-process the image 2 Detect number plate in the image 3 Image segmentation 4 Character recognition Custom Vision will be used to develop an object detection model will be used to identify the coordinates of a vehicle's number plate in an image. This will be used to crop the image to focus on the number plate. The Read API will than use this cropped image to perform optical character recognition (OCR) to extract the number plate from the image. The approach for number plate recognition is simple yet very effective. No particular knowledge of neural network techniques is required to build a solution with this approach, as it relies on the use of cognitive services and APIs.


## The performance metrics of the trained model.

![WhatsApp Image 2022-01-30 at 9 09 23 PM](https://user-images.githubusercontent.com/61341411/152396445-863f115b-bcb1-4aac-8af2-0bb055861d91.jpeg)

Using the Quick Test function, we could validate the model against a holdout data set.
![image](https://user-images.githubusercontent.com/61341411/152396673-ddcc23d1-22f3-4cf0-a0bf-ad6c4884164e.png)

Publishing generates the URL and authentication key for the model.
![WhatsApp Image 2022-01-30 at 9 12 05 PM](https://user-images.githubusercontent.com/61341411/152407406-44e04d3d-2d87-4224-a976-7f1ab2608660.jpeg)

## Custom Vision API Call.
To make the call to custom vision API, the code first loads the image as a byte array, and using opencv library, the image is serialised into a cv2 image for diplaying
![image](https://user-images.githubusercontent.com/61341411/152410132-bfa2513e-fdea-4ca2-8556-e0243655070f.png)

The REST request must pass in the authentication key to the published custom vision model and the data array for the loaded image.
![image](https://user-images.githubusercontent.com/61341411/152410448-5bdee4cd-3133-4ff5-9ef0-d0f10da70f0d.png)
In the following image below, the respose from the REST API call is examined to find the detected object. The below code only looks at the returned hit with the highest probability.
"PLATE" was detected with a probablity of 0.99, the bounding box element of the response represents the location of the object relative to the size of the image.
![image](https://user-images.githubusercontent.com/61341411/152410970-5e8dde42-5514-43aa-943b-cf59d8adcd9d.png)

Now that the number plate is detected and located, a cropped image of the plate is saved to a new image object. The cropped plate image is then used for character recognition.
![image](https://user-images.githubusercontent.com/61341411/152413199-ad74eeaa-f4ad-444e-bd1b-b7784c7168de.png)
![image](https://user-images.githubusercontent.com/61341411/152413281-f9d3978f-da20-476d-a690-61da79a3063f.png)

## Text Recognition.
In this section, the cropped image for the detected rego plate is sent to the text extraction API.
Extracting text requires two API calls: One call to submit the image for processing, the other to retrieve the text found in the image.
First the cropped image is convered to a byte array before it is send to the text recognition API.
![image](https://user-images.githubusercontent.com/61341411/152413838-a17de99b-7420-4d7e-8abf-05dc0e9512c7.png)

The response of the text recognition API call holds the callback URL to retrieve the extracted text. This URL is saved, and then the API is polled for results.
A successful recognition result object includes the detected lines and text/words within these lines.
![image](https://user-images.githubusercontent.com/61341411/152414098-3ca0225a-56c5-4235-b9b7-d6a1c7ca67f2.png)

All is left is to extract the text from the response object.
![image](https://user-images.githubusercontent.com/61341411/152414377-f21af156-56f5-47b3-a0ae-104fc7fbdbda.png)

