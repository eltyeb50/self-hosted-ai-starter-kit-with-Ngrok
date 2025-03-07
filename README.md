
# n8n Self hosted AI starter kit and Ngrok Tunnel

This repository contains a Docker Compose setup for running n8n "self-hosted-ai-starter-kit" with Ngrok as a tunneling service. n8n is a workflow automation tool that allows you to connect different services and APIs. Ngrok exposes local servers behind NATs and firewalls to the public internet over secure tunnels.
### What’s included

✅ [**Self-hosted n8n**](https://n8n.io/) - Low-code platform with over 400
integrations and advanced AI components

✅ [**Ollama**](https://ollama.com/) - Cross-platform LLM platform to install
and run the latest local LLMs (huihui_ai/deepseek-r1-abliterated:8b)

✅ [**Qdrant**](https://qdrant.tech/) - Open-source, high performance vector
store with an comprehensive API

✅ [**PostgreSQL**](https://www.postgresql.org/) -  Workhorse of the Data
Engineering world, handles large amounts of data safely.

✅ [**Ngrok-Tunnel**](https://ngrok.com/) - Tunneling Service
## Prerequisites

Before you begin, ensure you have the following installed:
- Docker: [Get Docker](https://docs.docker.com/get-docker/)
- Docker Compose: [Install Docker Compose](https://docs.docker.com/compose/install/)

 Get AuthToken and Domain from Ngrok (Free)
- Ngrok Domain: [Create New domain](https://dashboard.ngrok.com/domains)

## Setup

1. **Clone the Repository**

   Clone this repository to your local machine:
   ```bash
   git clone https://github.com/eltyeb50/self-hosted-ai-starter-kit-with-Ngrok.git
   ```

2. **Ngrok Authentication**

   You need to authenticate with Ngrok. If you don't have an Ngrok account, create one at [Ngrok](https://ngrok.com/). After creating an account, get your auth token from the Ngrok dashboard.

   Set your Ngrok auth token in the `.env` file:

   ```sh
   NGROK_AUTHTOKEN=SET_NGROK_AUTH_TOKEN_HERE   
   ```

3. **Permanent Domain**
   In your Ngrok Dashboard you can reserve a domain, You can do this under Cloud Edge > Domains. Once you have the domain add it to the `.env` file:

   ```sh
   URL=https://from-ngrok.ngrok-free.app
   ```

   This will also need to be added to the `ngrok.yml`
   ```yaml
   version: 2
   log_level: debug
   tunnels:
       n8n:
           proto: http
           addr: n8n:5678
           domain: from-ngrok.ngrok-free.app
   ```

3. **Configure n8n**

   Optionally, you can configure n8n by modifying environment variables in the `docker-compose.yml` file under the `n8n` service.

### Running n8n using Docker Compose

#### For Nvidia GPU users

```
git clone https://github.com/eltyeb50/self-hosted-ai-starter-kit-with-Ngrok.git
cd self-hosted-ai-starter-kit-with-Ngrok
docker compose --profile gpu-nvidia up -d
```

> [!NOTE]
> If you have not used your Nvidia GPU with Docker before, please follow the
> [Ollama Docker instructions](https://github.com/ollama/ollama/blob/main/docs/docker.md).

### For AMD GPU users on Linux

```
git clone https://github.com/eltyeb50/self-hosted-ai-starter-kit-with-Ngrok.git
cd self-hosted-ai-starter-kit-with-Ngrok
docker compose --profile gpu-amd up
```

#### For Mac / Apple Silicon users

If you’re using a Mac with an M1 or newer processor, you can't expose your GPU
to the Docker instance, unfortunately. There are two options in this case:

1. Run the starter kit fully on CPU, like in the section "For everyone else"
   below
2. Run Ollama on your Mac for faster inference, and connect to that from the
   n8n instance

If you want to run Ollama on your mac, check the
[Ollama homepage](https://ollama.com/)
for installation instructions, and run the starter kit as follows:

```
git clone https://github.com/eltyeb50/self-hosted-ai-starter-kit-with-Ngrok
cd self-hosted-ai-starter-kit-with-Ngrok
docker compose up
```

##### For Mac users running OLLAMA locally

If you're running OLLAMA locally on your Mac (not in Docker), you need to modify the OLLAMA_HOST environment variable
in the n8n service configuration. Update the x-n8n section in your Docker Compose file as follows:

```yaml
x-n8n: &service-n8n
  # ... other configurations ...
  environment:
    # ... other environment variables ...
    - OLLAMA_HOST=host.docker.internal:11434
```

Additionally, after you see "Editor is now accessible via: (Your Ngrok Domain) <https://from-ngrok.ngrok-free.app/>":

1. Head to (Your Ngrok Domain) <https://from-ngrok.ngrok-free.app/home/credentials>
2. Click on "Local Ollama service"
3. Change the base URL to "http://host.docker.internal:11434/"

#### For everyone else

```
git clone https://github.com/eltyeb50/self-hosted-ai-starter-kit-with-Ngrok
cd self-hosted-ai-starter-kit-with-Ngrok
docker compose --profile cpu up
```

## ⚡️ Quick start and usage

The core of the Self-hosted AI Starter Kit is a Docker Compose file, pre-configured with network and storage settings, minimizing the need for additional installations.
After completing the installation steps above, simply follow the steps below to get started.

1. Open (Your Ngrok Domain) -> e.g. <https://from-ngrok.ngrok-free.app> in your browser to set up n8n. You’ll only 
   have to do this once.
2. Open the included workflow:
   <https://from-ngrok.ngrok-free.app/workflow/srOnR8PAY3u4RSwb>
3. Click the **Chat** button at the bottom of the canvas, to start running the workflow.
4. If this is the first time you’re running the workflow, you may need to wait
   until Ollama finishes downloading huihui_ai/deepseek-r1-abliterated:8b. You can inspect the docker
   console logs to check on the progress.
 ```
docker logs ollama-pull-llama
```


To open n8n at any time, visit (Your Ngrok Domain) https://from-ngrok.ngrok-free.app in your browser.

With your n8n instance, you’ll have access to over 400 integrations and a
suite of basic and advanced AI nodes such as
[AI Agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/),
[Text classifier](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.text-classifier/),
and [Information Extractor](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.information-extractor/)
nodes. To keep everything local, just remember to use the Ollama node for your
language model and Qdrant as your vector store.

> [!NOTE]
> This starter kit is designed to help you get started with self-hosted AI
> workflows and Tunelled via Ngrok free domain that will get. While it’s not fully optimized for production environments, it
> combines robust components that work well together for proof-of-concept
> projects. You can customize it to meet your specific needs

## Upgrading

* ### For Nvidia GPU setups:

```bash
docker compose --profile gpu-nvidia pull
docker compose create && docker compose --profile gpu-nvidia up
```

* ### For Mac / Apple Silicon users

```
docker compose pull
docker compose create && docker compose up
```

* ### For Non-GPU setups:

```bash
docker compose --profile cpu pull
docker compose create && docker compose --profile cpu up
```



# n8n Self-hosted AI Starter Kit and Ngrok Tunnel

Maintained by [Altayeb Mohamed](https://github.com/eltyeb50)

This project is based on and incorporates elements from the following repositories:

*   ([n8n-io/self-hosted-ai-starter-kit](https://github.com/n8n-io/self-hosted-ai-starter-kit/tree/main))
*   [Joffcom/n8n-ngrok](Original Repository 2 URL) by [Original Author 2](https://github.com/Joffcom/n8n-ngrok)

This project combines and extends the functionality of these repositories to create a unified self-hosted AI starter kit with Tunnel functinality.
