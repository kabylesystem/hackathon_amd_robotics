# AMD Robotics Hackathon 2025: Selective Waste Sorting via Few Shot Action Learning (Fixed Setup)

## Team Information

**Team:** Team4, Préhistorique  
**Members:** Ylan Nabti, Kuan-Lun Huang, Yihuan Zhang

**Summary**  
This repository contains our work for the AMD Robotics Hackathon 2025.

Mission 1 covered hardware and software familiarization (Block Pick and Place).  
Mission 2 is our creative project: **Selective Waste Sorting** (Glass, Plastic, Paper) in a **fixed, structured environment**, using a **Few Shot Action Learning** approach built on the **LeRobot ACT policy** with **large external visual priors**.

---

## Demo Videos

### Mission 1: Block Pick and Place
https://images-ext-1.discordapp.net/external/kSjh7-C2JdR-kJGkcu8lDvjkKOvF-wS6Qbh14lXeUq0/https/media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExZmc5bHg0NTNqenk3NnJ1eTFtdWtxazExNmY4b2cxb3h0M3RwZmVpbCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/duJAnwM1e9s6PIisBN/giphy.gif?width=540&height=960

### Mission 2: Waste Sorting Attempts + Failure Analysis
Add your mission 2 video here, ideally showing:
- Successful attempts
- Typical failure modes
- One run with a retry / recovery behavior (if any)/
https://images-ext-1.discordapp.net/external/-ec4hnaqH33t3JD56qdYQ58XKVbf0pZmi4mkQnFOWvU/https/media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExdDA4b2Jpc280aWE0M3F5Y3RiZThvNnI2cG05N2F1d2tmbjJ2a2owZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/CUOBmwFE2Xnih3HSRy/giphy.gif?width=508&height=900
---

## Mission 1

We used the official unified task to complete block pick and place. This helped us quickly familiarize ourselves with:
- LeRobot training and inference pipeline
- SO-101 follower/leader arm system
- Dataset format and evaluation loop

---

## Mission 2: Selective Waste Sorting

Delivery URLs : 
- Dataset: https://huggingface.co/datasets/nalyda/picking_merged)
- Model: https://huggingface.co/docs/lerobot/en/act)

### Mission Description
Real world motivation: selective sorting is expensive because collecting large robot-specific datasets is costly and time-consuming.

Our project validates a **data-efficient blueprint**:
- learn high-level recognition from **public visual datasets**
- learn manipulation from a **small local action dataset**

Target categories:
- **Glass:** bottles, jars
- **Plastic:** bottles, packaging, films, trays, bags
- **Paper:** cardboard, magazines, office paper

In our fixed setup, the robot attempts this pipeline:
1. Identify target item class in a cluttered stationary pile
2. Execute **pre-grasp displacement** (micro-adjustment for pose refinement)
3. Grasp
4. Place into assigned fixed-location bin

This setup isolates failures to the **visual-to-action mapping** rather than environment randomness.

---

## Creativity / Novelty

### 1) Few Shot Action Learning with Large Visual Priors
We combine:
- **External visual data** (ex: Garbage_Classification_YOLO and similar datasets) to build strong recognition features
- **Minimal robot demonstrations** (about 300 action episodes) to learn grasp and place actions

Method:
1. Train or adapt a visual encoder using public garbage datasets
2. Freeze this encoder
3. Fine-tune the ACT action head on our small robot action dataset

Goal: verify whether strong recognition priors reduce the amount of robot action data needed.

### 2) Pre-Grasp Displacement for Robustness
Instead of directly grasping, the policy outputs a **pre-grasp displacement** step.
In a fixed environment, this improves robustness against small localization errors right before grasp execution.

### 3) Structured Environment Focus + Failure Mode Analysis
We intentionally constrain:
- fixed camera placement
- fixed bin locations
- fixed workspace layout

This makes errors easier to attribute to:
- perception mislocalization
- action head generalization limits
- contact dynamics / gripper alignment issues

---

## Technical Implementation

### Training
- **Policy:** ACT (Action Chunking with Transformers) & SmolVLA via LeRobot
- **Inputs:** single-view RGB stream + robot joint values
- **Approach:** frozen visual encoder + fine-tune action decoder on custom demos

### Teleoperation / Dataset Capture
- **Action dataset:** about 300 high-quality grasp-and-place demonstrations
- Purpose: teach the manipulation mapping specific to our robot + environment

### Inference and Evaluation
Evaluation focuses on:
- grasp success rate
- number of retries needed
- failure categories (localization vs contact vs kinematics)

Note: the robot did not reach 100% full-task success, which highlights remaining challenges in few-shot action mapping.

---

## How To Reproduce Our Work

### Prerequisites
- AMD environment prepared per Hackathon instructions (ROCm / GPU setup as applicable)
- conda environment with `lerobot` installed
- SO-101 follower and leader arms connected via USB
- at least one camera connected for recording and inference
  
Recommended tools:
```bash
sudo apt-get update
sudo apt-get install -y v4l-utils
