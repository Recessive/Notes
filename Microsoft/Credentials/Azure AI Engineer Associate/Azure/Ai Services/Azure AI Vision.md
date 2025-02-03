Has the following functionality:
- **Description and tag generation**
- **Object detection**
- **People detection**
- **Image metadata, color, and type analysis**: Also determines if it has clip art??
- **Category identification**: Category and whether it contains landmarks
- **Background removal**: Creates an *alpha matte* of foreground and background
- **Moderation rating**
- **Optical character recognition**: Read text
- **Smart thumbnail generation**: Generate thumbnail between `0.75` to `1.80` size of original image

# Application

Calling `ImageAnalysisClient` from python, you can provide the following features:
- `VisualFeatures.TAGS`: Identifies tags about the image, including objects, scenery, setting, and actions
- `VisualFeatures.OBJECTS`: Returns the bounding box for each detected object
- `VisualFeatures.CAPTION`: Generates a caption of the image in natural language
- `VisualFeatures.DENSE_CAPTIONS`: Generates more detailed captions for the objects detected
- `VisualFeatures.PEOPLE`: Returns the bounding box for detected people
- `VisualFeatures.SMART_CROPS`: Returns the bounding box of the specified aspect ratio for the area of interest
- `VisualFeatures.READ`: Extracts readable text

## Example Code:
```python
from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential

client = ImageAnalysisClient(
    endpoint=os.environ["ENDPOINT"],
    credential=AzureKeyCredential(os.environ["KEY"])
)

result = client.analyze(
    image_url="<url>",
    visual_features=[VisualFeatures.CAPTION, VisualFeatures.READ],
    gender_neutral_caption=True,
    language="en",
)
```

## Classification/Object Detection

Provision an **Azure AI Custom Vision** service. Allows for classification or object detection.


### Training (choose either)
- **Azure AI services multi-service** resource
- **Azure AI Custom Vision (Training)** resource
### Prediction (choose either)
- **Azure AI services multi-service** resource
- **Azure AI Custom Vision (Prediction)** resource

For training, you use the [Azure AI Custom Vision Portal](https://www.customvision.ai), or use the Custom Vision API. It's expected you use the GUI.

Portal allows you to:
1. Create an image classification project for your model and associate it with a training resource.
2. Upload images, assigning class label tags to them.
3. Review and edit tagged images.
4. Train and evaluate a classification model.
5. Test a trained model.
6. Publish a trained model to a prediction resource.

### Labelling:
To make labelling easier, you can use [Azure Machine Learning Studio](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-label-data) or [Microsoft Visual Object Tagging Tool (VOTT)](https://github.com/microsoft/VoTT/blob/master/README.md) and assign tasks to people to label inputs.

**Azure AI Custom Vision API** expects bounding boxes to be in the following format:

- Left: 0.1
- Top: 0.5
- Width: 0.5
- Height: 0.25
Where the values are proportions of the image.


You can also use the **Azure AI Vision** service for detecting people and bounding boxes, but it's not so great at faces specifically.

## Faces (not an **AI Vision** service, but relevant)
**Face Service** offer:
- Face detection (with bounding box).
- Comprehensive facial feature analysis (including head pose, presence of spectacles, blur, facial landmarks, occlusion and others).
- Face comparison and verification.
- Facial recognition.
Must consider the following to use this tool ethically:

- **Data privacy and security**
- **Transparency**
- **Fairness and inclusiveness**

When using the **Azure AI Vision** service, call the **Analyze Image** endpoint and specify **People** as a feature.

The features available in the face service are:
- **Face Detection**: an ID that identifies the face and a bounding box
- **Face attribute analysis**:
	- Head pose (_pitch_, _roll_, and _yaw_ orientation in 3D space)
	- Glasses (_NoGlasses_, _ReadingGlasses_, _Sunglasses_, or _Swimming Goggles_)
	- Blur (_low_, _medium_, or _high_)
	- Exposure (_underExposure_, _goodExposure_, or _overExposure_)
	- Noise (visual noise in the image)
	- Occlusion (objects obscuring the face)
	- Accessories (glasses, headwear, mask)
	- QualityForRecognition (_low_, _medium_, or _high_)
- **Facial landmark location**: Coordinates for key facial features
- **Face comparison**: Can compare across multiple images
- **Facial recognition**: Can train a model on specific people
- **Facial liveness**: Detect whether image is real or fake

### Face Comparison

Face service can be single-service or provisioned as a part of **Azure Ai Services** resource.

When a **Face Service** detects a face, it assigns it a GUID and caches for 24 hours. It can be compared to later images to verify whether this face is *similar* or to *verify* they are the same person.

You *do not* need to know who the person is, or perform any kind of training.

### Facial Recognition
1. Create a **Person Group** that defines the set of individuals you want to identify (for example, _employees_).
2. Add a **Person** to the **Person Group** for each individual you want to identify.
3. Add detected faces from multiple images to each **person**, preferably in various poses. The IDs of these faces will no longer expire after 24 hours (so they're now referred to as _persisted_ faces).
4. Train the model.

once trained, the model can be used to:

- _Identify_ individuals in images.
- _Verify_ the identity of a detected face.
- Analyze new images to find faces that are _similar_ to a known, persisted face.
## OCR (Optical character recognition)

Use one of the following 2 services for OCR:
- **Image Analysis** OCR
	- Use for smaller, unstructured documents or images
	- Immediate response from single API call
	- Can analyze beyond just text:
		- Object detection
		- Describe image
		- Gen thumbnails
	- Good for street signs, hand written notes etc
- **Document Intelligence**
	- Use for reading PDFs and images
	- Services uses context and structure of document to improve accuracy
	- Initial API call returns asynch ID which is used in subsequent calls to retrieve results
	- Good for receipts, articles, invoices etc

### Image Analysis API Call

Specify the **Read** feature:
```
https://<endpoint>/computervision/imageanalysis:analyze?features=read&...
```
In python:
```python
result = client.analyze(
    image_url=<image_to_analyze>,
    visual_features=[VisualFeatures.READ]
)
```

The results are return in json format, broken down into: Blocks, words, lines of text

## Azure Video Indexer

Used for extracting information from videos. Used for:

- **Facial Recognition** - detecting the presence of individual people in the image. This requires [Limited Access](https://aka.ms/cog-services-limited-access) approval.
- **Optical character recognition** - reading text in the video.
- **Speech transcription** - creating a text transcript of spoken dialog in the video.
- **Topics** - identification of key topics discussed in the video.
- **Sentiment** - analysis of how positive or negative segments within the video are.
- **Labels** - label tags that identify key objects or themes throughout the video.
- **Content moderation** - detection of adult or violent themes in the video.
- **Scene segmentation** - a breakdown of the video into its constituent scenes.
Can do all this through a GUI at the [Azure Video Indexer Portal](https://www.videoindexer.ai)

Can create custom models for:
- **People**
- **Language**: Train on specific phrases outside of normal vocabulary
- **Brands**: Train on specific brands
If you want to deploy any of these features, there are 2 options:

### Azure Video Indexer Widgets
All the functionality of the Azure Video Indexer Portal can be embedded in your own website as widgets.

### Azure Video Indexer API
Access the API at 
```
https://api.videoindexer.ai/Auth/<location>/Accounts/<accountId>
```
Get an Access token by appending `AccessToken` (*not 100% on this*)

Example GET:
```
https://api.videoindexer.ai/Auth/<location>/Accounts/<accountId>/Customization/CustomLogos/Logos/<logoId>?<accessToken>
```

### Deploy with Azure Resource Manager (ARM) template
Azure Resource Manager (ARM) templates are available to create the Azure AI Video Indexer resource in your subscription, based on the parameters specified in the template file.

^^^ no idea what that means

