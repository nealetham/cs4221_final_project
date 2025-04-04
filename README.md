# cs4221_final_project

### Setting up TimescaleDB
```
docker pull timescale/timescaledb-ha:pg17
docker run -d --name timescaledb -p 5432:5432 -e POSTGRES_PASSWORD=password timescale/timescaledb-ha:pg17
```

### Setting up RagFlow
```
git clone https://github.com/infiniflow/ragflow.git
cd ragflow/docker
git checkout -f v0.17.2

# Open the .env file and on line 5 change:
# DOC_ENGINE=${DOC_ENGINE:-elasticsearch} ~> DOC_ENGINE=${DOC_ENGINE:-infinity}

# Use CPU for embedding and DeepDoc tasks:
$ docker compose -f docker-compose.yml up -d

# To use GPU to accelerate embedding and DeepDoc tasks:
# docker compose -f docker-compose-gpu.yml up -d

http://host.docker.internal:11434/
```

### [Setting up Ollama](https://hub.docker.com/r/ollama/ollama)
```
# CPU Only
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama

# Nvidia GPU

## Install Nvidia Toolkit
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
    | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
    | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
    | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update

## Restart Docker
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

## Start Container
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama

sudo docker exec ollama ollama pull llama3.2
```

<!-- ### [Configuring RagFlow and Ollama](https://ragflow.io/docs/dev/deploy_local_llm)
```
# Install chat and embedding model
sudo docker exec ollama ollama pull llama3.2
sudo docker exec ollama ollama pull bge-m3

# Ensure Ollama is accessible
sudo docker exec -it ragflow-server bash
root@8136b8c3e914:/ragflow# curl  http://host.docker.internal:11434/
``` -->


### [Configuring Qdrant](https://qdrant.tech/documentation/quickstart/)
```
docker pull qdrant/qdrant
docker run -p 6333:6333 -p 6334:6334 -v "$(pwd)/qdrant_storage:/qdrant/storage:z" qdrant/qdrant
```
