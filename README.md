Motion Analysis with Optical Flow
A computer vision project exploring how motion can be detected, visualised, and tracked in video using dense optical flow. Built independently as part of a structured self-study track in deep learning and computer vision.

What This Project Does
This notebook takes a real video , extracts every consecutive pair of frames, and computes the motion that occurred between them in 33 milliseconds(optical array). That motion is then visualised as colour (HSV) direction becomes hue, speed becomes brightness, and stillness becomes black.
The result is two outputs:

A side-by-side video showing the original footage on the left and the live motion map on the right
A motion magnitude graph showing how much movement occurred at every moment across the full video


Output
The output video plays the original footage alongside a real-time optical flow overlay. Regions of fast movement appear as bright coloured clusters. Regions with no movement stay black. The colours encode direction — a pixel moving right gets a different hue than a pixel moving down.
When I ran this on a video of my own hand movements, the flow map produced exact replicas of every motion — bright clusters wherever my hand moved, black screen everywhere that stayed still.
The motion graph plots mean flow magnitude against time in seconds. Spikes above the red baseline correspond to fast movements. Valleys below it correspond to stillness between movements.

Technical Stack

Python
OpenCV (cv2) — frame extraction, optical flow, video writing
NumPy — array operations, HSV construction
Matplotlib — motion magnitude visualisation
Google Colab — GPU-accelerated execution


How It Works
1. Frame Extraction
A video is a sequence of images played at 30 frames per second. Each consecutive pair of frames is separated by 33 milliseconds. This notebook extracts those pairs one at a time for analysis.
2. Grayscale Conversion
Optical flow operates on brightness patterns, not colour. Each frame is converted from BGR to grayscale before comparison. This reduces computation and makes the algorithm more robust to lighting changes.
3. Dense Optical Flow — Farneback Algorithm
For every pixel in the frame, the algorithm computes a motion vector: how far that pixel moved horizontally (x) and vertically (y) between the two frames. The result is an array of shape (height, width, 2) — one motion vector per pixel.
The algorithm uses an image pyramid: it first processes a compressed version of the frame to detect large motions, then progressively refines at higher resolutions to capture smaller, precise movements.
4. HSV Visualisation
The raw x and y motion values are converted into magnitude and angle using cv2.cartToPolar. These are then mapped onto an HSV colour space:
Hue        →  direction of motion (angle)
Saturation →  always 255 (fully vivid)
Value      →  speed of motion (magnitude)
Pixels with zero motion get zero brightness — they appear black. Pixels moving fast get bright colours. The direction they moved determines which colour.
5. Motion Timeline
Across all 646 frame pairs in the test video, the mean motion magnitude is computed per frame and stored. Plotting this against time in seconds produces a complete motion timeline — a graph that shows exactly when in the video movement was fast, slow, or absent.

Key Results
MetricValueVideo duration21.5 secondsTotal frames647Frame pairs processed646Max motion magnitude5.91Mean motion magnitude1.38Output resolution2160 × 1920

Concepts Learned

How video is structured as a sequence of frames at a fixed frame rate
Why grayscale conversion is used before motion analysis
How the Farneback pyramid algorithm detects motion at multiple scales
How motion vectors (x, y per pixel) are converted into magnitude and direction
How HSV colour mapping encodes two-dimensional motion data into a single interpretable image
How mean magnitude over time produces a motion timeline


Relevance to Clinical Video Analysis
Motion detection from video is the foundational layer of systems that analyse human movement automatically. The same pipeline used here — frame extraction, grayscale conversion, optical flow, magnitude tracking — underlies clinical applications such as detecting and classifying involuntary movement patterns in neurological conditions.
The motion timeline graph produced by this notebook has direct structural similarity to what a movement detection system would monitor: a stable baseline with sudden spikes indicating rapid, localised motion events.
Future extensions of this project include pose estimation for body-specific motion tracking and region-of-interest optical flow targeting specific body parts.

Repository Structure
motion-analysis/
1.optical_flow.ipynb        # Main notebook
2.optical_flow_output.mp4   # Side-by-side output video
3.README.md

Author
Isaac Mutuma — Computer Science and Engineering, Pusan National University
GitHub: github.com/isaacmutuma
