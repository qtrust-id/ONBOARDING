# Onboarding Guide — Image Vision Engineer

**Team:** Image Vision Engineering  
**Role Overview:** Design, train, evaluate, and deploy the AI and computer vision models that power QTrust's intelligent product features.

---

## 1. Welcome

You are responsible for the intelligence layer of QTrust's products. Your work turns raw image and video data into structured, actionable signals — authenticating products, detecting anomalies, reading barcodes and labels, and classifying items at scale.

Your **primary deliverables** are:

- **Trained models** — versioned, reproducible model artifacts with documented performance
- **Serving API** — a containerized REST endpoint that Backend engineers can call from any QTrust service
- **Model Cards** — mandatory documentation for every model that reaches staging or production
- **Dataset management** — curated, versioned, well-labeled datasets with Dataset Cards

You collaborate closely with:

| Team | How You Interact |
|---|---|
| **Product Development** | Receive the PRD and accuracy/latency targets; confirm feasibility before sprint commitment |
| **Backend Engineer** | Hand off `/predict` endpoint URL and API schema; define the integration contract early |
| **QA / QC** | Provide Model Card and labeled test images; QA validates on held-out set and edge cases |
| **DevOps** | Provide `Dockerfile` and resource requirements; DevOps handles production deployment |
| **Mobile Apps Engineer** | Coordinate on on-device model optimization (TFLite / CoreML / ONNX Mobile) if applicable |

---

## 2. Project Configuration Sheet — AI/Vision Context

> **The Project Configuration Sheet tells you: the AI task type, required accuracy/latency targets, dataset source, model serving infrastructure, and integration method.** Read it before writing a single line of code. If any field is missing or ambiguous, resolve it with the IT Head and Product team before starting.

Key fields you must confirm at project kickoff:

| Field | Description | Example Values |
|---|---|---|
| **AI Task Type** | The core computer vision problem | Classification / Object Detection / OCR / Anomaly Detection / Segmentation |
| **Performance Targets** | Measurable accuracy and speed requirements | mAP ≥ 0.85, inference p95 ≤ 150ms |
| **Dataset Source** | Where training data comes from | Internal capture, Roboflow public, partner data, synthetic |
| **Model Serving** | Deployment target for the inference endpoint | GCP Cloud Run / Vertex AI Prediction / AWS SageMaker |
| **Integration Method** | How Backend consumes your model | REST API (JSON) / gRPC / on-device SDK |
| **Training Infrastructure** | Where formal training runs execute | GCP Vertex AI / AWS SageMaker / Local GPU server |

---

## 3. Tools & Access Checklist

Request all access on Day 1. Do not start development until GitHub and Cloud ML Platform access are confirmed.

| Tool | Purpose | How to Get Access |
|---|---|---|
| **Python 3.11+** | Primary language | Install locally via `pyenv` or system package manager |
| **PyTorch / TensorFlow** | Model training framework | `pip install` — check Project Config Sheet for which framework |
| **OpenCV** | Image processing and augmentation | `pip install opencv-python` |
| **Jupyter Lab** | Exploration and experimentation notebooks | `pip install jupyterlab` |
| **MLflow** | Experiment tracking — hyperparameters, metrics, artifacts | `pip install mlflow` — server URL in Project Config Sheet |
| **Google Drive** | Dataset docs, model cards, reports, shared assets | Editor access — request from IT Head |
| **GitHub** | Source code repository | Read + Write — request from IT Head; specify repo name |
| **Claude Desktop** | Code generation, architecture design, Model Card drafting | Premium account — provisioned by QTrust IT |
| **Cloud ML Platform** | Training infrastructure for formal runs | Check Project Config Sheet for GCP / AWS; request from IT Head |
| **Label Studio / Roboflow** | Dataset labeling and management | Check Project Config Sheet; request from IT Head |

---

## 4. ML Development Lifecycle

```
Phase 1: Problem Definition
        ↓
Phase 2: Dataset Management
        ↓
Phase 3: Experimentation
        ↓
Phase 4: Model Training
        ↓
Phase 5: Evaluation
        ↓
Phase 6: Model Export
        ↓
Phase 7: Deployment
```

### Phase 1 — Problem Definition

- Read the PRD in full; extract the exact computer vision task type
- Define measurable accuracy targets (e.g., mAP ≥ 0.85, F1 ≥ 0.90, OCR accuracy ≥ 0.95)
- Identify operational constraints: maximum latency, model size budget, target hardware (server GPU / CPU / mobile device)
- Confirm with Product team that the targets are achievable given the dataset size and task difficulty
- Document the agreed definition in the Project Config Sheet before proceeding

### Phase 2 — Dataset Management

- Collect or source images according to the dataset source listed in the Project Config Sheet
- Define the labeling schema (class names, bounding box format, annotation guidelines)
- Set up the labeling pipeline in **Label Studio** or **Roboflow**
- Create reproducible **70 / 15 / 15 train / val / test splits** — stratified by class if imbalanced
- Write a **Dataset Card** (see Section 8 for template) before any training begins
- Store raw images in cloud storage only — **never in Git**

### Phase 3 — Experimentation

- Work in `notebooks/` — one notebook per experiment or module
- Log **every** experiment in MLflow: architecture, hyperparameters, dataset version, metrics per epoch
- Try at least two baseline architectures before committing to one
- Apply data augmentation appropriate to the task (flips, rotations, color jitter, mosaic for detection)
- Tag notebook with dataset version and model variant name

### Phase 4 — Model Training

- Move from notebook to `src/training/` for formal training runs
- Use the Cloud ML Platform (Vertex AI / SageMaker) for runs exceeding local GPU budget
- Implement: checkpoint saving every N epochs, early stopping, learning rate scheduling (ReduceLROnPlateau or cosine annealing)
- Save the best checkpoint with full metadata: epoch, val metric, dataset version, hyperparameters
- Log the final training run to MLflow as the canonical experiment

### Phase 5 — Evaluation

- Evaluate on the **held-out test set only** — this set must never influence training or hyperparameter choices
- Generate: confusion matrix, precision/recall curves, per-class metrics
- Review a sample of correct predictions **and** incorrect predictions (at least 20 each)
- Document failure modes — what conditions cause the model to fail?
- Write the **Model Card** (see Section 8) before handing off to QA

### Phase 6 — Model Export

- Export to the appropriate format for your serving target:
  - **ONNX** — universal, recommended default for Cloud Run serving
  - **TorchScript** — PyTorch-native, good for controlled server environments
  - **TF SavedModel** — TensorFlow serving
  - **TFLite / ONNX Mobile** — on-device (confirm with Mobile Engineer)
- Validate that exported model accuracy matches trained model accuracy within 0.1%
- Apply quantization (INT8) or pruning if the model exceeds size or latency targets
- Store exported model in cloud storage; reference via environment variable (see Section 11)

### Phase 7 — Deployment

- Build the serving container using the standard `Dockerfile` in `src/serving/`
- Expose the standard REST endpoint `POST /api/v1/ai/predict` (see Section 5)
- Document the API schema in `docs/api/`
- Hand off the endpoint URL and API schema to the Backend Engineer
- Coordinate with DevOps for production deployment — provide Docker image tag and resource requirements
- Monitor inference latency and error rate for the first 48 hours after launch: track serving errors and inference failures in the `[project-code]-ai-serving` Sentry project ([`tools/sentry.md`](../tools/sentry.md)), and watch inference latency (p95), throughput, and memory in Datadog ([`tools/datadog.md`](../tools/datadog.md)) — connect both MCPs in Claude Desktop

---

## 5. Model Serving API Standard

All QTrust AI models expose a consistent REST API. Backend engineers rely on this contract — do not deviate from it without cross-team agreement.

### Endpoint

```
POST /api/v1/ai/predict
Content-Type: multipart/form-data
Authorization: Bearer {token}
```

### Request Body

```json
{
  "image": "<base64-encoded image string>",
  "options": {
    "confidence_threshold": 0.5
  }
}
```

### Success Response

```json
{
  "success": true,
  "data": {
    "predictions": [
      {
        "label": "authentic",
        "confidence": 0.97,
        "bounding_box": {
          "x": 120,
          "y": 45,
          "width": 200,
          "height": 80
        }
      }
    ],
    "inference_time_ms": 84,
    "model_version": "v2.1.0"
  },
  "message": null
}
```

> `bounding_box` is optional — include only for detection and segmentation tasks.

### Error Response

```json
{
  "success": false,
  "data": null,
  "message": "Invalid image format. Expected JPEG or PNG.",
  "errors": {
    "image": ["The image field must be a valid base64-encoded JPEG or PNG."]
  }
}
```

### Performance Defaults

| Metric | Target |
|---|---|
| Inference latency p95 | ≤ 200ms |
| Throughput | ≥ 10 req/s per instance |
| Model size | ≤ 500MB (prefer ≤ 100MB) |
| Memory per container | ≤ 1GB |

If your model cannot meet these defaults, escalate to the IT Head with benchmark data before deployment.

---

## 6. Project Structure

```
[repo-root]/
├── notebooks/                    # Jupyter notebooks (exploration + training)
├── src/
│   ├── data/                     # Dataset loading, preprocessing, augmentation
│   ├── models/                   # Model architecture definitions
│   ├── training/                 # Training loops, loss functions, schedulers
│   ├── evaluation/               # Metrics, visualisation, test set evaluation
│   └── serving/
│       ├── main.py               # FastAPI app with /predict endpoint
│       ├── predictor.py          # Model loading and inference logic
│       └── schemas.py            # Pydantic request/response schemas
├── models/                       # Saved checkpoints and exported files
│   └── [model-name]/
│       ├── model.onnx
│       └── model-card.md
├── data/
│   └── README.md                 # Dataset source, version, split stats (NOT raw images)
├── Dockerfile
└── requirements.txt
```

---

## 7. Technology Stack

| Layer | Technology |
|---|---|
| **Framework** | PyTorch (recommended) / TensorFlow 2.x |
| **Serving** | FastAPI + Uvicorn |
| **Export Format** | ONNX (universal) / TorchScript / TF SavedModel |
| **Image Processing** | OpenCV + Pillow |
| **Experiment Tracking** | MLflow |
| **Dataset Management** | Roboflow / Label Studio |
| **Training Infrastructure** | GCP Vertex AI / AWS SageMaker / Local GPU |
| **Deployment** | GCP Cloud Run / Vertex AI Prediction |

---

## 8. Dataset Standards & Data Governance

> **Data governance rules — no exceptions:**
>
> - **NEVER** commit raw image or video data to Git — store in cloud storage only
> - **NEVER** include PII (faces, ID numbers, personal identifiers) without consent and legal clearance
> - Every dataset version must have a **Dataset Card** before training begins
> - The **test set must NOT be used** during training or hyperparameter tuning — treat it as production data
> - All dataset access must be logged — use the access log in Google Drive

### Dataset Card Template

```markdown
# Dataset Card — [Dataset Name]

## Identity
- **Name:** [Dataset Name]
- **Version:** v1.0
- **Source:** [Internal capture / Roboflow / Partner / Synthetic]
- **Collection Date:** YYYY-MM-DD

## Statistics
- **Total Samples:** [N images]
- **Classes:**
  - [class_name]: [N samples]
  - [class_name]: [N samples]
- **Train / Val / Test Split:** 70% / 15% / 15%

## Labeling
- **Methodology:** [Manual / Semi-automated / Automated]
- **Labeling Tool:** [Label Studio / Roboflow]
- **Labeler Count:** [N]
- **Inter-annotator Agreement:** [score if applicable]

## Quality Notes
- **Known Limitations:** [class imbalance, lighting conditions, geographic bias, etc.]
- **Usage Restrictions:** [Internal only / Approved external use / etc.]
```

---

## 9. Collaboration with Other Teams

| Team | How You Work Together |
|---|---|
| **Product Development** | Receive accuracy targets and task definition from PRD; confirm feasibility and timeline before committing to sprint; flag unrealistic targets early |
| **Backend Engineer** | Hand off `/predict` endpoint URL and API schema after Phase 7; define the integration contract (request/response format) before starting Phase 3 so Backend can build the integration in parallel |
| **QA / QC** | Provide the Model Card and a labeled set of test images (including edge cases); QA validates on held-out set, adversarial inputs, and real-world scenarios |
| **DevOps** | Provide `Dockerfile`, Docker image tag, and resource requirements (CPU/memory/GPU); DevOps handles production deployment, scaling, and monitoring infrastructure |
| **Mobile Apps Engineer** | If the model runs on-device (TFLite / CoreML / ONNX Mobile), collaborate on model optimization, size budget, and SDK integration before export |

---

## 10. Working with Claude Desktop

Claude Desktop is your AI pair programmer. Connect your project repository as a workspace folder so Claude has full context.

**Effective prompts for common tasks:**

- **Architecture design:**
  > "Design a PyTorch model for [task type]. Input: [size, e.g., 224×224 RGB]. Classes: [list]. Constraints: inference < [N]ms on CPU, model size < [N]MB."

- **FastAPI serving app:**
  > "Write a FastAPI app serving a PyTorch [model type, e.g., EfficientNet classifier]. Include: /predict endpoint, input validation with Pydantic, model loading at startup with torch.load, error handling for corrupt or unsupported images, health check endpoint."

- **Training loop:**
  > "Write a PyTorch training loop for [task, e.g., multi-class image classification]. Include: early stopping with patience=5, ReduceLROnPlateau scheduler, MLflow logging of hyperparameters and per-epoch metrics, checkpoint saving of best val accuracy model."

- **MLflow experiment tracking:**
  > "Write an MLflow experiment tracking setup for a PyTorch training script. Log: all hyperparameters at run start, train/val loss and accuracy per epoch, artifacts (best checkpoint, confusion matrix PNG), dataset version as a run tag."

- **Model Card:**
  > "Generate a Model Card for a [model type, e.g., barcode detection] model. Accuracy: [metrics, e.g., mAP=0.91]. Dataset: [description]. Intended use: [use case]. Known limitations: [issues you found in evaluation]."

> **⚡ Usage tip:** Pulling large datasets, long logs, or whole notebooks into context is token-heavy and can exhaust your daily quota. **Scope your reads** (sample rows, specific log windows), **summarise large outputs** instead of pasting them whole, and use **Sonnet/Haiku** for routine tasks — reserve Opus for hard problems. Full guidance: [Managing Token Usage & Quota](../tools/README.md#managing-token-usage--quota).

---

## 11. Git Workflow

### Branch Naming

```
feature/vision-[task]-[description]
```

**Examples:**
- `feature/vision-detection-barcode`
- `feature/vision-classification-product-auth`
- `feature/vision-ocr-label-reader`

> **Environments:** the serving container follows the same 3-tier flow as every other service. PRs target `develop` (auto-deploys the `/predict` endpoint to **staging**, where Backend and QA validate it); production is a gated deploy promoted from `main`. Reference model artifacts per environment via the `MODEL_PATH` env var — never bake weights into the image. Full flow: [`../environments-and-promotion.md`](../environments-and-promotion.md).

### Commit Convention

| Prefix | Use For |
|---|---|
| `feat(vision):` | New model, new serving endpoint, new training pipeline |
| `fix(vision):` | Bug in preprocessing, inference logic, or serving |
| `test(vision):` | New evaluation scripts, test cases, benchmark scripts |
| `chore(vision):` | Dependency updates, config changes, CI tweaks |
| `docs(vision):` | Model Card, Dataset Card, API schema updates |

### Never Commit

- Raw dataset files (images, video, annotation ZIP exports)
- Trained model weights larger than 50MB — store in cloud storage
- API keys, credentials, or service account JSON files
- Data containing PII

### Large File Reference Pattern

Store models and datasets in cloud storage and reference them via environment variable:

```
MODEL_PATH=gs://qtrust-models/[project]/[version]/model.onnx
DATASET_PATH=gs://qtrust-datasets/[project]/[dataset-version]/
```

Add these variables to `.env.example` with placeholder values so other team members know they are required.

---

## 12. First Week Checklist

- [ ] Read Project Configuration Sheet — note AI task type, performance targets, and dataset source
- [ ] Python environment set up (virtualenv or conda)
- [ ] PyTorch / TensorFlow installed and tested; GPU accessible if applicable
- [ ] MLflow server accessible or local instance running (`mlflow ui`)
- [ ] Dataset source identified and access confirmed (cloud storage bucket or labeling platform)
- [ ] Label Studio or Roboflow access confirmed
- [ ] Claude Desktop installed and project repository connected as workspace folder
- [ ] `by-team/vision/` folder in Google Drive reviewed
- [ ] First exploration notebook started (`notebooks/[module]-exploration.ipynb`)
- [ ] Model architecture proposed and reviewed with IT Head and Product team
- [ ] Dataset Card drafted for the training dataset
- [ ] Integration contract (API schema) agreed with Backend Engineer
- [ ] Sentry MCP connected for `[project-code]-ai-serving`; Datadog MCP connected for inference metrics
- [ ] Attended sprint planning and tasks estimated

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Sentry

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
