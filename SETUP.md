# LateScript-Sub Setup Guide

This guide provides step-by-step instructions to set up and run the LateScript-Sub application on your local machine.

## System Prerequisites

### Hardware Requirements
- **GPU**: NVIDIA GPU with at least 4GB VRAM (tested on GTX 1650)
- **RAM**: Minimum 8GB (16GB recommended)
- **Storage**: At least 10GB free space for models

### Software Requirements
- **Python**: 3.8 or higher (3.10 recommended)
- **Git**: Latest version
- **NVIDIA Drivers**: Latest NVIDIA GPU drivers
- **CUDA**: CUDA 11.8 or 12.1 (compatible with PyTorch)

### Verify NVIDIA Setup
```bash
# Check NVIDIA driver and CUDA version
nvidia-smi
```

You should see your GPU listed with driver version and CUDA version. If not, install NVIDIA drivers from: https://www.nvidia.com/Download/index.aspx

---

## Installation Methods

### Method 1: Local Installation (Recommended for Development)

#### Step 1: Clone the Repository
```bash
# Clone your forked repository
git clone https://github.com/<your-username>/LateScript-Sub.git
cd LateScript-Sub

# Switch to dev branch
git checkout dev
```

#### Step 2: Create Virtual Environment
```bash
# Windows (PowerShell/CMD)
python -m venv venv
venv\Scripts\activate

# Linux/Mac
python -m venv venv
source venv/bin/activate
```

#### Step 3: Install Dependencies
```bash
# Upgrade pip
pip install --upgrade pip

# Install main requirements
pip install -r requirements.txt

# Install backend requirements (if using backend API)
pip install -r backend/requirements-backend.txt

# Install faster-whisper for optimized performance
pip install faster-whisper
```

#### Step 4: Run the Application

**Option A: WebUI (Gradio Interface)**
```bash
# Windows
start-webui.bat

# Linux/Mac
bash start-webui.sh
```

**Option B: Backend API**
```bash
cd backend
python main.py
```

#### Step 5: Access the Application
Open your browser and navigate to:
- **WebUI**: http://localhost:7860
- **Backend API**: http://localhost:8000 (if running backend separately)

---

### Method 2: Docker Installation

#### Prerequisites
- Docker Desktop installed and running
- Docker Compose installed

#### Run with Docker
```bash
# Clone the repository
git clone https://github.com/<your-username>/LateScript-Sub.git
cd LateScript-Sub

# Build and run with Docker Compose
docker compose build
docker compose up

# Access at http://localhost:7860
```

To stop the containers:
```bash
docker compose down
```

---

## First Test: Generate SRT from Sample Video

### Step 1: Prepare a Test Video
- Use a short video file (MP4, AVI, etc.) - 1-5 minutes recommended for first test
- Place it in an accessible location or use a YouTube URL

### Step 2: Process Video in WebUI
1. Open http://localhost:7860
2. Go to the **Transcription** tab
3. Upload your video or paste YouTube URL
4. Select settings:
   - **Model**: `base` or `small` (faster for testing, `large-v2` for accuracy)
   - **Language**: Select source language or `auto` for detection
   - **Task**: `transcribe`
5. Click **Run** and wait for processing

### Step 3: Verify Output
- SRT file will be saved in `outputs/` directory
- Check the output for accuracy
- Note processing time and GPU memory usage

### Example Test Command (if using CLI)
```python
# Test script example
import whisper

model = whisper.load_model("base")
result = model.transcribe("test_video.mp4")

# Save as SRT
with open("test_output.srt", "w", encoding="utf-8") as f:
    # Write SRT format
    pass
```

---

## Common Issues & Troubleshooting

### Issue 1: CUDA Out of Memory
**Symptoms**: `CUDA out of memory` error during processing

**Solutions**:
- Use a smaller Whisper model (`tiny`, `base`, `small`)
- Reduce batch size in config
- Close other GPU-intensive applications
- Process shorter video segments

### Issue 2: CUDA Not Available
**Symptoms**: Model running on CPU (very slow), no GPU detected

**Solutions**:
```bash
# Check PyTorch CUDA availability
python -c "import torch; print(torch.cuda.is_available())"

# If False, reinstall PyTorch with CUDA support
pip uninstall torch torchaudio
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Issue 3: Missing Dependencies
**Symptoms**: Import errors, module not found

**Solutions**:
```bash
# Reinstall all dependencies
pip install -r requirements.txt --force-reinstall
```

### Issue 4: Port Already in Use
**Symptoms**: `Address already in use` error

**Solutions**:
```bash
# Windows - Find and kill process on port 7860
netstat -ano | findstr :7860
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:7860 | xargs kill -9
```

### Issue 5: FFmpeg Not Found
**Symptoms**: Audio extraction errors

**Solutions**:
```bash
# Windows (using chocolatey)
choco install ffmpeg

# Linux (Ubuntu/Debian)
sudo apt update
sudo apt install ffmpeg

# Mac
brew install ffmpeg
```

---

## Configuration

### Model Configuration
Edit `configs/default_parameters.yaml` to customize:
- Default Whisper model
- VAD (Voice Activity Detection) settings
- Translation settings
- Output formats

### Backend Configuration
Edit `backend/configs/config.yaml` for:
- API port settings
- Database configuration
- Cache settings
- CORS settings

---

## Development Workflow

### Running Tests
```bash
# Run all tests
pytest tests/

# Run specific test
pytest tests/test_transcription.py

# Run backend tests
pytest backend/tests/
```

### Code Structure
- `app.py` - Main Gradio WebUI application
- `backend/main.py` - FastAPI backend server
- `modules/whisper/` - Whisper transcription logic
- `modules/uvr/` - Background music separation
- `modules/diarization/` - Speaker diarization
- `modules/translation/` - Translation functionality

---

## Next Steps

1. ✅ Clone repository and create dev branch
2. ✅ Set up local environment
3. ✅ Generate first SRT file
4. 📝 Document your specific setup steps
5. 🐛 Create issues for bugs found
6. 🚀 Start development tasks

---

## Team Collaboration

### Creating Issues
When you encounter bugs or have feature requests:
1. Go to GitHub Issues
2. Use issue templates if available
3. Provide:
   - Clear description
   - Steps to reproduce
   - Expected vs actual behavior
   - System info (GPU, OS, Python version)

### Branch Strategy
- `master` - Stable production code
- `dev` - Development branch (current)
- `feature/*` - Feature branches
- `bugfix/*` - Bug fix branches

---

## Resources

- **Whisper Documentation**: https://github.com/openai/whisper
- **Faster Whisper**: https://github.com/guillaumekln/faster-whisper
- **Gradio Documentation**: https://www.gradio.app/docs/
- **FastAPI Documentation**: https://fastapi.tiangolo.com/

---

## Support

If you encounter issues not covered in this guide:
1. Check existing GitHub Issues
2. Create a new issue with detailed information
3. Contact team lead

---

**Last Updated**: October 27, 2025  
**Tested On**: Windows 11, NVIDIA GTX 1650, Python 3.10, CUDA 11.8
