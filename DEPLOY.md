# OpenSource Clipping — Deployment Runbook

## Live Frontend
**Web Studio:** [https://parth-ux-arch.github.io/opensource-clipping/studio/](https://parth-ux-arch.github.io/opensource-clipping/studio/)

## Backend Architecture
- **Notebook:** 
otebooks/kaggle-studio-server.ipynb
- **Colab GPU Runtime:** T4 GPU
- **HTTPS Tunnel:** ngrok
- **CORS Allowed Origins:** https://parth-ux-arch.github.io

---

## Quick Restart Guide (After Colab Disconnect)

Google Colab runtimes disconnect after ~12 hours or ~90 minutes of inactivity. To restart the backend:

1. **Open the Notebook in Colab:**
   👉 [Open in Colab](https://colab.research.google.com/github/Parth-ux-arch/opensource-clipping/blob/main/notebooks/kaggle-studio-server.ipynb)

2. **Select GPU Hardware:**
   - Go to **Runtime** > **Change runtime type**
   - Select **T4 GPU**
   - Click **Save**

3. **Add Secrets (Colab Secrets Tab - 🔑 Icon on left):**
   - GOOGLE_API_KEY: Your Gemini API key from [Google AI Studio](https://aistudio.google.com/app/apikey)
   - NGROK_AUTHTOKEN: Your authtoken from [ngrok Dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
   - OSC_ALLOWED_ORIGINS: https://parth-ux-arch.github.io
   - *(Optional)* HF_TOKEN: Hugging Face token (for speaker diarization)
   - Toggle **Notebook access** ON for all added secrets.

4. **Run Cells Top to Bottom:**
   - Execute Cell 1 (Environment setup & dependencies)
   - Execute Cell 2 (Configuration & Secrets loading)
   - Execute Cell 3 (Sanity checks: nvidia-smi, ffmpeg)
   - Execute Cell 4 (Start FastAPI server + ngrok tunnel)

5. **Copy the Public URL:**
   - Cell 4 will poll /api/ready and print:
     `
     ==================================================
     BACKEND SERVER READY!
     Public URL: https://xxxx-xx-xx-xx-xx.ngrok-free.app
     ==================================================
     `

6. **Connect the Web Studio:**
   - Open [https://parth-ux-arch.github.io/opensource-clipping/studio/](https://parth-ux-arch.github.io/opensource-clipping/studio/)
   - Click the **Connect** button.
   - Paste the https://....ngrok-free.app URL.
   - Click **Test & Connect**. The indicator turns **Connected**.

---

## Known Limits & Troubleshooting

- **ngrok Interstitial:** On free accounts, if direct API calls fail with 403/HTML, open the ngrok URL directly once in your browser to accept the disclaimer.
- **CORS Origin Mismatch:** OSC_ALLOWED_ORIGINS must match https://parth-ux-arch.github.io (no path).
- **YouTube Blocking (yt-dlp):** If YouTube blocks video downloads, provide a cookies.txt file in the working directory.
- **GPU Quota:** If Colab exhausts GPU quota, switch runtime to CPU (will transcribe and render slower).