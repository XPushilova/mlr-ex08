---
name: mlr-ex08
description: Guide Task 4 leaf classifier experiments and submission checks for MLR Exercise 8.
---

# MLR Exercise 8

Follow `AGENTS.md` and read Tasks 2–4 in `ex08_student.ipynb`. The student writes the instructions, chooses the experiments, and judges the results. Start from Task 3 and make only the small, readable PyTorch changes each Task 4 variant needs. Preserve Tasks 1–3 and the notebook structure.

## Data and preprocessing

- Task 2a extracts the teacher's shared Drive file (`GROUP_ZIP`) into `data/leaves/train/<class>/` and `data/leaves/val/<class>/`. The split keeps each student's photos of a species together.
- The optional historical archive has 175 train and 30 validation images per class. Keep the extracted splits separate at `data/last_year/train/<class>/` and `data/last_year/val/<class>/`. [Historical ZIP from Task 4](https://raw.githubusercontent.com/tomasvicar/MLR-public/master/exercises/data/ex08_leaves_images.zip)
- The current `data/leaves` folders match the archive's per-class counts and lack `.group_zip_url`. Verify their source before using them as group data.
- Class indices 0–3 are `apple`, `cherry`, `chestnut`, `maple`. Group photos are already center-cropped and resized to 96 × 96. On the phone, center-crop a square and resize it to 96 × 96. `ToTensor()` puts RGB channels first and scales pixels by `1/255`. `Normalize()` uses mean `[0.485, 0.456, 0.406]` and std `[0.229, 0.224, 0.225]`. Input shape is `(N, 3, 96, 96)`, with `N=1` on the phone.

## Task 3 pipeline

- `LeafDataset` sorts class folders and image files. `__len__` returns the sample count. `__getitem__` opens the image with PIL, converts it to RGB, applies the transform, and returns the tensor and class index.
- `LeafNet` has four 3 × 3 convolutions with padding 1 and 16, 32, 64, and 64 output channels. Each is followed by ReLU and 2 × 2 max pooling. The network flattens the 64 × 6 × 6 output, applies a 64-unit linear layer with ReLU, and returns four logits. `LeafNetBN` adds `BatchNorm2d` after each convolution. The geometric training variant uses a resized crop at scale `(0.6, 1.0)`, horizontal and vertical flips, and rotation up to 20°.
- The baseline trains from scratch with seed 0, batches of 32, Adam at `1e-3`, and cross-entropy on logits for 15 epochs. Only the training loader shuffles. Each epoch uses `train()` and moves batches to the model's device. For each batch, clear gradients, compute logits and loss, backpropagate, and step the optimizer. Use `eval()` and `torch.no_grad()` to count correct `argmax` predictions. Print mean batch loss and validation accuracy each epoch. The baseline score is the final epoch's accuracy.

## Task 4 requirements

1. Use one fixed baseline, either Task 3 `LeafNet` or Task 2's fully fine-tuned pretrained MobileNetV3 Small. Compare one change at a time against it, then try the best combinations. Keep training images, splits, and seeds fixed. When testing extra historical data as the change, add only its `train` images. Repeat important runs with two or three seeds and report only measured results.
2. Measure every variant on the same clean validation split and the same camera-like set. Generate the camera-like set once from clean validation with a fixed seed, using random crops, rotations, and color changes. The notebook accepts historical validation as an alternative second evaluation. Under `AGENTS.md`, report any historical validation separately. Keep all validation images out of training and tuning.
3. The student writes a short `REPORT.md` with a results table, what each change brought, whether gains exceed noise, what did not help, and the model chosen for the phone. Export that model to ONNX as in Task 2e with its class order and preprocessing metadata, and try it in the course camera app. Preserve the working Task 2 app and assets.

Candidates include `ColorJitter` with geometric augmentation, training only the new MobileNet classifier versus full fine-tuning, and pretrained MobileNet with augmentation, a learning-rate scheduler such as cosine, and more epochs. Also consider dropout, global average pooling to replace flattening, AdamW with weight decay, label smoothing, best-epoch weights, and historical training photos.

## Submission checklist

Task 2 and the Task 4 report are compulsory for attendance credit. Push both to the public `mlr-ex08` GitHub repository within 48 hours after the lab. Follow `AGENTS.md` for authorization to use Git or GitHub, deploy, or invite collaborators.

- [ ] `index.html` live on GitHub Pages with on-device camera classification on Android and iPhone. Put the live URL in `README.md`.
- [ ] `leaf_model.onnx`, `model_info.json`, `check_image.jpg`, and `check_expected.json` from `ex08_for_agent.zip`. Keep the full group dataset and ZIPs out of Git. Check `check_image.jpg` for sensitive content before committing.
- [ ] Student-written `REVIEW.md` documents what was tested, found, and fixed for four checks. Show live probabilities with a leaf in a phone screenshot. Upload `check_image.jpg` in the app and compare probabilities with `check_expected.json` to three decimals. Verify crop, resize, scaling, normalization, and channel order against the notebook and `model_info.json`. Verify images stay on the phone and explain which URLs the page loads and why.
- [ ] Student-written `REPORT.md` beside `REVIEW.md`.
- [ ] `MLRteacher` invited as a collaborator. A pending invitation counts.

The suggested Task 2 brief also includes a labelled QR code generated locally or in the browser, with a clickable text link to the verified Pages URL.
