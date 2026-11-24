# Raspberry Pi Fruit Recognition for Accessible Self-Checkout

![Fruit classification output from the Raspberry Pi prototype](assets/pi_inference_output.jpg)

This prototype classifies bananas, limes, and oranges from a live camera feed on a Raspberry Pi 3B+. It explores how on-device recognition could reduce the need to look up produce manually at self-checkout. The camera script displays class predictions and scores over the video feed.

Built as a three-person project for **CEN/BMI 598 Embedded Machine Learning** at Arizona State University.

> **Demo to add:** Post a 15–30 second clip from the original project recordings. Show a fruit entering the frame and the predicted label appearing on screen. If the recording shows a misclassification, include it where it helps illustrate the prototype's limits. Replace this note with the video link once the clip is available.

## How it works

| Stage | Implementation |
|---|---|
| Train | A TensorFlow CNN takes 128 × 128 RGB images and predicts one of three fruit classes. The [training notebook](notebooks/train_fruit_classifier.ipynb) shows preprocessing, training, and model export. |
| Deploy | The [TensorFlow Lite model](models/fruit_classifier.tflite) runs on a Raspberry Pi 3B+ with Camera Module v1. |
| Display | The [Python inference script](src/pi_inference.py) captures frames with OpenCV and overlays the predictions and scores. |

[Training pipeline diagram](assets/training_pipeline.png) · [Deployment pipeline diagram](assets/deployment_pipeline.png) · [Project manuscript](docs/manuscript.pdf)

## Run inference on the Raspberry Pi

1. Connect and enable the camera. Confirm OpenCV can open it as a video capture device.
2. Use a Python environment with OpenCV and TensorFlow Lite Support (`tflite-support`) compatible with your Raspberry Pi OS and Python version. Run from a desktop session with display access for the OpenCV preview.
3. From the repository root, run the inference script with the bundled model.

   ```bash
   python3 src/pi_inference.py --model models/fruit_classifier.tflite --cameraId 0
   ```

   Change `--cameraId` if the camera is exposed at a different index.

## Evaluation and scope

- The screenshot shows example output from the Raspberry Pi prototype.
- The archived notebook reuses the training generator for validation and shuffles test images before comparing predictions with labels in directory order. A new evaluation should use a separate validation split and a test generator with shuffling disabled.
- The prototype covers three fruits. It does not include audio output or accessibility user testing.

## Team and credits

Manish Kumar Peteri, Narain Pattabhiraman, and Swathi Mengji.

<!-- Before publishing, add a short "My contributions" section describing the components Manish personally implemented or tested. Use concrete details and distinguish individual work from shared team work. -->

The camera inference script is based on a TensorFlow Lite image classification example and retains the TensorFlow Authors' Apache 2.0 notice.
