# Real-time Carton Detection & OCR

A real-time computer vision system for detecting cartons from a live camera stream, reading label text with OCR, counting cartons, and exporting results to Excel.

## Project video



## Features

- Live carton detection from an IP camera stream
- OCR for  product labels
- Carton counting based on detected `kg` values
- Overlay of extracted text on each detected carton
- Excel export with:
  - `count box`
  - individual weight values
  - total `kg product`

## Tech stack

- C++
- YOLOv8 (ONNX Runtime)
- EasyOCR (Python)
- OpenCV
- Win32 desktop UI
- Excel export

## Overview

This project was built as a practical industrial vision tool for live carton recognition and weight-based counting from camera input.
