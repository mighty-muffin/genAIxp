# genAIxp

---

## [ollama](https://ollama.com/)

Install the engine on amd64 hardware, if you want to try on arm6e read the docs.
Check your [GPU](https://github.com/ollama/ollama/blob/main/docs/gpu.md) for compatibility first.

If you don't have a GPU or additional hardware (Apple Silicon has built-in AI accelerator) expect 4 to 16 token per seconds (t/s) for CPU bound engine.  50-80 t/s is a rather comfortable pace for single user concurrencies.

You can get a model from [ollama model](https://ollama.com/search) website and find few that met your needs.

I like to use `llama3.2:1B` and `llama3.2:3B` for low end GPU or CPU bound application.
If you have a better GPU with ~8GB of VRAM, `gemma3:4b`, `deepseek-r1:7b` or `mistral:7b` are pretty decent with larger dataset.
If you are looking for a coding assistant and a high end GPU (RTX 3080, 4080, 5080 or better... good for you !!!), `qwen2.5-coder:14b` has great feedback.

These steps will install the ollama agent engine for "terminal inference" meaning you will use your terminal.

```bash
curl -fsSL https://ollama.com/install.sh | sh # watch before you install
ollama --version
ollama pull llama3.2:1B

# Test if the API works => if you have an issues here... read the docs
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2:1B",
  "prompt": "Why is the sky blue?",
  "format": "json"
}'

# Run the full model - GL HF
ollama run llama3.2:1B
```

## [openwebui](https://openwebui.com/)

Install docker on your amd64 linux debian based computer.

```bash
sudo apt update
sudo apt install apt-transport-https curl
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test if docker works => if you have an issues here... read the docs
sudo systemctl is-active docker

# Enabling non-root users to run docker commands
sudo usermod -aG docker ${USER}
sudo reboot now

# Test if docker works => if you have an issues here... read the docs
docker run hello-world
```

Deploy openwebui for NVidia based GPU accelerator, there are other options depending on your hardware.

```bash
docker run -d -p 80:8080 --gpus all -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:cuda
```

Create a custom Mario model

```bash
cat << EOF > Modelfile-mario
FROM llama3.2:3b

# https://github.com/ollama/ollama/blob/main/docs/modelfile.md
# set the temperature to 1 [higher is more creative, lower is more coherent]
PARAMETER temperature 10
# Reduces the probability of generating nonsense. A higher value (e.g. 100) will give more diverse answers
PARAMETER top_k 99
# Works together with top-k. A higher value (e.g., 0.95) will lead to more diverse text
PARAMETER top_p 0.95

# set the system message
SYSTEM """
You are Mario from Super Mario Bros.
Answer as Mario, the assistant, only.
"""
EOF

# Might have to refresh your openwebui session to load the newer custom model
ollama create mario -f ./Modelfile-mario
```

## [stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

Stable Diffusion web UI for generative AI pictures and movies.  Check the GH for instruction, it took me a while to get everything working.  I did not show this tool given it was a very slow process (~10-15min per prompt for picture generation.)

The setup is complexe given it relies on `Python3.10` and has a lot of compatiblities issues.

## [exo](https://github.com/exo-explore/exo)

Run your own AI cluster at home with everyday devices.  Check the GH for instruction, it took me a while to get everything working.

The setup is complexe given it relies on `Python3.12` and has a lot of compatiblities issues.

---

Not responsable for your power bills.
