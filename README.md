# Guiding-Eye Assist

AI application for visually impaired users at railway stations

Guiding Eye Assist continuously perceives the environment using the device camera, generates concise scene descriptions, and delivers spoken alerts to visually impaired users. The system combines a visual recognition model with a caption generation model, optional translation, and text‑to‑speech to provide real‑time, context‑aware audio feedback tailored for transit environments.

Overview
Guiding Eye Assist captures camera frames, identifies salient objects or regions, generates natural language captions describing the scene, optionally translates captions to the user’s preferred language, and converts the final text into speech. The app is designed for low‑latency, continuous operation in railway stations to help users detect signage, obstacles, platforms, and other relevant cues.

Primary goals
1)Provide continuous situational awareness through spoken descriptions.
2)Demonstrate a reproducible ML pipeline for image understanding and captioning.
3)Offer multilingual support and a lightweight mobile client for real‑world testing.

Key Features
1)Real‑time image perception using a visual recognition model (CNN backbone).
2)Caption generation using a Transformer decoder to produce human‑readable scene descriptions.
3)Multilingual translation to Marathi and Hindi via Google Cloud Translate API.
4)Audio output using Google Text‑to‑Speech or Android TTS for immediate spoken feedback.
5)Haptic feedback to assist visually impaired users in navigating the app UI and selecting options.
6)Android native client developed in Java for on‑device capture and audio I/O.
7)Drive‑mounted dataset workflow for storing and accessing images during development.
8)Data augmentation performed with Roboflow to increase dataset size and variability.

Architecture and Flow
High level pipeline
1)Frame capture
Camera frames are captured continuously or at configurable intervals by the Android client.

2)Preprocessing
Frames are resized, normalized, and optionally augmented to match model input requirements.

3)Visual recognition
A visual model identifies objects or regions of interest in the frame and produces structured outputs (labels, bounding boxes, or feature embeddings).

4)Caption generation
A captioning model consumes visual features and generates a natural language description of the scene.

5)Postprocessing and filtering
Captions are cleaned, filtered for relevance, and formatted for speech.

6)Translation 
Captions are sent to Google Cloud Translate API when language conversion is required.

7)Text to speech
Final captions are converted to audio and played back to the user via the Android client.

8)User interaction
Voice‑to‑text enables simple voice commands for start/stop or language selection where implemented.

Components
Android App — camera capture, permission handling, audio playback, haptic UI, and Java client logic.
Inference Engine — visual recognition model and Transformer captioning model (on‑device for lightweight models or remote for larger models).
Cloud Services — Google Cloud Translate API for Marathi and Hindi translation; optional remote inference endpoints.
Storage — dataset and model artifacts stored on Google Drive and mounted during development.

Dataset
Creation: The dataset was created manually by capturing images representative of railway station environments (platforms, signage, crowds, obstacles).
Augmentation: Standard augmentation techniques were applied to increase dataset size and variability (scaling, rotation, color jitter, cropping).
Storage: Images and annotations were stored on Google Drive and accessed by mounting the drive during training and inference.
Notes: Dataset is tailored to the target domain; domain‑specific augmentation and fine‑tuning improved caption relevance for station scenarios.

Models and Training
Two stage modeling approach
Stage 1 Visual Recognition
A CNN backbone extracts visual features and identifies objects or regions of interest. Outputs from this stage provide structured cues for the captioning stage.

Stage 2 Caption Generation
A Transformer encoder‑decoder pattern (visual encoder + Transformer decoder) generates natural language captions from visual features. The Transformer decoder was used to improve sequence modeling and caption fluency.

Training details
Models were trained on the augmented dataset with standard supervised objectives for detection/classification and sequence cross‑entropy for captioning.
Typical preprocessing included tokenization, vocabulary construction, and padding for sequence inputs.
Checkpoints and model weights were saved to the models directory and loaded by the inference pipeline.

Implementation notes
Visual encoder implemented with a CNN backbone for feature extraction.
Caption decoder implemented as a sequence model  (Transformer decoder) to generate fluent captions.
Inference can be performed on‑device for lightweight models or via a remote server for larger models.

Usage and Demo
Typical user flow
Launch the app and grant camera and microphone permissions.
Select preferred language if translation is required.
Start continuous perception mode to receive spoken descriptions.
Use voice commands to control the app if voice control is enabled.

