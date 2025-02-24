# Deploying Fine-Tuned LoRA Model on Ollama ⚙️

To deploy a LoRA fine-tuned model on Ollama, follow these steps:

## 1. Install llama.cpp: 

Ensure you have built llama.cpp locally for converting the model.

## 2. Navigate to llama.cpp Directory:
```bash
cd llama.cpp
```
## 3. Convert Fine-Tuned Model to GGUF Format: 

Use the following command to convert your base model to the GGUF format
```bash
python ./convert_hf_to_gguf.py --outfile /path/to/output/BaseModel.gguf /path/to/HuggingFace/base/model
```

## 4. Convert Fine-Tuned LoRA Adapter to FP16 GGUF Format
```bash
python ./convert_lora_to_gguf.py --base  /path/to/HuggingFace/base/model --outfile /path/to/output/lora_adaptor.gguf --outtype f16 /path/to/FineTuned/LoRA/Adapter
```

## 5. Merging GGUF-Formatted FP16 Base Model with Fine-Tuned LoRA Adapter

```bash
./build/bin/llama-export-lora -m /path/to/output/BaseModel.gguf --lora /path/to/output/lora_adaptor.gguf -o /path/to/output/FinalModel.gguf
```

## 6. Requantize the Merged Model to Q4_0 Precision
```bash
./build/bin/llama-quantize /path/to/output/FinalModel.gguf /path/to/output/FinalModel_Q4_0.gguf 2
```

Our finetuned model is now ready in the appropriate format. Below are the instructions for deploying our model on Ollama.
## 1. Install ollama:

Ensure you have installed ollama.

## 2. Create Modelfile
Ollama needs a Modelfile to specify the model’s prompt format. This TEMPLATE is used by Ollama to render the input to the model. 
The template should follow the same prompt format as the fine-tuning dataset.
First create a Modelfile and add the following contents. You can modify these settings based on your model.

```bash
FROM ./FinalModel_Q4_0.gguf

PARAMETER stop <|im_start|>
PARAMETER stop <|im_end|>
PARAMETER temperature 0.1
PARAMETER top_p 0.95

TEMPLATE """{{ if .System }}<|im_start|>system
{{ .System }}<|im_end|>
{{ end }}{{ if .Prompt }}<|im_start|>user
{{ .Prompt }}<|im_end|>
{{ end }}<|im_start|>assistant
{{ .Response }}<|im_end|>
"""
```
Then:
```bash
ollama create YourModelName -f your/Modelfile/path
```
Finally, you can use the following commands to ensure your model has been added to Ollama and run it.
```bash
ollama list (You should see your model in this list.)
ollama run YourModelName
```

Your model is ready! 🎉





