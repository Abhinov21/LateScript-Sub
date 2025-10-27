# GitHub Issues Template

Use these templates to create issues for team collaboration.

---

## Issue 1: Verify Installation on GTX 1650

**Title**: [Setup] Verify installation and baseline test on GTX 1650

**Labels**: setup, testing, day-1

**Description**:
```markdown
## Objective
Verify that the application runs successfully on GTX 1650 GPU and can produce SRT files.

## Tasks
- [ ] Install system prerequisites (Python, FFmpeg, CUDA)
- [ ] Create virtual environment
- [ ] Install dependencies from requirements.txt
- [ ] Verify CUDA is available (`torch.cuda.is_available()`)
- [ ] Run WebUI on http://localhost:7860
- [ ] Process a test video (1-3 minutes)
- [ ] Generate SRT file successfully

## Expected Outcome
- Application runs without errors
- Generated .srt file in `outputs/` directory
- Processing completes in reasonable time

## Hardware
- GPU: GTX 1650 (4GB VRAM)
- CUDA Version: [To be filled]
- Driver Version: [To be filled]

## Deliverable
- Update `DAY1_CHECKLIST.md` with completion status
- Create `TEST_RESULTS.md` with test results
- Report any issues encountered
```

**Assignee**: [Team Lead]

---

## Issue 2: Test Different Whisper Models

**Title**: [Testing] Compare Whisper model performance on GTX 1650

**Labels**: testing, performance, day-1

**Description**:
```markdown
## Objective
Test different Whisper models to determine optimal configuration for GTX 1650.

## Models to Test
- [ ] tiny
- [ ] base
- [ ] small
- [ ] medium (may fail due to VRAM)

## Test Procedure
For each model:
1. Use same test video (3-5 minutes)
2. Record processing time
3. Check VRAM usage during processing (`nvidia-smi`)
4. Evaluate transcription accuracy (spot check)

## Data to Collect
- Processing time
- Peak VRAM usage
- Accuracy assessment (good/fair/poor)
- Any errors or warnings

## Expected Outcome
- Recommendations for optimal model on GTX 1650
- Documentation of VRAM usage per model
- Performance benchmarks

## Deliverable
Create `PERFORMANCE_TESTS.md` with results table
```

**Assignee**: [Team Member]

---

## Issue 3: Document Common Errors

**Title**: [Documentation] Document common errors and solutions

**Labels**: documentation, day-1

**Description**:
```markdown
## Objective
Document common errors encountered during setup and their solutions.

## Tasks
- [ ] Test installation on fresh Windows machine
- [ ] Document CUDA-related errors
- [ ] Document FFmpeg-related errors
- [ ] Document dependency conflicts
- [ ] Test solutions for each error

## Errors to Cover
1. CUDA out of memory
2. CUDA not available / CPU fallback
3. FFmpeg not found
4. Port already in use
5. Module import errors

## Deliverable
- Update `SETUP.md` troubleshooting section
- Create FAQ if needed
```

**Assignee**: [Team Member]

---

## Issue 4: Test Docker Installation

**Title**: [Testing] Verify Docker installation method

**Labels**: docker, testing, day-1

**Description**:
```markdown
## Objective
Verify that Docker installation method works correctly.

## Tasks
- [ ] Install Docker Desktop
- [ ] Build Docker image from docker-compose.yaml
- [ ] Run container
- [ ] Access WebUI at http://localhost:7860
- [ ] Process test video in Docker environment
- [ ] Compare performance with local installation

## Expected Outcome
- Docker container builds successfully
- WebUI accessible and functional
- GPU accessible from container

## Notes
- Docker image is ~7GB
- Requires Docker Desktop with WSL2 backend (Windows)

## Deliverable
- Update `SETUP.md` with Docker instructions validation
- Document any Docker-specific issues
```

**Assignee**: [Team Member]

---

## Issue 5: Test Faster-Whisper Backend

**Title**: [Testing] Verify faster-whisper implementation

**Labels**: testing, optimization, day-1

**Description**:
```markdown
## Objective
Verify faster-whisper implementation works and provides performance benefits.

## Tasks
- [ ] Install faster-whisper
- [ ] Run same test video with faster-whisper
- [ ] Compare with standard Whisper
- [ ] Document VRAM usage difference
- [ ] Document speed difference

## Comparison Metrics
- Processing time
- VRAM usage
- CPU usage
- Accuracy (should be same)

## Expected Outcome
- faster-whisper shows reduced VRAM usage
- Faster processing time
- Same or similar accuracy

## Reference
According to docs:
- Standard whisper (fp16): 11325MB VRAM, 4m30s
- faster-whisper (fp16): 4755MB VRAM, 54s

## Deliverable
Add comparison results to `PERFORMANCE_TESTS.md`
```

**Assignee**: [Team Member]

---

## Issue 6: Test YouTube URL Input

**Title**: [Testing] Verify YouTube URL transcription

**Labels**: testing, feature, day-1

**Description**:
```markdown
## Objective
Test YouTube URL input functionality.

## Tasks
- [ ] Find short test video on YouTube (1-3 minutes)
- [ ] Enter URL in WebUI
- [ ] Process and generate SRT
- [ ] Verify output quality
- [ ] Test with different languages

## Test Cases
1. English video
2. Non-English video (if applicable)
3. Video with background music
4. Video with multiple speakers

## Expected Outcome
- YouTube download works
- Audio extraction successful
- SRT generation works same as file upload

## Deliverable
- Document YouTube feature in `SETUP.md`
- Report any issues
```

**Assignee**: [Team Member]

---

## Issue 7: Backend API Testing

**Title**: [Testing] Test FastAPI backend

**Labels**: testing, backend, day-1

**Description**:
```markdown
## Objective
Verify that the FastAPI backend works independently.

## Tasks
- [ ] Navigate to `/backend` directory
- [ ] Install backend requirements
- [ ] Run `python main.py`
- [ ] Access API docs at http://localhost:8000/docs
- [ ] Test transcription endpoint
- [ ] Test task status endpoint

## Expected Outcome
- Backend runs on port 8000
- Swagger docs accessible
- API endpoints respond correctly
- Tasks are tracked in database

## Deliverable
- Update `backend/README.md` with test results
- Document API usage examples
```

**Assignee**: [Team Member]

---

## How to Create These Issues on GitHub

1. Go to your repository: `https://github.com/<your-username>/LateScript-Sub`
2. Click on "Issues" tab
3. Click "New Issue"
4. Copy-paste the title and description from above
5. Add appropriate labels
6. Assign to team member
7. Click "Submit new issue"

## Labels to Create

Create these labels in your repository:
- `setup` - Setup and installation tasks
- `testing` - Testing tasks
- `documentation` - Documentation tasks
- `performance` - Performance-related
- `day-1` - Day 1 tasks
- `bug` - Bug reports
- `enhancement` - Feature requests
- `docker` - Docker-related
- `backend` - Backend API related
- `optimization` - Optimization tasks

---

**Note**: Customize assignees based on your team members!
