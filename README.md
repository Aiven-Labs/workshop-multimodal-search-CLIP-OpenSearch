# Workshop: Searching for images with vector search - OpenSearch and CLIP model

## Part 1. Prepare working environment

### Step 1. Set up OpenSearch service with Aiven

Register at [https://go.aiven.io/signup-opensearchclip](https://go.aiven.io/signup-opensearchclip) to host your OpenSearch service and get $300 credits for 30 days for other services.

### Step 2. Set up Codespaces
Press the button to open this repository in GitHub Codespaces:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/codespaces/new/Aiven-Labs/workshop-multimodal-search-CLIP-OpenSearch)

Or, if you prefer to do it manually: at the top of this GitHub page, above the file browser, select the <> Code button, choose the Codespaces tab and choose Create codespace on main. This will start up a new Codespaces environment.

### Step 3. Install Python libraries
We'll need python libraries to operate OpenSearch, run the model and work with credentials.
Install them by running

```
pip install -r requirements.txt
```

### Step 4. Set OpenSearch credentials
To connect to OpenSearch cluster we'll use the URI of your cluster. 

1. Grab the service URI from the service page of your Aiven for OpenSearch.
2. Copu `.env.examples` and rename it to `.env`.
3. Set `SERVICE_URI` in `.env` to your cluster's URI.



## Part 2. Create OpenSearch index that supports KNN

To make sure that OpenSearch recognises vector data and supports KNN search, when creating an index we need to:
- set `knn` setting to true
- specify name of the property to contain vector data, set its type to `knn_vector` and specify its dimension size.

Go to [1-prepare-opensearch.ipynb](1-prepare-opensearch.ipynb) and run the notebook. Install/enable suggested extentions for python and Jupitor notebooks. Select recommended python environment.

You should see the response 

```
{'acknowledged': True, 'shards_acknowledged': True, 'index': 'photos'}
```

Also a newly added index will appear in the list of indexes in the service page of your Aiven for OpenSearch service.

## Part 3. Process each image and upload the vectors to OpenSearch

In this step we'll load the CLIP model, compute feature vectors for a batch of images and send the data into OpenSearch.

Go to [2-process-and-upload.ipynb](2-process-and-upload.ipynb) and run the notebook steps one by one. The last step will take several minutes to iterate over the photos.

We can use the OpenSearch dashboard to see the contents of the index:
1. ..
2. ..
3. ..

## Part 4. Search for images

Time to search for an image by providing a text description. For this we'll do the following:

1. Translate the text into a vector using CLIP model.
2. Compare this single vector to the vectors for images that we stored in OpenSearch
3. Retrieve 3 nearest images to the vector that is searched for.

Go to [3-run-vector-search.ipynb](3-run-vector-search.ipynb) and run the notebook steps one by one. 
Change the value of ``text_input`` to search for different images.

## Interesting links

* [CLIP: Connecting text and images](https://openai.com/index/clip/), the OpenAI blog post from 2021 that describes CLIP
* [CLIP](https://github.com/openai/CLIP), the OpenAI GitHub repository for CLIP. This has code examples, and is the the basis for this workshop. At the end, it says:
  > **See also**
  > * [OpenCLIP](https://github.com/mlfoundations/open_clip): includes larger and independently trained CLIP models up to ViT-G/14
  > * [Hugging Face implementation of CLIP](https://huggingface.co/docs/transformers/model_doc/clip): for easier integration with the HF ecosystem

Other Aiven links:
* [When text meets image: a guide to OpenSearch® for multimodal search](https://aiven.io/developer/opensearch-multimodal-search?utm_source=github&utm_medium=referral&utm_content=workshop-opensearchclip&utm_campaign=workshop) is the ancestor of this workshop, but uses ~25,000 images Unsplash images, at their original resolution.
* [An app for searching for images matching a text, using CLIP, PostgreSQL® and pgvector](https://github.com/Aiven-Labs/app-multimodal-search-CLIP-PostgreSQL) presents a Python web app using PostgreSQL® and pgvector to do essentially the same thing as this workshop.
