---

Add " ai_therapist_model " file to your google drive

Use base_code.ipynb file to run the the model


````markdown
# Aura: AI Therapy Chatbot (Colab)

A mental health support chatbot powered by a fine-tuned LLaMA 3.2B model, served via Flask and exposed using Ngrok in a Colab environment.

---

## Features

- Text and audio responses via `/chat` endpoint
- Powered by Unsloth + PEFT
- TTS support with gTTS
- Public access using Ngrok

---

## Setup (in Colab)

1. **Install dependencies & configure Ngrok**  
   Replace `YOUR_TOKEN` with your Ngrok authtoken.

2. **Mount Google Drive**  
   Access your model from Drive.

3. **Set model path**  
   ```python
   model_path = "/content/drive/MyDrive/your_model_path"
````

4. **Define Flask app**
   Chat endpoint: `/chat`
   Accepts:

   * `message`: user input
   * `response_type`: `"text"` (default) or `"audio"`

5. **Start Flask + Ngrok**
   Tunnel opens on port 5000. Logs show the public URL.

6. **Keep server alive**
   The final cell runs a loop to keep Colab active.

---

## Example Usage

**Text Response**

```bash
curl -X POST https://<ngrok-url>/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "I feel lost"}'
```

**Audio Response**

```bash
curl -X POST https://<ngrok-url>/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "I'm stressed", "response_type": "audio"}' --output reply.mp3
```

---

## Notes

* This is **not** a diagnostic tool.
* Keep Colab running for the server to stay active.
* Do not share your Ngrok token.

-----

## 🔧 Fine-Tuning Instructions

To fine-tune your own version of the model:

1. Open `base_model_fine_tuning.ipynb` in Google Colab.
2. Install required dependencies (Unsloth, PEFT, TRL, etc.).
3. Load the base model: `unsloth/Llama-3.2-3B-Instruct-unsloth-bnb-4bit`.
4. Apply LoRA adapters using `FastLanguageModel.get_peft_model()`.
5. Load the dataset (e.g., `vibhorag101/phr_mental_therapy_dataset` from Hugging Face).
6. Format the dataset to have a `text` field.
7. Train the model using `SFTTrainer`.
8. Save the trained adapter weights to your Google Drive:
   `/content/drive/MyDrive/ai_therapist_model`
9. Use this saved model in `base_code.ipynb` for running the chatbot.
