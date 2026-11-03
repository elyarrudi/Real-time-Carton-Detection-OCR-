# Real-time Carton Detection & OCR

A real-time computer vision system for detecting cartons from a live camera stream, reading label text with OCR, counting cartons, and exporting results to Excel.

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

## How it works

The application is mainly written in **C++** and runs as a Windows desktop app with a live camera viewer.

- **Detection:** A YOLOv8 model (exported to ONNX) runs in C++ through ONNX Runtime and OpenCV to find cartons in each frame.
- **Model origin:** The YOLO weights were prepared/trained in a **Python** workflow, then converted to ONNX and used only for **inference** inside the C++ app.
- **OCR:** Detected carton regions are cropped in C++ and sent to a lightweight Python EasyOCR process that returns the label text.
- **Counting logic:** The system counts cartons based on recognized `kg` values in the OCR output, then updates the on-screen result and Excel file when the count changes.

This hybrid setup keeps the live pipeline fast in C++, while reusing a Python-trained YOLO model and EasyOCR for text reading.

## Tech stack

- C++
- YOLOv8 (ONNX Runtime)
- EasyOCR (Python)
- OpenCV
- Win32 desktop UI
- Excel export

## Overview

This project was built as a practical industrial vision tool for live carton recognition and weight-based counting from camera input.
