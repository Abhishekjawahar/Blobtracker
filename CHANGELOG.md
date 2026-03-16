# Changelog

## v10 — Current (Browser-native)
- Complete rewrite as pure HTML/JS — no Python, no server
- True real-time performance using requestAnimationFrame
- Front/rear camera toggle
- Record and download to device gallery
- Frame differencing replaces background subtractor
- BFS flood fill blob detection
- All math overlays ported to JavaScript

## v9 — Hugging Face / Gradio
- Gradio 6 compatible rewrite
- gr.Interface with live=True streaming
- Frame differencing for instant detection

## v8 — Streamlit Cloud
- streamlit-webrtc integration
- Mobile camera support attempt
- WebRTC configuration for iOS

## v7 — Python Desktop (Final)
- Movement-only detection with MOG2
- Background subtractor
- Optical flow, convex hull, velocity heatmap
- Overlap detection with red warning
- Processing thread + display thread architecture

## v6 — Python Desktop
- Parabolic curve fit
- Gradient vectors
- Fibonacci spiral (removed)
- Trail lines with neon green

## v5 — Python Desktop
- Threaded architecture (processing + UI)
- Frame queue for smooth display
- Source FPS-matched recording

## v4 — Python Desktop
- Auto-threshold (Otsu's method)
- Movement sensitivity slider
- Scrollable UI with full screen support

## v3 — Python Desktop
- Full Tkinter UI rewrite
- Glassmorphism dark theme
- Video file picker (no hardcoded path)
- Blob count live display

## v1–v2 — Python Desktop (Initial)
- Basic OpenCV blob detection
- SimpleBlobDetector
- Manual threshold slider
- Webcam only
```
