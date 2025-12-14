# AMD_Robotics_Hackathon_2025_[TRI_ROBOTIQUE_FIXE]

## Team Information

**Team:** *Team4, Préhistorique, Ylan Nabti, Yihuan Zhang*

**Summary:** Tri-Robotique Fixe investigates a fundamental data-efficient approach for **selective waste sorting (Glass, Plastic, Paper)** in a **fixed, structured environment**. We leverage the **Hugging Face Act Model** framework, combining vast external visual data (from datasets like Garbage_Classification_YOLO) with a minimal set of custom robotic action demonstrations, aiming to validate a Few-Shot Action Learning methodology for high-purity recycling tasks.

*< Images or video demonstrating your project >*

## Submission Details

### 1. Mission Description
- **Real world application of your mission**

The core mission validates a **Few-Shot Action Learning** approach for robotic sorting, designed to tackle the cost and time barrier associated with collecting extensive robot-specific datasets. This methodology is a scalable blueprint for deploying cost-effective sorting robots in highly structured industrial pre-sorting environments, proving that high-level object recognition can be sourced externally while only local, critical manipulation actions need to be taught.

Our robot autonomously attempts the pipeline in a fixed setup:
1. Identify a target item (Glass, Plastic, or Paper) from a cluttered, stationary pile.
2. Execute a **pre-grasp displacement** for final pose refinement.
3. Grasp the item.
4. Place the item into the assigned, fixed-location bin.

This project represents a meaningful shift toward **integrating off-the-shelf visual models with low-data action learning**.

### 2. Creativity
- **The novel or unique in our approach：**

| Target Category | Item Descriptions |
| :--- | :--- |
| **1. Glass** | **Glass Containers:** Glass Bottles (wine, oil, beverage), Glass Jars (jam, sauces). |
| **2. Plastic** | **All Plastic Packaging:** Plastic bottles, pots, films, tubes, trays, bags. |
| **3. Paper** | **All Cardboard/Paper:** Boxes, newspapers, magazines, office paper. |

---

* **Novelty 1: Few-Shot Action Learning with Large Visual Priors:**
    Our primary technical contribution is the implementation of the **Hugging Face Act Model** framework. 
    1.  **Vision Feature Integration:** We utilize **Garbage_Classification_YOLO** and similar public datasets to train or enhance the Act Model's visual encoder, gaining extensive **general visual knowledge** of our three target categories. This encoder is subsequently **frozen**.
    2.  **Action Fine-Tuning:** The action head is trained exclusively on our small **300-sample custom action dataset**. This strategy tests whether a robot can effectively **learn new actions** (grasping/placing) using **minimal action data** because it already possesses sophisticated **recognition capability** from external sources.

* **Novelty 2: Pre-Grasp Displacement for Robustness in Fixed Systems:**
    The Act Model is trained to output a **pre-grasp displacement** action. In our fixed setup, this serves as a **micro-adjustment mechanism** just before the final grasp, intended to correct minor visual localization errors and improve the robustness of the final picking pose.

* **Structured Environment Focus and Analysis:**
    Our experimental design utilizes a **fixed-point environment** (fixed item starting positions and fixed bin locations). This structured setting isolates the failure mode analysis to the **visual-to-action mapping** itself, providing clear feedback on the limits of the few-shot action learning approach.

### 3. Technical implementations

**Training**

* **Policy:** **ACT (Action Chunking with Transformers) Framework** (Adapted for few-shot learning).
* **Inputs:** Image streams (single-view) + robot joint values.
* **Methodology:** **Frozen visual encoder** (trained on *Garbage_Classification_YOLO*) and fine-tuning only the action decoder on custom data.

**Teleoperation / Dataset capture**

* **Action Dataset:** **~300** high-quality grasp-and-place demonstrations.
* **Purpose:** This dataset provides the critical few-shot learning samples required to connect the generalized visual recognition (from YOLO) to specific robotic end-effector motions.
* *<Image/video of teleoperation or dataset capture>*

**Inference and Evaluation**

* **Setup:** Fixed item placement and fixed bin locations (Highly structured environment).
* **Focus:** Evaluation centers on the **success rate of the predicted grasp pose** and the **number of retry attempts** required to complete the pick-and-place sub-tasks.

**1. Localization Success**
The model reliably identifies the target item's pose due to the strong visual backbone.

**2. Grasp Attempt Analysis**
We analyzed the failure modes where the predicted action from the few-shot data was insufficient, pinpointing the gap between visual certainty and successful physical manipulation. **(Note: The robot did
