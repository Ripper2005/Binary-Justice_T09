# Deepfake Verifier

A hackathon project designed to provide a simple, accessible tool for detecting deepfakes in video evidence. This repository contains the complete backend codebase, including the Flask API server and the integrated machine learning model.

---

##  Backend Architecture Overview

The backend is a self-contained Python application, containerized with Docker for easy deployment. Its primary role is to expose a single API endpoint that accepts a video file and returns a deepfake probability score.

*   **Framework:** **Flask** was chosen for its lightweight and minimalistic nature, making it perfect for a single-purpose API.
*   **Machine Learning:** **PyTorch** is used to run the inference on a pre-trained **EfficientNet-B0** model.
*   **Video Processing:** **OpenCV** handles the video file reading and frame extraction.
*   **Face Detection:** **facenet-pytorch (MTCNN)** is used to accurately detect and crop faces from the video frames before analysis.
*   **Deployment:** The application is containerized with **Docker** and deployed on **Hugging Face Spaces**.

## Backend Codebase Explained

The backend logic is primarily contained within `app.py`, where the web server and the machine learning inference pipeline are integrated.

| File / Component | Role |
| :--- | :--- |
| **`app.py`** | This is the heart of the backend. It contains the **Flask web server**, defines the `/predict` API endpoint, handles file uploads and cleanup, and integrates the entire ML inference logic. |
| **Integrated Inference Logic** | The core logic from our experimental notebook was refactored and moved directly into `app.py`. This logic includes the one-time loading of the models (EfficientNet and MTCNN) at server startup, and the `predict_on_video` function which orchestrates the frame sampling, face detection, preprocessing, and final prediction. |
| **`efficientnet_b0_...pth`** | The pre-trained model weights file. This is loaded once when the application starts, allowing for fast inference on subsequent requests. |
| **`Dockerfile`** | The recipe for building our production environment. It specifies the base Python image, copies the code, installs all dependencies from `requirements.txt`, and defines the command to start the server. |
| **`requirements.txt`** | A list of all Python libraries required for both the Flask server and the machine learning model to function. |

---

## How to Run the Original ML Notebook

This notebook was our original environment for testing the model's inference logic before integrating it into the Flask application. To run it and see the core ML component in action, please follow these steps using Google Colab.

**Prerequisites:**
*   A Google Account for Google Colab.
*   The notebook file (`.ipynb` extension).
*   The model weights file (`efficientnet_b0_epoch_15_loss_0.158.pth`).

### Step-by-Step Instructions:

1.  **Open Google Colab:**
    *   Navigate to [colab.research.google.com](https://colab.research.google.com).
    *   Click `File` > `Upload notebook...` and select the `.ipynb` file from this project.

2.  **Upload the Model Weights:**
    *   After the notebook loads, find the **"Files"** tab on the left-hand sidebar (folder icon).
    *   Click the **"Upload to session storage"** button (paper icon with an up arrow).
    *   Select and upload the `efficientnet_b0_epoch_15_loss_0.158.pth` file.
    *   **Crucially, ensure the notebook and the `.pth` file are in the same root directory.**

3.  **Run the Notebook Cells:**
    *   Run each code cell sequentially from top to bottom by clicking the play button next to each cell or by pressing `Shift + Enter`. (Please use TPUs in colab runtime for faster inference)
    *   The cells will install dependencies, set up the device, and load the face detection and EfficientNet models.

4.  **Upload a Video for Testing:**
    *   The final cells in the notebook are designed for inference. When you run the cell containing `files.upload()`, a button will appear.
    *   Click this button to upload a sample video file from your computer.

5.  **View the Result:**
    *   The last cell will automatically run the `predict_on_video` function on your uploaded video and print the final prediction string directly in the notebook's output.

This process allows you to verify the core machine learning functionality in isolation from the web server.
