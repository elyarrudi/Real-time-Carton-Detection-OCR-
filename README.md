# Real-time Carton Detection & OCR

A real-time computer vision system for detecting cartons from a live camera stream, reading product label text with OCR, counting cartons based on weight labels, and exporting the results to Excel.

## Project video

[Project video](https://drive.google.com/file/d/19K9rEqU0Oi2FIxt3k0d7ghBBpIPVi3dd/view?usp=sharing)

## Features

- Live carton detection from an IP camera stream
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

A **Python side process** is used only where it is more practical:

1. YOLO model preparation / training workflow in Python
2. EasyOCR for reading English text from carton label crops

So the final product is a hybrid system:

- **C++** = live application + inference pipeline + UI
- **Python** = model preparation + OCR service

## Detection pipeline

1. The app connects to a phone IP camera stream and receives live frames.
2. Each frame is processed in C++ with OpenCV.
3. A YOLOv8 model in **ONNX** format is loaded through ONNX Runtime.
4. YOLO detects carton candidates and returns bounding boxes.
5. Each detected carton region is cropped from the frame.
6. The crop is sent to a persistent EasyOCR Python process.
7. OCR returns the label text for that carton.
8. The extracted text is drawn on the corresponding box in the live view.
9. Carton count is calculated from recognized `kg` values in the OCR text.
10. When the count changes, results are written to an Excel file.

## YOLO model usage

- The YOLO weights were prepared in a **Python / ultralytics-style workflow**.
- After training or exporting, the model was converted to **ONNX**.
- In the final application, those ONNX weights are used only for **inference in C++**.
- This means Python is not required for the live detection loop itself; C++ handles realtime inference with ONNX Runtime.

This approach gives two advantages:

- reuse a model trained in the common Python YOLO ecosystem
- keep the live app responsive and self-contained in C++

## OCR and counting logic

After carton detection:

- C++ crops each box
- the crop is passed to EasyOCR
- returned text is shown on the carton

Counting is not based only on the number of YOLO boxes.  
The system also uses OCR content and counts valid `kg` mentions, which better matches labeled product cartons in the scene.

Excel output includes:

- `count box` → number of counted cartons
- individual weight numbers extracted from labels
- `kg product` → sum of those weight values

## Tech stack

- **C++** for the main application, camera handling, inference loop, UI, and Excel export
- **YOLOv8 + ONNX Runtime** for carton detection inference
- **OpenCV** for image processing, frame handling, and visualization
- **EasyOCR (Python)** for label text recognition
- **Win32 UI** for the live desktop viewer
- **Excel export** for final count and weight summary

## Overview

This project was built as a practical industrial vision tool for live carton recognition and weight-based counting from camera input. It combines a C++ realtime pipeline with a Python-trained YOLO model for inference and EasyOCR for label reading.
