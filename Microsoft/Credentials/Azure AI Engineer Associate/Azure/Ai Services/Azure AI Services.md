A set of services that provide AI capabilities.

The services are as follows:

| [[Azure AI Language\|Natural language processing]] | Knowledge mining and document intelligence | [[Azure AI Vision\|Computer vision]] | Decision support   | Generative AI           |
| -------------------------------------------------- | ------------------------------------------ | ------------------------------------ | ------------------ | ----------------------- |
| Text analysis                                      | AI Search                                  | Image analysis                       | Content safety     | Azure OpenAI Service    |
| Question answering                                 | Document Intelligence                      | Video analysis                       | Content moderation | DALL-E image generation |
| Language understanding                             | Custom Document Intelligence               | Image classification                 |                    |                         |
| Translation                                        | Custom skills                              | Object detection                     |                    |                         |
| Named entity recognition                           |                                            | Facial analysis                      |                    |                         |
| Custom text classification                         |                                            | Optical character recognition        |                    |                         |
| Speech                                             |                                            | Azure AI Video Indexer               |                    |                         |
| Speech Translation                                 |                                            |                                      |                    |                         |

# Provisioning a service

Create a resource to define an endpoint where:
- a [[Azure AI Services||service]] can be consumed
- provide access keys for authenticated access
- manage billing

You have the following provision options:
- **Multi-service resource**: Provision an [[Azure AI Services||AI Services]] resource that supports multiple services. This allows for only one endpoint
- **Single-service resource**: Provision a [[Azure AI Services||service]] individually, with a unique endpoint per service. This allows for services to be located in different regions, for example. Single-service resources usually have a free tier.
- **Training and prediction resource**: Most [[Azure AI Services||services]] can be used through a single resource, but some offer or require a separate training and prediction resource. This allows for the endpoints to be distinct, which is desirable if deployment will be in a different region to training, or you want to separate billing streams.

Once provisioned, an application will require the following information:
- **Endpoint URI**: HTTP address where the REST interface for the service can be accessed.
- **Subscription Key**: A key that allows access to the endpoint. When provisioned, two keys are created.
- **Resource Location**: When provisioned, all resources are assigned to a location. Most SDKs use the **Endpoint URI**, but some require the **Resource Location** as well.

# Interfacing
The REST API allows simple JSON over HTTP communication.

There exists SDKs that abstract the API communication. These exist for:
- C#
- Python
- JS
- Go
- Java
The endpoints and keys can be accessed at the resources **Keys and Endpoint** page (under **Resource Management**)
# Security
Subscription keys are how users access these services. Regenerating these keys regularly is very important for security.

They can be regenerated at the azure CLI with: `az cognitiveservices account keys regenerate`

They can be regenerated through the GUI by:
- Go to the resource's **Keys and Endpoint** page
- At the top of the page, there are two options for regenerating Key 1 or Key 2.

Each service is provided with 2 keys, the purpose of this is to allow for key regeneration without interrupting service.

## Azure Key Vault
A service designed for storing secrets. Access is granted to **security principles**. Administrators can assign a **security principle** to an application (in which case it is known as a **service principle**) which creates a **managed identity** for that application. This is all to prevent hard-coded keys.

## Token-based Authentication
Some [[Azure AI Services]] support/require token-based authentication.

This works by first supplying the subscription key, then an authentication token is returned that is valid for 10 minutes. Subsequent requests must provide the token. an SDK will handle this for you.
## [[Microsoft Entra ID Authentication]]


## Network Security
You can restrict IP ranges to only allow connections from specific IPs. You do this in **Firewalls and Virtual networks**

# Monitoring

## Monitor Azure AI services costs

Can estimate cost using [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/)

View overall cost of subscription by viewing the **Cost analysis** tab. To view only AI Services, add a filter that restricts resources with a **service name** of **Cognitive Services**.

## Create alerts
You can monitor resources by creating **Alert Rules**. They are based on resource events or metric thresholds.

To create an alert rule for an Azure AI services resource, select the resource in the Azure portal and on the **Alerts** tab, add a new alert rule.

You must define:
- **Scope**: The resource that triggers the alert
- **Condition**: There are 2 **signal types**:
	- **Activity Log**: An event occurring on the resource
	- **Metric**: Some metric threshold being met
- **Optional Actions**: Automatic functions to run on trigger, such as sending an email.
- **Alert Rule Details**: Tags for the alert, such as name, resource group etc.

## View metrics
You can view a resources metrics in the Azure Portal by selecting it and viewing its **Metrics** page. You can add the metric of your choice.
![[Pasted image 20250108151736.png]]

You can **share** this by exporting it to excel, or **clone** it to create a duplicate chart in the **Metrics** page.

### Dashboard
You can make a dashboard that combines multiple **metric** charts.

To create a dashboard, select **Dashboard** in the Azure portal menu (your default view may already be set to a dashboard rather than the portal home page). From here, you can add up to 100 named dashboards to encapsulate views for specific aspects of your Azure services that you want to track.
![[Pasted image 20250108151941.png]]

## Diagnostic Logging
Capture operation data.

To do so, you need a destination. Use **Azure Event Hubs** as a destination to export to a custom destination, but generally just use:
- **Azure Log Analytics**: Allows for querying and visualizing in the Azure Portal
- **Azure Storage**: Archive, can be exported if needed
These resources should be made *before* configuring logging. Make sure to create the **Azure Storage** account in the same region as the resource you wish to log.

Define diagnostic settings on the **Diagnostic Settings** page of the resource. When adding a setting, you must specify:
- A name for your diagnostic settings.
- The categories of log event data that you want to capture.
- Details of the destinations in which you want to store the log data.

![[Pasted image 20250108152407.png]]
it takes roughly an hour before data starts to flow to the destination(s). When it has arrived, if you used **Azure Log Analytics** for example, you can query it as follows:
![[Pasted image 20250108152527.png]]


# Deploying
Azure has  their own container hosts:
- **Azure Container Instance** (ACI)
- **Azure Kubernetes Service** (AKS) cluster

You can locally deploy an Azure Ai Services resource. This must have access to Azure to send metrics for billing. It's encrypted and obfuscated to ensure you pay. Also stops working if it cant phone home.

Pretty basic stuff though:
- Deploy locally. It exposes an API on the container
- Local clients access via the api
- It sends metrics back to Azure for billing

**Importantly**, you still need to provision an Azure Ai Services resource in Azure for locally hosted containers for billing.

Due to size, a lot of resources are split in to multiple containers. For example, the Language service is split into:

|Feature|Image|
|---|---|
|Key Phrase Extraction|mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase|
|Language Detection|mcr.microsoft.com/azure-cognitive-services/textanalytics/language|
|Sentiment Analysis|mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment|
|Named Entity Recognition|mcr.microsoft.com/product/azure-cognitive-services/textanalytics/language/about|
|Text Analytics for health|mcr.microsoft.com/product/azure-cognitive-services/textanalytics/healthcare/about|
|Translator|mcr.microsoft.com/product/azure-cognitive-services/translator/text-translation/about|
|Summarization|mcr.microsoft.com/azure-cognitive-services/textanalytics/summarization|

[All available containers are here](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-container-support#containers-in-azure-ai-**services**)
Use docker `pull` to download container images.

When deploying a container, you must specify:

|Setting|Description|
|---|---|
|ApiKey|Key from your deployed Azure AI service; used for billing.|
|Billing|Endpoint URI from your deployed Azure AI service; used for billing.|
|Eula|Value of **accept** to state you accept the license for the container.|
## Once deployed
Consume the resource from the locally hosted endpoint. Basically don't be stupid. Clients don't need subscription keys for hosted endpoints, you handle your own security.

# Content Safety
Need it for:
- **Increase in harmful content**: There's been a huge growth in user-generated online content, including harmful and inappropriate content.
- **Regulatory pressures**: Government pressure to regulate online content.
- **Transparency**: Users need transparency in content moderation standards and enforcement.
- **Complex content**: Advances in technology are making it easier for users to post multimodal content and videos.

[Azure AI Content Safety Studio](https://contentsafety.cognitive.azure.com/) seems to be commercial level content safety. Very sleek looking, too good really.

Content safety Vision is run by the **Florence** model.

In general, the content safety works as following for different media inputs. They are analyzed over 4 categories:
- Violence
- Hate Speech
- Sexual Content
- Self-Harm
## Text
- **Moderate Text**: Returns a score from 0 to 6, determining its severity
- **Prompt Shields**: an API to prevent jailbreaks from LLMs. Protects against prompts embedded in text or documents
- **Protected Material Detection**: Prevents recipes/lyrics from being posted. Scummy as fuck
- **Groundedness detection**: Verifies an LLMs output against reality. A grounded response is one based on source material, an ungrounded one varies. There's an additional *reasoning* field that explains why it's ungrounded, but that costs extra
## Image
- **Moderate Image**: Same categories, severity is *safe*, *low*, or *high*. You also set a threshold of *low*, *medium* or *high*. The combinations of these determines if an image is allowed or blocked. Sounds stupid
- **Moderate Multi-model Content**: Scans text and images, including text extracted from an image using Optical Character Recognition (**OCR**). Analyzed over same categories
## Custom Categories
Provide your own positive and negative examples and train the model.
**Safety System Messages** allows you to provide prompts to guide AI behavior. Sounds stupid as well.

## Limitations
It's AI. Might get stuff wrong. Test it

## Evaluating Accuracy
Get the **Confusion Matrix** of the TP, FP, TN and FN to evaluate.

