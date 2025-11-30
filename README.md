# YOLO-COCO-Visual-Search-Streamlit
A Streamlit-based web application that enables intelligent image search using YOLOv11 object detection. Search through your image collections by specifying objects, classes, and count thresholds.
# Features
1. **Smart Image Search** - Find images containing specific objects
2. **Object Detection** - Powered by YOLOv11 for accurate detection
3. **Bounding Box Visualization** - See detected objects with labeled boxes
4. **Metadata Management** - Save and load detection results
5. **Visual Results** - Grid display of matching images
6. **Flexible Filtering** - Search with AND/OR logic and count thresholds
7. **Persistent Storage** - Reuse previously processed metadata
# Project Structure
```
Yolo_11/
├── app.py                          # Main Streamlit application
├── configs/
│   └── default.yaml                # Model configuration
├── src/
│   ├── __init__.py
│   ├── config.py                   # Configuration loader
│   ├── inference.py                # YOLOv11 inference engine
│   └── utils.py                    # Utility functions
├── data/
│   ├── raw/                        # Input images
│   └── processed/                  # Generated metadata
└── test/
    └── streamlit_basics.py         # Streamlit examples
```
# Installation

# Prerequisites
1. Python 3.8+
2. CUDA-compatible GPU (recommended)
3. Anaconda/Miniconda (optional)
# Setup
1. Clone or download the project

2. Install dependencies
   ```
   pip install streamlit ultralytics pillow pyyaml
   ```
3. Download YOLO model weights (optional - auto-downloads on first run)
   ```
   # The app uses yolo11m.pt by default
   # It will auto-download if not present
   ```
# Usage

# Starting the Application
```
streamlit run app.py
```
The application will open in your browser at http://localhost:8501

# Workflow
# Option 1: Process New Images
1. Select "Process new images"
2. Enter paths:
      1. Image directory: C:\Users\admin\Desktop\mini project\Yolo_11\data\raw\coco-val-2017-500
      2. Model weights: yolo11m.pt (default)
3. Click "Start Inference"
# Sample Input:
```
Image directory: data/raw/coco-val-2017-500
Model weights: yolo11m.pt
```
# Sample Output:
```
Processed 500 images. Metadata saved to:
data/processed/coco-val-2017-500/metadata.json
```
# Option 2: Load Existing Metadata
1. Select "Load existing metadata"
2. Enter metadata path:
      C:\Users\admin\Desktop\mini project\Yolo_11\data\processed\coco-val-2017-500\metadata.json
3. Click "Load Metadata"
# Sample Input:
```
Metadata file path: data/processed/coco-val-2017-500/metadata.json
```
# Sample Output:
```
Successfully loaded metadata for 500 images.
```
# Search Examples

# Example 1: Simple Search (OR Logic)
# Input:
    1. Search mode: Any of selected classes (OR)
    2. Classes: apple, person
    3. Thresholds: None, None
Result: Returns all images containing either apples OR persons (or both)
```
Found 150 images
```
# Example 2: Exact Count Search
# Input:
    1. Search mode: Any of selected classes (OR)
    2. Classes: person
    3. Max count for person: 1
Result: Returns images with exactly 1 person
```
Found 45 images
```
# Example 3: Combined Search (AND Logic)
# Input:
    1. Search mode: All selected classes (AND)
    2. Classes: person, car
    3. Max count for person: 2
    4. Max count for car: None
Result: Returns images containing BOTH persons (max 2) AND cars (any count)
```
Found 23 images
```
# Configuration
Edit **configs/default.yaml** to customize:
```
model:
  conf_threshold: 0.25    # Detection confidence threshold (0-1)

data:
  image_extension:        # Supported image formats
    - .jpg
    - .jpeg
    - .png
```
# Metadata Format
Generated metadata.json structure:
```
[
  {
    "image_path": "C:/path/to/image.jpg",
    "detections": [
      {
        "class": "person",
        "confidence": 0.8548,
        "bbox": [111.91, 296.87, 353.71, ...],
        "count": 1
      }
    ],
    "total_objects": 3,
    "unique_class": ["person", "car"],
    "class_counts": {
      "person": 1,
      "car": 2
    }
  }
]
```
# Sample Outputs
**Note**: To add screenshots to this documentation, capture images of the application interface and save them in **docs/images/** directory. See **docs/images/README.md** for detailed instructions.
# Application Interface

# Main Screen

<img width="1797" height="851" alt="image-1" src="https://github.com/user-attachments/assets/54e6c5f0-2c12-431f-8e8f-145abe35ff1c" />

The main application interface showing the two options: Process new images or Load existing metadata
# Processing Images

<img width="1840" height="706" alt="image-2" src="https://github.com/user-attachments/assets/25de89eb-c74e-4563-a56d-49acee624610" />

Object detection in progress on a batch of images
# Search Interface

<img width="1801" height="697" alt="image-3" src="https://github.com/user-attachments/assets/c4fb6201-3999-4d5c-b63b-7f13d1586ec4" />

Search configuration with class selection and count thresholds
# Search Results

<img width="1827" height="906" alt="image-4" src="https://github.com/user-attachments/assets/e67bfe98-accc-4a45-ad19-9eaed09c8f12" />

Grid display of images matching the search criteria with detected objects
# Search Results Display
When you search for images, the app displays:

    1. Success message: Found 100 matching images
    2. Bounding Box Toggle: Checkbox to show/hide bounding boxes
    3. Image grid: 3 columns showing:
          Image preview with bounding boxes (if enabled)
          Color-coded boxes around detected objects
          Labels with class name and confidence score
          Filename
          Detected objects with counts
          
# Bounding Box Features:
1. **Color-coded**: Each object class gets a unique color from a 10-color palette
2. **Labels**: Shows class name and confidence score (e.g., "person 0.85")
3. **Toggle**: Enable/disable visualization with the checkbox
4. **Multi-object**: Displays all detected objects with individual boxes
5. **Automatic**: Boxes are drawn automatically when search results are displayed
6. **Confidence threshold**: Only objects above the configured threshold are shown
Example display:
```
[Image 1]                [Image 2]                [Image 3]
[Red box: person 0.85]   [Red box: person 0.92]   [Blue box: apple 0.78]
[Green box: car 0.76]    [Orange: bicycle 0.81]   [Red box: person 0.84]
000000056381.jpg         000000123456.jpg         000000789012.jpg

Detected:                Detected:                Detected:
• person: 1              • person: 2              • apple: 5
• car: 2                 • bicycle: 1             • person: 1
```
# Search Logic

# OR Mode (Any of selected classes)
1. Returns images containing at least one of the selected classes
     Example: Search for apple OR banana → returns images with either or both
# AND Mode (All selected classes)
1. Returns images containing all selected classes
    Example: Search for person AND car → returns only images with both
# Count Thresholds
    None: Any count (≥1)
    1: Exactly 1 object
    2: 1 or 2 objects
    5: 1 to 5 objects
# Troubleshooting

# Issue: ModuleNotFoundError
**Solution**: Ensure all dependencies are installed:
```
pip install streamlit ultralytics pillow pyyaml
```

# Issue: CUDA out of memory
**Solution**: Use CPU mode by modifying src/inference.py:
```
device='cpu'  # Change from 'cuda'
```

# Issue: Image not displaying
**Solution**: Verify image paths are absolute and files exist at the specified locations

# Performance Tips
1. **Use GPU**: Significantly faster than CPU for inference
2. **Batch processing**: Process images in batches for large datasets
3. **Cache metadata**: Load existing metadata instead of re-processing
4. **Adjust threshold**: Higher conf_threshold = fewer detections but higher accuracy

# Supported Object Classes
YOLOv11 detects 80 COCO classes including:

1. People: person
2. Vehicles: car, truck, bus, bicycle, motorcycle
3. Animals: dog, cat, horse, elephant, bear
4. Food: apple, banana, sandwich, pizza, donut
5. Electronics: laptop, cell phone, tv, keyboard
And 60+ more...

# License
This project uses:

    1. YOLOv11 by Ultralytics (AGPL-3.0)
    2. Streamlit (Apache 2.0)

