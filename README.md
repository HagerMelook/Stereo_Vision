# Stereo Vision Lab Report

## Team Members
| Name | ID |
|------|----|
| Hager Ashraf Mohamed Melook | 21011505 |
| Nouran Ashraf Yousef | 21011492 |
| Rana Mohamed Ali Attia | 21010528 |

---

## Lab Objective
Implement and compare **stereo vision algorithms**—Block Matching (BM) and Dynamic Programming (DP)—to compute disparity maps from stereo image pairs.

---

## 1. Block Matching (BM)

**Overview:**  
- Local method that computes disparity by comparing small windows (blocks) in the left image with corresponding blocks in the right image along the same scanline.  
- Disparity is the horizontal shift that **minimizes an error metric** between the blocks.

**Implementation Details:**

1. **Initialization & Preprocessing**  
   - Images loaded and converted to grayscale.  
   - Reflect padding applied to handle window boundaries and maximum disparity shifts.  
   - Ensures a complete \(w \times w\) block is always available around each pixel.

2. **Cost Functions**  
   Two methods to measure similarity between windows:  
   
   **a) Sum of Absolute Differences (SAD)**  
   - Computes absolute difference in pixel intensities and sums over the block.  
   - Efficient and robust to small intensity variations.

   **b) Sum of Squared Differences (SSD)**  
   - Squares intensity differences before summing.  
   - More sensitive to noise, often produces smoother disparity maps.

3. **Disparity Map Computation**  
   - For each pixel \((x,y)\) in the left image, evaluate all possible disparities \(d \in [0, max\_disparity]\).  
   - Compare left window with shifted right window using SAD or SSD.  
   - Assign disparity value that minimizes cost to the disparity map.

**Example Disparity Maps:**  
| Window Size | SAD | SSD |
|-------------|-----|-----|
| W = 1       | ... | ... |
| W = 5       | ... | ... |
| W = 9       | ... | ... |

---

## 2. Dynamic Programming (DP)

**Overview:**  
- Global method processing each scanline using DP.  
- Handles occlusions and ensures **row-by-row optimal alignment**.

**Algorithm Details:**

1. **Pixel Matching Cost Function**  
   - Normalized SSD metric between pixels \(i\) (left) and \(j\) (right).  
   - σ is a normalization parameter.

2. **Dynamic Programming Cost Matrix**  
   - Construct \(N \times N\) DP matrix for each row (\(N\) = image width).  
   - Recurrence relation incorporates:  
     - Diagonal moves → valid pixel match  
     - Vertical moves → occlusion in left image  
     - Horizontal moves → occlusion in right image  
   - Includes skipping/occlusion penalty \(c\).

3. **Backtracking & Disparity Extraction**  
   - Start from bottom-right of DP matrix.  
   - Diagonal moves indicate pixel matches; disparity computed as horizontal shift.  
   - Repeat for all rows to produce the final disparity map.

**Example Disparity Maps:**  
| Original Image | Left Disparity |
|----------------|----------------|
| Left           | ...            |
| Right          | ...            |

---

## Observations

- **Block Matching:**  
  - Simple and computationally efficient.  
  - Window size affects smoothness: small windows → noisy; large windows → smoother but may lose detail.  
  - SAD is faster; SSD produces smoother results.

- **Dynamic Programming:**  
  - Better handles occlusions and enforces global consistency.  
  - Produces more accurate disparity maps in challenging regions (e.g., textureless surfaces, edges).

---

## Conclusion
- **BM** is straightforward, efficient, and suitable for simple stereo images.  
- **DP** provides more accurate and consistent disparity maps, especially in occlusion-prone or low-texture regions.  
- Choice depends on **accuracy vs. computational cost trade-off**.
