# Deploying a Fine-Tuned LoRA Model on Ollama 🚀
This project provides a step-by-step guide for converting and deploying a fine-tuned LoRA model on Ollama. It covers the entire process of converting the base model (FP16) to GGUF format, merging the LoRA adapter with the base model, and deploying the final model on Ollama.
The process includes:

✅ Converting the base model to GGUF using llama.cpp

✅ Converting the fine-tuned LoRA adapter to GGUF (FP16 format)

✅ Merging the base model and LoRA adapter to create the final model

✅ Quantizing the merged model to optimize performance (Q4_0 format)

✅ Creating a Modelfile and deploying the model on Ollama

Once completed, you can run your fine-tuned model on Ollama for efficient inference. 🚀

## Requirements
### Build llama.cpp locally
```bash
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
cmake -B build
cmake --build build --config Release
```
For the complete guide, you can refer to the official instructions:
[Build llama.cpp locally](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md)

### Ollama installation 
To install Ollama (linux), run the following command:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```
For manual installation of Ollama, refer to this link: 
[Manual install Ollama](https://github.com/ollama/ollama/blob/main/docs/linux.md)




