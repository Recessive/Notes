## Premise
Idea is to have a dashboard that works as a gateway to pre-trained open-source models. These models should have an API exposed so that you can access them.

We would have a website that hosts numerous open source models, that come pre-packaged with their own API so you can rapidly deploy them on-premises (as docker containers). Clients could download one such model and host it locally. The **bProp Hub** would then connect to this internal API, and expose its own internet-facing API such that an end use could effectively access the model.

This centralization shouldn't be mandatory, however a very valuable feature of **bProp Hub** should be automatic request routing and model updating. Assuming a large implementation, one instance of a model might be busy with a different request, so the current request could be routed to a cloned instance (ie, multiple instances of one LLM) to ensure rapid response times. The app would use pre-packaged docker images with APIs ready to run out-of-the-box. It would essentially be a gateway to hugging face models.

Expanding on this, a user-driven model listing would exist that allows companies to download other pre-packaged open-source model images so they can deploy them locally. This would take the strain off us from having to manually create APIs for every model, meaning we would only have to provide immediate support for current and popular open-source models.

The benefit of using **bProp Hub** is the simplicity for developers. Companies don't need to hire expert ML engineers, they only need very basic training in web development to access the **bProp Hub** API and bind multiple AI Service together.


To clarify the flow of the application, I've included an example below:

## Example: Re-creating Chat GPT
Models:
- DeepSeek
- Stable Diffusion
- YOLO
- CLIP-S

Functionality:
- Handle complex natural language inputs and respond in a helpful manner
- Generate images upon request
- Detect objects within a provided image upon request
- Provide a caption to an image upon request

Workflow:
1. Developer runs a locally hosted instance of **bProp Hub** and signs in
2. Developer downloads docker images from **bprop.ai** for the 4 listed models via **bProp Hub**
3. Developer moves these docker images to the appropriate machines within their local network. Due to demand, developer clones the DeepSeek docker image and runs two containers of said image. 
4. Developer creates four *services* on **bProp Hub**:
	1. *Chat*
	2. *ImageGen*
	3. *ObjectDetection*
	4. *Caption*
5. Inside the *Chat* service, developer clicks the **+** icon to add a linked container. They input the address of the HTTP API of the desired container. They do this twice so as to link both instances of the *DeepSeek* containers.
6. Developer repeats this process for *ImageGen*, *ObjectDetection* and *Caption* services.
7. In the *API* page of **bProp Hub**, developer modifies default API values for each *service*. Expanding the dropdown for each *service*, they set default values for fields that are not required from the user (encoding style, priority etc). Fields left empty will be exposed in the **bProp Hub** API. The only field left empty in *Chat*, for example, is *Text*.
8. Developer creates an additional custom HTTP server that is exposed to the internet. This server has one endpoint that accepts a string. For an example request, the following happens:
    1. A request is sent to the internally-exposed **bProp Hub** *Chat Service* API with the string. The string is pre-pended with prompt instructions saying: 

     >Classify the intent of the following response, outputting a 1 for yes or 0 for no in an array format for whether the user is requesting any of the following services: [Image Generation Service, Object Detection Service, Caption Service]
	
	2. **bProp Hub** then sends a response containing an array that looks like: `[1,0,0]`
	3. An additional request is sent to the **bProp Hub** *Chat Service* API with the raw string and a default prompt pre-pended specifying kind and helpful responses.
	4. An additional request is sent to the **bProp Hub** *ImageGen Service* API with the string.
	5. Both of these requests are sent asynchronously. The *ImageGen Service* request takes the longest, so the server sends a response to the user with the *Chat Service* response only. The user sees in their browser the text response, and a loading image icon.
	6. After some time, the *ImageGen Service* sends a response containing the generated image. The server then sends this to the user and the user sees their requested image

With this stack, developers can work independently of the complexity setting up AI models. It allows them to operate with a simple API, whilst different team members manage running the models.

As the market currently stands, to make a similar app you would need fully trained AI Engineers to deploy open-source models locally and develop APIs for each one. They would have to handle their own update cycles, and their own request routing should load demand it. Scaling would be a nightmare. Our selling point is the ability for **bProp Hub** to handle this and remove the need for advanced AI Engineers for quick applications.

As open source models become smaller and more powerful, companies are going to be more inclined to self host these models.

## Market Research

It is worth noting that all gateways listed below (with the exception of GitLab AI Gateway) could all probably use locally hosted AI services, and route between them.

However, you would need to tailor the APIs of these locally hosted services to work with these gateways. Potentially these gateways may only support the big players anyway, such as Azure and AWS, so unless your custom made API matched the signatures of these providers, these gateways would not work with your locally hosted instances.

Importantly, these gateways *cannot handle deployment or updating*. They expect all the work on deploying the model to be done by the online providers, or by you if you locally host. **bProp Hub** *has* to have this functionality to stand apart from these gateways.

### [Kong AI Gateway](https://konghq.com/products/kong-ai-gateway)
This works as a gateway between a request and AI APIs. It does a very similar thing to what's proposed here, and pulls metrics from associated websites so you can view summarized metrics across multiple services.

This essentially shows that facilitating the use of multiple pre-existing AI services has existing products. We would need to build a product focused on open-source and self-hosting. This means an emphasis on the pre-packaged docker images that allow for rapid deployment, request routing and automatic model updating. **Very open to suggestions on anything else clients might want.**

### [PortKey AI Gateway](https://portkey.ai/features/ai-gateway)
Can be locally hosted.

Used by booking.com, specifically designed to be used with online services. It has:
- **Load Balancing**: Distribute workloads between providers
- **Fallbacks**: Automatically switch providers in case one goes down
- **Caching**: Cache previous responses

They also have **PortKey Guardrails**, that adds guardrails to LLMs (security on prompts). Integrating this with their gateway means you can run checks before you send a request to any service. This centralizes and separates the process. It's pretty normal right now for developers to build their own custom guardrails in their application, but by placing it in their Gateway they standardized this process and centralize it. **<- This is a powerful feature**


Essentially just routes a request to your linked AI Services. What we want to do, but only online. There's a big market for this.

### [GitLab AI Gateway](https://docs.gitlab.com/ee/user/gitlab_duo/gateway.html)
This is not a public facing solution. It is described by GitLab as their own implementation of an AI Gateway to route your response and ensure fast response times. They basically just tell you how it works, but you cannot access the internal workings, deploy it or modify it in anyway. It's just transparency.

Still, useful information on features something like this should include.

### [Traefik AI Gateway](https://traefik.io/solutions/ai-gateway/)
Similar to the previous gateways, this connects to online providers, not designed for local. it spouts the usual benefits:
- Not relying on one vendor
- Manage all keys in one place
- Better credential security
- Unified governance (basically the same thing PortKey Guardrails was banging on about)
- Consolidated metrics

Notably doesn't mention load balancing, which I imagine is a main benefit of this tbh

### [F5 AI Gateway](https://www.f5.com/products/ai-gateway)

This is similar to all previous examples, however it claims it works with on-prem models as well. I can't find any additional information on this, so I'm going to assume it's a pretty rudimentary implementation of this. Local model hosting is definitely not the focus, this app is focused on the security an AI Gateway provides.
### [Hugging Face Inference API](https://huggingface.co/docs/api-inference/en/index)
Would be useful for what we're doing. Basically just an API on top of a model that allows you to run it. This is helpful for developers, but still requires some technical expertise to run.

This does not allow for non-technical staff to deploy models, and does nothing for routing or updating.


## Selling it
[This goes over benefits of an API Gateway, which is what this is](https://www.youtube.com/watch?v=hWRRdICvMNs)

