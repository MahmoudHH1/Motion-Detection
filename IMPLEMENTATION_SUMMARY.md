# Assignment 3 - From-Scratch Implementation Summary

## ✅ COMPLETELY FROM-SCRATCH IMPLEMENTATION

### Final Implementation Status

Both **EstimateBackground** and **DetectMotion** are now implemented **completely from scratch** with:
- ✅ **NO cv2 functions**
- ✅ **NO predefined NumPy functions**
- ✅ Only manual loops, conditionals, and basic arithmetic operations

---

## Function Signatures

### 1. **EstimateBackground**
```python
def EstimateBackground(frames):
    """
    Input: frames (3D numpy array) - shape: (height, width, num_frames)
    Output: background (2D numpy array) - shape: (height, width)
    
    COMPLETELY FROM SCRATCH:
    - NO cv2 functions
    - NO np.median, np.sort, or any NumPy functions
    - Manual median calculation with manual quicksort
    """
```

**Implementation Details:**
- ✅ Manual quicksort implementation (recursive)
- ✅ For each pixel: collect 599 values → sort → find median
- ✅ Processes 789,600 pixels (700 × 1128)
- ✅ Only uses: loops, `if/else`, basic arithmetic, array indexing

---

### 2. **DetectMotion**
```python
def DetectMotion(frames, background_image):
    """
    Input 1: frames (3D numpy array) - shape: (height, width, num_frames)
    Input 2: background_image (2D numpy array) - shape: (height, width)
    Output: motion_frames (3D numpy array) - shape: (height, width, num_frames)
    
    COMPLETELY FROM SCRATCH:
    - NO cv2 functions
    - NO np.abs, np.where, or any NumPy functions
    - Threshold hardcoded to 25
    - Manual absolute difference and comparison
    """
```

**Implementation Details:**
- ✅ Manual absolute difference: `if diff < 0: diff = -diff`
- ✅ Manual threshold: `if diff > 25: motion = 255 else motion = 0`
- ✅ Processes 599 frames × 789,600 pixels = 473,002,400 operations
- ✅ Only uses: loops, `if/else`, basic arithmetic, array indexing

---

## What's Allowed vs NOT Allowed

### ✅ **ALLOWED in Core Functions:**
- Array indexing: `frames[row, col, idx]`
- Array properties: `.shape`, `.copy()`, `.dtype`
- Basic arithmetic: `+`, `-`, `*`, `//`, `/`
- Comparisons: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Loops: `for`, `while`
- Conditionals: `if`, `else`, `elif`
- Type casting: `int()`, `float()`
- List operations: `[], .append()`

### ❌ **NOT ALLOWED in Core Functions:**
- `np.median()` - Implemented manually with sort + midpoint
- `np.sort()` / `np.argsort()` - Implemented manual quicksort
- `np.abs()` - Implemented manually: `if x < 0: x = -x`
- `np.where()` - Implemented manually with if/else
- `np.zeros()` / `np.ones()` - Use `.copy()` instead
- `np.dot()` - Not needed in core functions
- `np.min()` / `np.max()` / `np.sum()` - Not used
- Any other NumPy functions
- Any cv2 functions

---

## Algorithm Implementations

### **Median Calculation (Manual)**
```python
# 1. Collect pixel values across all frames
pixel_values = []
for frame_idx in range(num_frames):
    pixel_values.append(int(frames[row, col, frame_idx]))

# 2. Sort using manual quicksort
def quicksort(arr, low, high):
    if low < high:
        pivot = arr[high]
        i = low - 1
        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        pi = i + 1
        quicksort(arr, low, pi - 1)
        quicksort(arr, pi + 1, high)

quicksort(pixel_values, 0, len(pixel_values) - 1)

# 3. Find median manually
mid = len(pixel_values) // 2
if len(pixel_values) % 2 == 0:
    median = (pixel_values[mid - 1] + pixel_values[mid]) // 2
else:
    median = pixel_values[mid]
```

### **Absolute Difference (Manual)**
```python
# Manual absolute value
diff = current_pixel - background_pixel
if diff < 0:
    diff = -diff  # Manual abs()
```

### **Thresholding (Manual)**
```python
# Manual threshold comparison
THRESHOLD = 25
if diff > THRESHOLD:
    motion_frames[row, col, frame_idx] = 255
else:
    motion_frames[row, col, frame_idx] = 0
```

---

## Test Results ✅

Successfully tested with `video.mp4`:

```
PREPROCESSING: LOADING VIDEO
  - Total frames: 599
  - Resolution: 1128x700
  - FPS: 30.0
✅ Successfully loaded 599 frames

STEP 1: BACKGROUND ESTIMATION (FROM SCRATCH)
  - Computing median background from 599 frames...
  - Using manual median calculation with quicksort...
  - Processing row 100/700 (14.3%)
  - Processing row 200/700 (28.6%)
  - Processing row 300/700 (42.9%)
  - Processing row 400/700 (57.1%)
  - Processing row 500/700 (71.4%)
  - Processing row 600/700 (85.7%)
  - Processing row 700/700 (100.0%)
✅ Background estimation complete!

STEP 2: MOTION DETECTION (FROM SCRATCH)
  - Detecting motion with threshold=25
  - Processing 599 frames of size 1128x700...
  - Using manual operations (from scratch)...
  - Processed 50/599... 100/599... 550/599...
✅ Motion detection complete! Processed 599 frames

OUTPUTS GENERATED:
  ✅ estimated_background.png
  ✅ motion_detection_output.mp4
  ✅ background_visualization.png
  ✅ motion_samples.png
```

---

## Performance Notes

### **EstimateBackground:**
- Pixels to process: 700 × 1128 = 789,600
- Values per pixel: 599 frames
- Sorting complexity: O(n log n) per pixel with quicksort
- Total operations: ~789,600 × 599 × log(599) ≈ 4.2 billion operations
- **Time:** ~10-15 minutes (depends on CPU)

### **DetectMotion:**
- Pixels to process: 700 × 1128 × 599 = 473,002,400
- Operations per pixel: 3 (subtract, abs, compare)
- **Time:** ~5-10 minutes (depends on CPU)

### **Why It's Slow:**
- Pure Python loops (not vectorized)
- No hardware optimization
- No SIMD instructions
- But it's **completely from scratch**!

---

## Directory Structure

```
CV_assign3/
├── assignment3.py                          # Main implementation
├── assignment3_optimized_backup.ipynb      # Jupyter notebook
├── video.mp4                               # Input video
├── estimated_background.png                # Output: background
├── motion_detection_output.mp4             # Output: motion video
├── background_visualization.png            # Output: visualization
├── motion_samples.png                      # Output: samples
└── IMPLEMENTATION_SUMMARY.md               # This file
```

---

## Usage Example

```python
from assignment3 import load_video_as_grayscale, EstimateBackground, DetectMotion

# Step 1: Preprocessing (NumPy allowed here)
grayscale_frames, fps = load_video_as_grayscale('video.mp4')

# Step 2: Background Estimation (NO NumPy functions - from scratch)
background = EstimateBackground(grayscale_frames)

# Step 3: Motion Detection (NO NumPy functions - from scratch)
motion_frames = DetectMotion(grayscale_frames, background)
```

---

## Verification Checklist

✅ **EstimateBackground:**
- [x] Takes 3D array as input
- [x] Returns 2D array as output
- [x] NO cv2 functions used
- [x] NO np.median used → Manual median with quicksort
- [x] NO np.sort used → Manual quicksort implementation
- [x] Only loops, conditionals, basic operations

✅ **DetectMotion:**
- [x] Takes 3D array + 2D array as input
- [x] Returns 3D array as output
- [x] NO cv2 functions used
- [x] NO np.abs used → Manual: `if diff < 0: diff = -diff`
- [x] NO np.where used → Manual: `if diff > threshold: 255 else 0`
- [x] Threshold hardcoded to 25
- [x] Only loops, conditionals, basic operations

---

## Comparison: Before vs After

### Before (Using NumPy):
```python
# EstimateBackground
background = np.median(frames, axis=2).astype(np.uint8)

# DetectMotion
diff = np.abs(gray_frame.astype(np.int16) - background.astype(np.int16))
binary = np.where(diff > THRESHOLD, 255, 0).astype(np.uint8)
```

### After (From Scratch):
```python
# EstimateBackground
for row in range(height):
    for col in range(width):
        pixel_values = [frames[row, col, i] for i in range(num_frames)]
        quicksort(pixel_values, 0, len(pixel_values) - 1)
        median = pixel_values[len(pixel_values) // 2]
        background[row, col] = median

# DetectMotion
for frame_idx in range(num_frames):
    for row in range(height):
        for col in range(width):
            diff = frames[row, col, frame_idx] - background[row, col]
            if diff < 0:
                diff = -diff
            motion_frames[row, col, frame_idx] = 255 if diff > 25 else 0
```

---

## Key Achievements

✅ **Completely From Scratch:** No predefined NumPy functions in core algorithms  
✅ **Manual Median:** Implemented with manual quicksort  
✅ **Manual Absolute Value:** Simple conditional check  
✅ **Manual Thresholding:** Simple if/else statement  
✅ **Successfully Tested:** Processes 599 frames at 1128x700 resolution  
✅ **Clean Code:** Well-documented and readable  
✅ **Educational Value:** Shows exactly how algorithms work under the hood  

---

**Status: ✅ COMPLETE - Fully From-Scratch Implementation**

Both functions contain **ZERO predefined NumPy functions** and **ZERO cv2 functions**.
Everything is implemented manually using only loops, conditionals, and basic arithmetic.
