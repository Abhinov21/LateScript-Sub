# Day 1 Checklist - Setup & Baseline Testing

**Objective**: Get app running on GTX 1650 and produce first .srt file

**Time Estimate**: 4-6 hours

---

## ✅ Deliverables Completed

### 1. Repository Setup
- [x] Created `dev` branch
- [x] Created comprehensive `SETUP.md` with installation instructions
- [ ] Pushed `dev` branch to origin (run: `git push -u origin dev`)
- [ ] Created GitHub Issues for team tasks

---

## 📋 Installation Checklist

### Prerequisites Verification
- [ ] **NVIDIA Drivers Installed**
  ```powershell
  nvidia-smi
  ```
  - Should show GPU: GTX 1650
  - Should show driver version
  - Should show CUDA version (11.8 or 12.x)

- [ ] **Python Installed** (3.10 recommended)
  ```powershell
  python --version
  ```

- [ ] **Git Installed**
  ```powershell
  git --version
  ```

- [ ] **FFmpeg Installed**
  ```powershell
  ffmpeg -version
  ```
  - If not installed: `choco install ffmpeg` (requires Chocolatey)
  - Or download from https://ffmpeg.org/download.html
  - Add to PATH: `C:\path\to\ffmpeg\bin`

---

## 🚀 Setup Steps

### Step 1: Clone and Branch
```powershell
# Clone repository (if not already done)
git clone https://github.com/<your-username>/LateScript-Sub.git
cd LateScript-Sub

# Switch to dev branch
git checkout dev
```

### Step 2: Create Virtual Environment
```powershell
# Create venv
python -m venv venv

# Activate (Windows PowerShell)
.\venv\Scripts\Activate.ps1

# If you get execution policy error:
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Step 3: Install Dependencies
```powershell
# Upgrade pip
pip install --upgrade pip

# Install main requirements
pip install -r requirements.txt

# Install faster-whisper (optimized for GTX 1650)
pip install faster-whisper

# Verify PyTorch CUDA support
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'CUDA version: {torch.version.cuda}'); print(f'GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else None}')"
```

**Expected Output:**
```
CUDA available: True
CUDA version: 11.8 (or 12.x)
GPU: NVIDIA GeForce GTX 1650
```

---

## 🎯 First Test: Generate SRT

### Step 4: Run the WebUI
```powershell
# Windows (run the batch file)
.\start-webui.bat

# Should see output like:
# Running on local URL:  http://127.0.0.1:7860
```

### Step 5: Test Transcription
1. **Open Browser**: Navigate to `http://localhost:7860`

2. **Prepare Test Video**:
   - Use a short video (1-3 minutes recommended)
   - Supported formats: MP4, AVI, MOV, etc.
   - Or use a YouTube URL

3. **Configure Settings** (in WebUI):
   - **Tab**: "File" or "Youtube"
   - **Whisper Implementation**: Select `faster-whisper` (default, optimized)
   - **Model**: Start with `base` or `small` (faster, good for testing)
     - `tiny`: Fastest, least accurate (~1GB VRAM)
     - `base`: Fast, decent accuracy (~1GB VRAM)
     - `small`: Good balance (~2GB VRAM)
     - `medium`: Better accuracy (~5GB VRAM) - **May be tight on GTX 1650!**
     - `large-v2`: Best accuracy (~10GB VRAM) - **Will likely OOM on GTX 1650!**
   - **Language**: Select language or use `auto-detect`
   - **Task**: `transcribe`
   - **Subtitle Format**: `SRT`

4. **Upload and Run**:
   - Upload your test video
   - Click "Run" button
   - Monitor progress in terminal

5. **Verify Output**:
   - SRT file saved in `outputs/` directory
   - Check file format and content
   - Note processing time

### Step 6: Document Your Results

Create a file `TEST_RESULTS.md` with:

```markdown
# First Test Results

**Date**: [Date]
**Tester**: [Your Name]
**Hardware**: GTX 1650, [RAM amount]GB RAM

## Test Configuration
- **Video**: [Name/Description, duration]
- **Model**: [e.g., base, small]
- **Language**: [e.g., English, auto-detect]
- **Implementation**: faster-whisper

## Results
- ✅/❌ **Success**: [Yes/No]
- **Processing Time**: [e.g., 2 minutes for 5-minute video]
- **Output Location**: outputs/[filename].srt
- **GPU Memory Used**: [Check nvidia-smi during processing]
- **Accuracy**: [Spot check - good/fair/poor]

## Issues Encountered
- [List any errors or problems]

## Notes
- [Any observations, suggestions, etc.]
```

---

## 🐛 Common Issues on GTX 1650

### Issue 1: CUDA Out of Memory
**Symptoms**: Error during model loading or processing
```
torch.cuda.OutOfMemoryError: CUDA out of memory
```

**Solutions**:
1. Use smaller model (`tiny` or `base`)
2. Close other GPU applications
3. Reduce video resolution before processing
4. Process in smaller chunks

### Issue 2: Slow Processing (CPU Fallback)
**Symptoms**: Very slow transcription, high CPU usage

**Check**:
```powershell
python -c "import torch; print(torch.cuda.is_available())"
```

If returns `False`, reinstall PyTorch with CUDA:
```powershell
pip uninstall torch torchaudio torchvision
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Issue 3: FFmpeg Not Found
**Symptoms**: 
```
FileNotFoundError: [WinError 2] The system cannot find the file specified
```

**Solution**:
```powershell
# Install via Chocolatey
choco install ffmpeg

# Or download manually and add to PATH
# https://ffmpeg.org/download.html
```

### Issue 4: Port Already in Use
**Symptoms**:
```
OSError: [Errno 10048] error while attempting to bind on address ('127.0.0.1', 7860)
```

**Solution**:
```powershell
# Find process using port 7860
netstat -ano | findstr :7860

# Kill the process (replace <PID> with actual PID)
taskkill /PID <PID> /F
```

---

## 📊 Performance Expectations (GTX 1650 - 4GB VRAM)

| Model  | VRAM Usage | Speed (Real-time Factor) | Accuracy |
|--------|------------|--------------------------|----------|
| tiny   | ~1GB       | 0.1x (very fast)         | Fair     |
| base   | ~1GB       | 0.2x (fast)              | Good     |
| small  | ~2GB       | 0.3x (moderate)          | Better   |
| medium | ~5GB       | ⚠️ **May OOM**           | Great    |
| large  | ~10GB      | ❌ **Will OOM**          | Best     |

**Recommendation**: Start with `base` or `small` model for GTX 1650.

---

## ✅ Final Checklist

Before marking Day 1 complete:

- [ ] `nvidia-smi` shows GTX 1650 is detected
- [ ] Virtual environment created and activated
- [ ] All dependencies installed without errors
- [ ] `torch.cuda.is_available()` returns `True`
- [ ] WebUI launches at http://localhost:7860
- [ ] Successfully processed a test video
- [ ] Generated .srt file in `outputs/` directory
- [ ] Verified .srt file content is reasonable
- [ ] Documented setup commands in `SETUP.md` ✅
- [ ] Created `TEST_RESULTS.md` with your results
- [ ] Committed and pushed `dev` branch
- [ ] Created GitHub issues for next tasks

---

## 📝 Next Steps (Day 2+)

After completing Day 1:
1. Review generated SRT for accuracy
2. Create issues for bugs found
3. Test different models and settings
4. Document optimal settings for GTX 1650
5. Begin feature development tasks

---

## 🆘 Need Help?

If stuck:
1. Check `SETUP.md` for detailed troubleshooting
2. Review existing GitHub Issues
3. Create new issue with:
   - Error message (full stack trace)
   - Steps to reproduce
   - System info (`nvidia-smi` output, Python version)
   - What you've tried

---

**Last Updated**: October 27, 2025  
**Status**: Dev branch ready, awaiting first test run
