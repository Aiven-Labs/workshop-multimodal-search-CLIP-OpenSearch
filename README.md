# Workshop: Searching for images with vector search - OpenSearch and CLIP model

## Part 1. Prepare working environment

### Step 1. Set up OpenSearch service with Aiven

Register at [https://go.aiven.io/signup-opensearchclip](https://go.aiven.io/signup-opensearchclip) to host your OpenSearch service and get $300 credits for 30 days for other services.

### Step 2. Set up Codespaces
Press the button to open this repository in GitHub Codespaces:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/Aiven-Labs/workshop-multimodal-search-CLIP-OpenSearch/)

Or, if you prefer to do it manually: at the top of this GitHub page, above the file browser, select the <> Code button, choose the Codespaces tab and choose Create codespace on main. This will start up a new Codespaces environment.

### Step 3. Set OpenSearch credentials
To connect to OpenSearch cluster we'll use the URI of your cluster. 

1. Grab srevice URI from the service page of your Aiven for OpenSearch.
2. Clone `.env.examples` and rename it to `.env`.
3. Set `SERVICE_URI` in `.env` to your cluster's URI.

### Step 4. Install Python libraries
We'll need python libraries to operate OpenSearch, run the model and work with credentials.
Install them by running

```
pip install opensearch-py
pip install python-dotenv
pip install git+https://github.com/openai/CLIP.git
pip install ftfy regex tqdm
pip install Pillow   
pip install torch
```


## Part 2. Create OpenSearch index that supports KNN

To make sure that OpenSearch recognises vector data and supports KNN search, when creating an index we need to:
- set `knn` setting to true
- specify name of the property to contain vector data, set its type to `knn_vector` and specify its dimension size.

Go to `1-prepare-opensearch.ipynb` and run the notebook. Install/enable suggested extentions for python and Jupitor notebooks. Select recommended python environment.

You should see the response 

```
{'acknowledged': True, 'shards_acknowledged': True, 'index': 'photos'}
```

Also a newly added index will appear in the list of indexes in the service page of your Aiven for OpenSearch service.

## Part 3. Process each image and upload the vectors to OpenSearch

In this step we'll load the CLIP model, compute feature vectors for a batch of images and send the data into OpenSearch.

Go to `2-process-and-upload.ipynb` and run the notebook steps one by one. The last step will take several minutes to iterate over the photos.

## Part 4. Search for images

Time to search for an image by providing a text description. For this we'll do the following:

1. Transpate the text into a vector using CLIP model.
2. Compare this single vector to the vectors for images that we stored in OpenSearch
3. Retrieve 3 nearest images to the vector that is searched for.

Go to `3-run-vector-search.ipynb` and run the notebook steps one by one. 
Change value of ``text_input`` to search for different images.

 
