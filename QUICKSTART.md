# Quick Start Guide - GTX 1650

**For Team Lead - Day 1 Immediate Actions**

---

## 🚀 Quick Install (Windows)

```powershell
# 1. Create virtual environment
python -m venv venv

# 2. Activate it
.\venv\Scripts\Activate.ps1

# 3. Install dependencies
pip install --upgrade pip
pip install -r requirements.txt

# 4. Run WebUI
.\start-webui.bat

# 5. Open browser: http://localhost:7860
```

---

## 🎯 First Test (Quickest Path to SRT)

1. **Open**: http://localhost:7860
2. **Go to**: "File" tab
3. **Upload**: Short video (1-3 min MP4)
4. **Settings**:
   - Model: `base` ✅ (fastest for GTX 1650)
   - Language: `auto-detect` or your language
   - Task: `transcribe`
   - Format: `SRT`
5. **Click**: "Run"
6. **Wait**: ~2-5 minutes (depends on video length)
7. **Find**: Output in `outputs/` folder

---

## ⚡ Recommended Settings for GTX 1650

### Best Performance/Quality Balance
```yaml
Model: small
Whisper Type: faster-whisper
VAD: Enabled
Language: auto-detect
```

### Fastest (for testing)
```yaml
Model: base
Whisper Type: faster-whisper
VAD: Disabled
Language: specify (don't use auto)
```

### Best Quality (may be slow)
```yaml
Model: small or medium
Whisper Type: faster-whisper
VAD: Enabled
Language: auto-detect
```

⚠️ **Avoid**: `large` or `large-v2` models - they will likely run out of memory on GTX 1650!

---

## 🔧 Quick Troubleshooting

### CUDA Not Available?
```powershell
python -c "import torch; print(torch.cuda.is_available())"
# If False:
pip uninstall torch torchvision torchaudio
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### FFmpeg Missing?
```powershell
# Install via Chocolatey
choco install ffmpeg

# Or download: https://ffmpeg.org/download.html
# Then add to PATH
```

### Port Busy?
```powershell
netstat -ano | findstr :7860
taskkill /PID <PID> /F
```

---

## 📊 Expected Performance on GTX 1650

| Video Length | Model | Processing Time | VRAM Usage |
|--------------|-------|-----------------|------------|
| 3 min        | base  | ~30-60 sec      | ~1GB       |
| 3 min        | small | ~1-2 min        | ~2GB       |
| 10 min       | base  | ~2-3 min        | ~1GB       |
| 10 min       | small | ~4-6 min        | ~2GB       |

---

## 📝 After First Success

1. Create `TEST_RESULTS.md`:
   ```markdown
   # First Test Results
   - Date: [today]
   - Video: [name, duration]
   - Model: [base/small]
   - Time: [processing time]
   - Output: ✅ Success / ❌ Failed
   - Issues: [any problems]
   ```

2. Commit to dev branch:
   ```powershell
   git add TEST_RESULTS.md
   git commit -m "Add first test results on GTX 1650"
   git push origin dev
   ```

3. Create GitHub Issues (use `GITHUB_ISSUES.md` templates)

---

## 🎓 Next Steps After Day 1

- [ ] Test different models (base, small, medium)
- [ ] Try YouTube URL input
- [ ] Test with different languages
- [ ] Test background music separation
- [ ] Test speaker diarization
- [ ] Review backend API
- [ ] Create issues for bugs/improvements

---

## 📚 Full Documentation

- **Complete Setup**: See `SETUP.md`
- **Day 1 Checklist**: See `DAY1_CHECKLIST.md`
- **GitHub Issues**: See `GITHUB_ISSUES.md`
- **Original README**: See `README.md`

---

**Good Luck!** 🚀

If you encounter any issues, check `SETUP.md` for detailed troubleshooting or create a GitHub issue.
