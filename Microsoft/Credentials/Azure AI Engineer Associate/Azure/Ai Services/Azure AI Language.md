Provision this with **Azure AI Language** service. Like all other services, this can be provisioned as an **Azure AI Service** multi-service resource, however this isn't specified in this module because it is abundantly clear this course is written by many different people who haven't read the previous modules.

# Text Analysis

Its functionality includes:
- _Language detection_ - determining the language in which text is written.
- _Key phrase extraction_ - identifying important words and phrases in the text that indicate the main points.
- _Sentiment analysis_ - quantifying how positive or negative the text is.
- _Named entity recognition_ - detecting references to entities, including people, locations, time periods, organizations, and more.
- _Entity linking_ - identifying specific entities by providing reference links to Wikipedia articles.

## Language Detection
Works with **documents** under 5,120 characters. "**Documents**" are just strings. You can have up to 1000 documents per request, each with their own unique **ID**. It returns a language and confidence score between 0 and 1 for each document.

Multi-lingual input will return which-ever language is most prominent with a lower confidence score.

If it doesn't recognize the characters or language, it will return 0 with a type of "(Unknown)"

Example JSON:
```json
{
    "kind": "LanguageDetection",
    "parameters": {
        "modelVersion": "latest"
    },
    "analysisInput":{
        "documents":[
              {
                "id": "1",
                "text": "Hello world",
                "countryHint": "US"
              },
              {
                "id": "2",
                "text": "Bonjour tout le monde"
              }
        ]
    }
}
```

## Key Phrase Extraction
Works best for larger documents. (max size=5,120 characters). Same format as before, just change `kind` to `KeyPhraseExtraction` and add a language tag for each document: `"language": "en"`

## Sentiment Analysis
Same as before, just change `"kind": "SentimentAnalysis"`

This time it will provide a sentiment analysis for the entire document, but also one for each sentence with a document. It will specify if it is "positive", "neutral" or "negative", with an associated confidence score between 0 and 1 for each category.

## Named Entity Recognition

[Full list of entities found here](https://learn.microsoft.com/en-us/azure/ai-services/language-service/named-entity-recognition/concepts/named-entity-categories?tabs=ga-api)

Very useful for extracting key information. Will classify, for example "Joe went to London on Saturday" into:
```json
[
	  {
		"text":"Joe",
		"category":"Person",
		"offset":0,
		"length":3,
		"confidenceScore":0.62
	  },
	  {
		"text":"London",
		"category":"Location",
		"subcategory":"GPE",
		"offset":12,
		"length":6,
		"confidenceScore":0.88
	  },
	  {
		"text":"Saturday",
		"category":"DateTime",
		"subcategory":"Date",
		"offset":22,
		"length":8,
		"confidenceScore":0.8
	  }
]
```
## Entity Linking
Useful for differentiating ambiguous entities. For example, Venus the planet vs Venus the Goddess.

Depending on context, Entity Linking will provide Wikipedia articles referencing exactly what the document is referencing. Only works for entities. It will pick the entities out on it's own

"I saw Venus shining in the sky" would return "https://en.wikipedia.org/wiki/Venus"

# Question Answering
Can create a *knowledge base* or *question* and *answer* pairs that can be published to a REST endpoint.

Can be developed from existing sources such as:
- Web sites containing frequently asked question (FAQ) documentation.
- Files containing structured text, such as brochures or user guides.
- Built-in _chit chat_ question and answer pairs that encapsulate common conversational exchanges.

The difference between this and a standard LLM is below:

| |Question answering|Language understanding|
|---|---|---|
|Usage pattern|User submits a question, expecting an answer|User submits an utterance, expecting an appropriate response or action|
|Query processing|Service uses natural language understanding to match the question to an answer in the knowledge base|Service uses natural language understanding to interpret the utterance, match it to an intent, and identify entities|
|Response|Response is a static answer to a known question|Response indicates the most likely intent and referenced entities|
|Client logic|Client application typically presents the answer to the user|Client application is responsible for performing appropriate action based on the detected intent|

You can use the REST api to do this, but normally people use the [Language Studio](https://language.azure.com/) to define a *knowledge base*.

To create a knowledge base you:
1. Sign in to Azure portal.
2. Search for **Azure AI services** using the search field at the top of the portal.
3. Select **Create** under the **Language Service** resource.
4. Create a resource in your Azure subscription:
    - Enable the _question answering_ feature.
    - Create or select an **Azure AI Search** resource to host the knowledge base index.
5. In Language Studio, select your Azure AI Language resource and create a **Custom question answering** project.
6. Add one or more data sources to populate the knowledge base:
    - URLs for web pages containing FAQs.
    - Files containing structured text from which questions and answers can be derived.
    - Predefined _chit-chat_ datasets that include common conversational questions and responses in a specified style.
7. Edit question and answer pairs in the portal.

Also supports *multi-turn* conversation, where a response is only decided after multiple questions have been asked/answered.

## Publishing

After defining a knowledge base, you test and publish a trained model. You can test the trained model interactively in the Language Studio. 

Once published, you can use it by submitting a request to the endpoint that looks like this:
```json
{
  "question": "What do I need to do to cancel a reservation?",
  "top": 2,
  "scoreThreshold": 20,
  "strictFilters": [
    {
      "name": "category",
      "value": "api"
    }
  ]
}
```

| Property       | Description                                                |
| -------------- | ---------------------------------------------------------- |
| question       | Question to send to the knowledge base.                    |
| top            | Maximum number of answers to be returned.                  |
| scoreThreshold | Score threshold for answers returned.                      |
| strictFilters  | Limit to only answers that contain the specified metadata. |

## Improving
You can improve performance with *active learning* (online learning) and *synonyms*.

To do *active learning*:
1. Create pairs of *questions* and *answers* in Language Studio, or import a file
2. Review the models newly learnt suggestions
3. Accept or reject these suggestions. You can also manually add alternate questions with the "Add alternate question" button
To use *synonyms*, simply define similar words as synonyms in a request:
```json
{
    "synonyms": [
        {
            "alterations": [
                "reservation",
                "booking"
                ]
        }
    ]
}
```
(yes this makes no sense. This implies anyone can submit new synonyms. If more info is required, [see here](https://learn.microsoft.com/en-us/azure/ai-services/language-service/question-answering/tutorials/adding-synonyms))

# NLU (Natural Language Understanding)

## Preconfigured features
Not gonna lie, this is confusing. Does this fall under the same category as was covered in text analysis? Why is PII not included there?
- **Summarization**: Available for documents and conversations.
- **Named entity recognition**: Same as before in Text Analysis
- **Personally identifiable information (PII) detection**: Detects sensitive information such as passwords, emails etc.
- **Key Phrase Extraction**: Same as in Text Analysis
- **Sentiment analysis**
- **Language detection**

## Learned features:

- **Conversational Language Understanding (CLU)**: Allows you to build a custom NLU model to predict intent and extract information from utterances
- **Custom Named Entity Recognition**: Can add custom named entities to NER
- **Custom Text Classification**: Make customizable groups to classify text into
- **Question Answering**: Covered previously

## LUIS (Language Understanding Service)
_About to be deprecated Oct 1st 2025_

Provision an Azure AI Language resource first. This will be used for authoring model and processing requests.

