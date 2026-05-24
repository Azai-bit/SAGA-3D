
<!-- _class: lead -->

# SAGA-3D

## Semantic-Aligned Geometric Augmentation for Maritime 3D Detection


> Focus: <span class="emphasis"><strong>robust camera-LiDAR 3D perception for USVs</strong></span>

---

# 3D Perception: Road vs. Sea

<div class="columns">

<div>

## Autonomous Driving

<div>

![center w:430](presentation/assets/kitti2.png)

</div>

- Mature benchmarks: KITTI, nuScenes, Waymo.
- Strong detectors are now well established.

</div>

<div>

## Maritime Autonomy

<div>

![center w:880](presentation/assets/sea.png)

</div>

- Maritime 3D perception is still emerging.
- Waves, reflections, and platform motion dominate.

</div>

</div>

<span class="emphasis"><strong>Gap: mature road-side 3D perception cannot be directly transferred to the sea.</strong></span>

---

# Misalignment Breaks Fusion

<div class="columns-">
<div>

![center w:780](presentation/assets/extracted-000.png)

</div>

<div class="columns">
<div>

## Sensor-Side Disturbances

- Calibration errors
- Mechanical vibration
- Temperature / structure drift
- Wave-induced motion

</div>

<div>

## Environment-Side Degradation

- Reflection and illumination changes
- Occlusion, fog, distant targets
- Sparse LiDAR returns
- Unstable visual structure

</div>

</div>

</div>

<!-- <span class="emphasis"><strong>Key problem: misaligned fusion turns semantic cues into noise.</strong></span> -->

---

# Semantic Consistency Formulation

<div class="columns-12">

<div>

## Inputs
<div>

![center w:720](presentation/assets/input.png)

</div>

</div>

<div>

## Projection and Sampling
<div>

![center w:420](presentation/assets/projection.png)

</div>



SAGA-3D refines $\hat{T}$ by maximizing semantic agreement:

$$
\hat{T}=\arg\max_T \sum_{i=1}^{N} S(f_i)
$$

</div>

</div>

<span class="emphasis"><strong>Reformulation: alignment as semantic consistency optimization.</strong></span>

---


# SAGA-3D Method Overview

<div class="columns-37">

<div>

## Two Semantic Branches

- Build comparable semantic features for image and LiDAR.
- Use stable projections to teach LiDAR points visual semantics.
- Refine camera-LiDAR alignment before 3D detection.


</div>

<div>

![center w:820](presentation/assets/overview.png)

</div>

</div>

> RGB and LiDAR are encoded into semantic features first, then aligned before 3D detection.

---

# LiDAR Semantic MLP

<div>


![center w:1000](presentation/assets/mlp.png)

</div>


---

# Confidence-Aware Optimization


![center w:1000](presentation/assets/loop.png)



---

# Experiment Setup

<div class="columns">

<div>

## Dataset Overview

- SeePerSea benchmark
- In-water object scenes
- Maritime multi-sensor data
- Used for BEV and 3D detection

## Evaluation

- **TED-S**: LiDAR-only
- **TED-S + NSA**: naive semantics
- **SAGA-3D**: adaptive alignment
- IoU thresholds: 0.5 / 0.7
- Same protocol; RTX 3090 GPU and i9-14900 CPU.

</div>

<div>

<div>

![center w:560](presentation/assets/sensors.png)

  > Image source: SeePerSea dataset paper / website, Jeong et al., IEEE T-FR 2025, 
  DOI: 10.1109/TFR.2025.3602937.

</div>



</div>

</div>




---

# Quantitative Results
<span class="emphasis"><strong>semantics without alignment can hurt BEV.</strong></span>
<table class="results-table">
  <thead>
    <tr>
      <th>Method</th>
      <th>Modality</th>
      <th>BEV@0.5</th>
      <th>BEV@0.7</th>
      <th>3D@0.5</th>
      <th>3D@0.7</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>TED-S</td>
      <td>L</td>
      <td>65.16</td>
      <td>64.97</td>
      <td>51.84</td>
      <td>49.81</td>
    </tr>
    <tr>
      <td>TED-S + NSA</td>
      <td>L+C</td>
      <td>64.54</td>
      <td>62.23</td>
      <td>55.88</td>
      <td>53.63</td>
    </tr>
    <tr>
      <td><strong>SAGA-3D</strong></td>
      <td>L+C</td>
      <td><strong>77.28</strong></td>
      <td><strong>74.81</strong></td>
      <td><strong>68.06</strong></td>
      <td><strong>65.81</strong></td>
    </tr>
  </tbody>
</table>

<div class="result-notes">

<div>

<span class="emphasis"><strong>+12.12 BEV mAP@0.5 over TED-S.</strong></span>

</div>

<div>

<span class="emphasis"><strong>+16.22 3D mAP@0.5 over TED-S.</strong></span>

</div>

<div>

<span class="emphasis"><strong>NSA shows semantics alone are insufficient.</strong></span>

</div>

</div>



---

# Qualitative Results
<span class="emphasis"><strong>Better alignment leads to tighter 3D boxes.</strong></span>
<div class="columns">

<div class="center-text">

![center h:450](presentation/assets/extracted-004.png)

baseline left; SAGA-3D right.

</div>

<div class="center-text">

![center h:430](presentation/assets/extracted-006.png)

baseline top; SAGA-3D bottom.

</div>

</div>



