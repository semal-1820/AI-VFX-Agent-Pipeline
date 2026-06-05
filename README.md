# AI-VFX-Agent-Pipeline

1. Project Overview
This project implements an automated, text-guided Visual Effects (VFX) agent. The pipeline accepts an original image, a target object to replace (e.g., "person", "sky"), and a generative prompt (e.g., "a vibrant cosmic nebula"). The agent autonomously handles object detection, semantic masking, and generative inpainting to produce realistic edits.

2. The architecture flows in three distinct stages:

Zero-Shot Object Detection: Parsing the user's natural language target and locating its bounding box coordinates within the frame.

Instance Segmentation: Converting the bounding box into a highly precise, pixel-perfect binary mask.

Generative Inpainting: Using the mask to isolate the target area and synthesizing new visual elements based on the prompt, ensuring the new generation matches the existing lighting and depth geometry.

3. Tools and Frameworks Used
A. Hugging Face Ecosystem (transformers, diffusers)
Why I chose it: Hugging Face provides rapid, standardized access to foundational AI models. Using these libraries allowed me to download, configure, and push heavy models to the GPU with minimal boilerplate, which was critical for rapid prototyping within the time limit.

B. OWLv2 (by Google)
Role: Text-to-Bounding-Box Detection.

Why I chose it: Traditional object detectors (like YOLO) require predefined classes. OWLv2 is a zero-shot text-conditioned detector, meaning the user can type any word (e.g., "chocolate", "car") and the model will find it dynamically.

Engineering Choice: I implemented a dynamic thresholding and fallback mechanism. If OWLv2 fails to find an exact match for the text query, the system gracefully defaults to a localized fallback region rather than crashing the application.

C. Segment Anything Model - SAM (by Meta)
Role: Bounding-Box-to-Mask Generation.

Why I chose it: While Stable Diffusion can do rough inpainting, SAM guarantees pixel-perfect silhouettes. Feeding OWLv2's bounding box into SAM isolates the subject immaculately, preventing "bleed" and ensuring the generated VFX do not overwrite the foreground subjects.

D. Stable Diffusion Inpainting (RunwayML)
Role: Generative Scene Replacement.

Why I chose it: Standard Stable Diffusion generates entirely new images. The Inpainting variant is specifically trained to look at the unmasked areas of an image to infer lighting, shadows, and depth. This naturally handles the "Lighting Matching" requirement of the assessment, ensuring the new elements (like a sci-fi sky) blend realistically with the original terrain.

E. Gradio
Role: Interactive Web User Interface.

Why I chose it: Gradio is the industry standard for wrapping Python machine learning pipelines into shareable web applications. It allowed me to instantly visualize intermediate steps (like the SAM mask) alongside the final output, providing a clean, professional testing environment directly from Google Colab.

Future Improvements:
This models is running very slow. Future improvements would include better optimization for faster working for this model.

