# Run Gemma with Ollama on Cloud Run

This sample shows how to deploy the Ollama API with gemma3:4b on Cloud Run, to run inference using CPU only.

**Gemma** is Google's open model built from the same research and technology used to create the Gemini models.

**Ollama** is a framework that makes it easy for developers to prototype apps with open models, including gemma. It comes with a REST API and this sample deploys that API with a Cloud Run service.

## Usage

To build the container with `gemma3:4b` included and deploy the Ollama API to a publicly accessible URL on Cloud Run, use the following command from the directory ./run/ollama-gemma:

```
bash deploy.sh
```

Respond to any prompts the command gives you. You might need to enable a few APIs
and choose a region to deploy to.

Building the container takes roughly 3 minutes.

Once the command completes, the deploy command shows the public URL of the service. For convenience, store the URL in an environment variable:

```
URL=$(gcloud run services list --format "value(URL)" --filter metadata.name=ollama-gemma)
```

## Explore the API

To display the list of available models, send a request to `api/tags`. This should list gemma:2b.

```
curl $URL/api/tags
```

Ask Gemma a question:

```
curl $URL/api/generate -d \
 '{
    "model": "gemma3:4b",
    "prompt": "Why is the sky blue?"
  }'
```

The first request to a new instance will take some extra setup time because Gemma is loaded into memory. Ollama keeps the model in memory for 5 minutes. I search for a config parameter to change this setting but didn't find it yet.

For the full Ollama API, refer to [the API docs](https://github.com/ollama/ollama/blob/main/docs/api.md).

## Clean up

To clean up after following this short tutorial, you can do the following:

- In Artifact Registry, find the `cloud-run-source-deploy` repository and remove
  the container image used by the Cloud Run service you created.
- In Cloud Run, delete the service you created.

## Links

- https://github.com/google/gemma.cpp
- https://github.com/ollama/ollama

---
