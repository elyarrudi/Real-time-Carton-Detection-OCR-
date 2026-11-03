# Real-time Carton Detection & OCR

A real-time computer vision system for detecting cartons from a live camera stream, reading product label text with OCR, counting cartons based on weight labels, and exporting the results to Excel.

## Project video

[Project video](https://drive.google.com/file/d/19K9rEqU0Oi2FIxt3k0d7ghBBpIPVi3dd/view?usp=sharing)

## Features

- Live carton detection from a CCTV / closed-circuit camera stream
- OCR for product labels
- Carton counting based on detected `kg` values
- Overlay of extracted text on each detected carton
- Excel export with:
  - `count box`
  - individual weight values
  - total `kg product`

## System architecture

The project is built as a **C++ Windows desktop application** with a live UI.  
The main realtime loop (camera capture, detection, tracking across frames, drawing boxes/text, and Excel update) runs in C++.

A **Python side process** is used for:

1. Training the carton detection model with YOLO
2. EasyOCR for reading English text from carton label crops

So the final product is a hybrid system:

- **C++** = live application + inference pipeline + UI
- **Python** = YOLO training workflow + OCR service

## Detection pipeline

1. The app connects to a closed-circuit camera stream and receives live frames.
2. Each frame is processed in C++ with OpenCV.
3. The trained carton detection model is loaded in **ONNX** format through ONNX Runtime.
4. The model detects carton candidates and returns bounding boxes.
5. Each detected carton region is cropped from the frame.
6. The crop is sent to a persistent EasyOCR Python process.
7. OCR returns the label text for that carton.
8. The extracted text is drawn on the corresponding box in the live view.
9. Carton count is calculated from recognized `kg` values in the OCR text.
10. When the count changes, results are written to an Excel file.

## Carton detection model

- The carton detection model was **trained with YOLO** in a Python workflow.
- After training, the model was exported to **ONNX** for deployment.
- In the final application, inference runs in **C++** using ONNX Runtime and OpenCV.
- This keeps training in the YOLO/Python ecosystem, while the live system stays fast and stable in C++.

## OCR and counting logic

After carton detection:

- C++ crops each box
- the crop is passed to EasyOCR
- returned text is shown on the carton

Counting is not based only on the number of detection boxes.  
The system also uses OCR content and counts valid `kg` mentions, which better matches labeled product cartons in the scene.

Excel output includes:

- `count box` → number of counted cartons
- individual weight numbers extracted from labels
- `kg product` → sum of those weight values

## Tech stack

- **C++** for the main application, camera handling, inference loop, UI, and Excel export
- **YOLO** for training the carton detection model
- **ONNX Runtime** for C++ inference
- **OpenCV** for image processing, frame handling, and visualization
- **EasyOCR (Python)** for label text recognition
- **Excel export** for final count and weight summary

## Overview

This project was built as a practical industrial vision tool for live carton recognition and weight-based counting from closed-circuit camera input. It combines a C++ realtime pipeline with a YOLO-trained carton detection model and EasyOCR for label reading.
