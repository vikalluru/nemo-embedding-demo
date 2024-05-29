source env_variables.txt

ngc registry model download-version "ohlfw0olaadg/ea-participants/nv-embed-qa-mistral-7b:1"
chmod -R o+rX nv-embed-qa-mistral-7b_v1

docker pull nvcr.io/ohlfw0olaadg/ea-participants/nemo-retriever-embedding-microservice:24.04

docker run \
  -it \
  --name $CONTAINER_NAME \
  --gpus=1 \
  --shm-size=8G \
  -v $(pwd):/model-checkpoint-path \
  -p $SERVICE_PORT:$SERVICE_PORT \
  nvcr.io/ohlfw0olaadg/ea-participants/nemo-retriever-embedding-microservice:24.04 \
  bin/web -p $SERVICE_PORT -c /model-checkpoint-path/$MODEL_NAME -g model_config_templates/${MODEL_ID}_template.yaml