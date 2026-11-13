# Report — Task 4

## Goal

I wanted to improve the leaf classifier for photos from a phone. I compared simple changes to LeafNet and also tried pretrained MobileNetV3-Small.

## Data and tests

The saved Task 4 run used 696 training images after removing 4 copies of validation images. It used 120 clean validation images and 120 camera-like versions made with seed 42. The Task 2 dataset was smaller (244 train, 62 validation). The source of the bigger Task 4 split still needs to be explained in the notebook.

## Results

These are results with seed 0 after 15 epochs:

| Model / change | Clean | Camera-like |
| --- | ---: | ---: |
| LeafNet baseline | 81.67% | 77.50% |
| Geometric augmentation | 80.00% | 80.83% |
| Geometric + ColorJitter | 77.50% | 82.50% |
| Dropout | 80.83% | 75.00% |
| Global average pooling | 65.00% | 63.33% |
| AdamW | 80.83% | 75.00% |
| Label smoothing | 82.50% | 76.67% |
| MobileNet, frozen | 84.17% | 78.33% |
| MobileNet, fine-tuned | 90.83% | 90.00% |

I also repeated two MobileNet settings with seeds 0, 1 and 2. These are the mean results for the best clean-validation checkpoint:

| Setting | Clean | Camera-like |
| --- | ---: | ---: |
| MobileNet baseline, 15 epochs | 92.22% | 89.17% |
| Augmentation + cosine, 20 epochs | 92.78% | 90.83% |

## What I found

ColorJitter helped on camera-like photos, but not on clean photos. Dropout, AdamW and global average pooling did not help in these runs. Fine-tuning all of MobileNet worked much better than training only the last layer.

The combination with augmentation and cosine was a little better over three seeds. But 1 image out of 120 changes accuracy by 0.83 percentage points, so the difference can be noise. The results also changed between seeds.

## My choice

I would choose the fine-tuned MobileNet with augmentation and cosine, using the best checkpoint. It had 90.83% average camera-like accuracy. The current web page uses the Task 2 ONNX model, so the Task 4 checkpoint still needs a separate phone test. I did not measure a separate historical validation set.