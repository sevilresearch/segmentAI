# segmentAI

### Dataset and Resources

All RELLIS-3D files and components can be found on the official GitHub repository:  
🔗 [RELLIS-3D GitHub](https://github.com/unmannedlab/RELLIS-3D)

Evaluation benchmarks:
- **HRNet+OCR** metrics: [HRNet Benchmark](https://github.com/unmannedlab/RELLIS-3D/tree/main/benchmarks/HRNet-Semantic-Segmentation-HRNet-OCR)  
- **GSCNN** metrics: [GSCNN Benchmark](https://github.com/unmannedlab/RELLIS-3D/tree/main/benchmarks/GSCNN-master)  
- **DALL·E 3** (used for generative segmentation): [DALL·E 3 by OpenAI](https://openai.com/index/dall-e-3/)
- mIoU benchmarks code are found within the HRNet & GSCNN repository

> ⚠️ **Note:** The free version of DALL·E 3 has a limited number of image generations per day. For consistent and high-quality results, it is recommended to upgrade to GPT Plus.

---

## How to Segment an Image Using DALL·E 3

1. **Upload an image to DALL·E 3.**

2. **Visually identify the most dominant class in the image.**  
   For RELLIS-3D images, the most common classes are typically **sky** or **grass**.

3. **Prompt DALL·E 3 to segment the dominant class.**  
   Use this format:  
   > “Segment the _{class name}_ in _{color}_.”  
   For example:  
   > “Segment the sky in RGB(0, 255, 0).”

   Acceptable color formats include **RGB**, **Hex**, or named colors (e.g., `cyan`, `dark green`). RGB or Hex values are preferred for consistency.

4. **Inspect the output.**  
   - If segmentation includes incorrect areas or merges multiple classes into one color, refine your prompt.  
   - If the result is poor, restart the segmentation process for better accuracy.

5. **After obtaining a satisfactory segmentation, proceed to morphological refinement.**

6. **Apply a first round of morphological operations.**  
   Choose a combination of:
   - **Kernel Size** (`K`)
   - **Structuring Element Shape** (`Circle` or `Square`)
   - **Operation Type** (`Open`, `Close`, `OpenClose`)

   Example prompt:  
   > “Apply a Closing operation using a circular structuring element with a kernel size of 5 to the segmented sky.”

7. **Evaluate the resulting segmentations.**  
   Select the image that produces the highest IoU. There should be a measurable improvement from the original—e.g., sky IoU increasing from `0.8763` to `0.8931`.

8. **Apply a second round of operations on the selected best image.**  
   Available operation combinations:
   - `Closing & Flood Fill`
   - `Flood Fill & Closing`
   - `Flood Fill & Opening`
   - `Imfill`
   - `Opening & Closing`
   - `Opening`
   - `Closing & Flood Fill`
   - `Opening & Flood Fill`

   Kernel sizes typically range from **5 to 60**.  
   Example prompt:  
   > “Perform a Flood Fill & Opening operation with kernel size 5 on the segmented sky region.”

   Repeat this for all morphological types and kernel sizes (24 combinations total), then select the image with the highest IoU.

9. **Repeat the process for each remaining class until the full image is segmented.**

---

> 🔍 **Important:** Smaller visual classes (e.g., bushes, road signs) are more difficult to segment accurately. Extra care or multiple iterations may be needed.
