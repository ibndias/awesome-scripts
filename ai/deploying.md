# Deploying a Model with Text Generation Inference (TGI)

## Overview
This guide explains how to deploy a model using **Text Generation Inference (TGI)** with Docker. It also provides an explanation of each argument used in the deployment command.

## Deployment Command
```bash
docker run -d --rm --gpus '"device=1"' \
  -e HUGGING_FACE_HUB_TOKEN=xx \
  --shm-size 1g \
  -p 12315:80 \
  --name tgi-cipher-qwen25 \
  -v /data2/huggingface/hub:/data \
  ghcr.io/huggingface/text-generation-inference:3.1.0 \
  --model-id PNU-Infosec/qwen25-cipher-chicao-32k-v1 \
  --revision=22686 \
  --max-total-tokens=32000 \
  --max-input-length=30000 \
  --max-batch-prefill-tokens=32000 \
  --cuda-memory-fraction 0.5
```

## Explanation of Arguments

### Docker Arguments

- `docker run -d`
  - Runs the container in detached mode (in the background).

- `--rm`
  - Automatically removes the container when it stops.

- `--gpus '"device=1"'`
  - Assigns the container to use GPU device **1**. This ensures it runs on a specific GPU instead of using all available GPUs.

- `-e HUGGING_FACE_HUB_TOKEN=xx`
  - Passes the **Hugging Face Hub token** as an environment variable, allowing access to private models.

- `--shm-size 1g`
  - Increases shared memory size to **1GB**, which improves performance for large models.

- `-p 12315:80`
  - Maps **port 80** of the container to **port 12315** on the host, making the API accessible via `http://localhost:12315`.

- `--name tgi-cipher-qwen25`
  - Assigns a specific name to the container (`tgi-cipher-qwen25`) for easier management.

- `-v /data2/huggingface/hub:/data`
  - Mounts the local directory `/data2/huggingface/hub` to `/data` inside the container, enabling caching and persistence of model files.

### TGI Arguments

- `ghcr.io/huggingface/text-generation-inference:3.1.0`
  - Specifies the **TGI version (3.1.0)** to use from the Hugging Face container registry.

- `--model-id PNU-Infosec/qwen25-cipher-chicao-32k-v1`
  - Defines the **model to load** from Hugging Face (`PNU-Infosec/qwen25-cipher-chicao-32k-v1`).

- `--revision=22686`
  - Specifies a particular **model revision**, ensuring consistency across deployments.

- `--max-total-tokens=32000`
  - Sets the **maximum total tokens** for inference requests, balancing memory usage and model performance.

- `--max-input-length=30000`
  - Limits the **maximum input length** to 30,000 tokens, preventing overly large inputs that could crash the system.

- `--max-batch-prefill-tokens=32000`
  - Defines the **maximum number of prefill tokens** in batch mode, optimizing batch processing efficiency.

- `--cuda-memory-fraction 0.5`
  - Allocates **50% of available GPU memory** to this container, preventing excessive resource usage.

## Monitoring the Deployment
To check the logs of the running container:
```bash
docker logs -f tgi-cipher-qwen25
```
This command follows (`-f`) the logs in real-time, helping debug any issues.

## Conclusion
This guide provides a clear breakdown of deploying a model with TGI using Docker. Understanding each argument ensures efficient deployment, optimized performance, and effective resource management.

# Deploying with VLLM
```
docker run --runtime nvidia --gpus '"device=7"' \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
     --env "HF_HUB_ENABLE_HF_TRANSFER=0" --env "HUGGING_FACE_HUB_TOKEN=<secret>" \
    -p 1313:8000 \
    --ipc=host \
    vllm/vllm-openai:latest \
    --model PNU-Infosec/xxx --revision checkpoint-XX
```
