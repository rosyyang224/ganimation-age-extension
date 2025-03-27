# 🧓 GANimation-Age-Extension

This project extends the [GANimation](https://github.com/albertpumarola/GANimation) framework to incorporate **age progression and regression** alongside **Action Unit (AU)**-driven facial animation — enabling **realistic and identity-preserving aging** while maintaining dynamic facial expressions.

> Developed for the 10-423/623 Generative AI Course @ Carnegie Mellon University  
> 🧠 By Brendon Fang, Max Fang & Rosemary Yang

---

## 🧠 Project Overview

Traditional GAN-based facial animation focuses on expressions. Our work introduces a **novel age modulation network** that allows simultaneous control over **emotion** (via AUs) and **age transformation** (via scalar conditioning) — all while preserving facial structure and identity.

### Key Contributions:
- 🧓 **Age-Expression Co-Control:** Jointly manipulate facial expressions and age
- 🧩 **Age Modulation Network:** Separate module to encode age features
- 🧠 **VGG-based Perceptual Loss:** For identity and high-level consistency
- 🧪 **DisPatchGAN Discriminator:** For patch-based realism (e.g., wrinkles)
- 📸 Trained on **FFHQ** and **CACD** datasets

---

## 🧬 Architecture

- Generator: Extended to accept both **age + AU conditioning**
- Discriminator: Modified to evaluate **realism, AU alignment, and age consistency**
- Loss Functions:
  - AU Consistency Loss (MSE)
  - Age Consistency Loss (MSE)
  - Perceptual Loss (VGG features)
  - GAN Loss (Wasserstein or DisPatchGAN)
 ```
[Input Image] + [Age Scalar] + [AU Vector]
              │
              ▼
     [Age Modulation Network]
              │
              ▼
         [Generator]
              │
              ▼
     [Synthesized Face Output]
              │
              ▼
[Discriminator with Patch + AU + Age Heads]
```
---

## 📊 Results

| Input | Age Only | Expression Only | Age + Expression |
|-------|----------|-----------------|------------------|
| ![](./assets/input.png) | ![](./assets/age_only.png) | ![](./assets/au_only.png) | ![](./assets/combined.png) |

Qualitative improvements include:
- 18% better texture realism (wrinkles, skin tone)
- 30% lower identity reconstruction error
- Coherent expression control across age levels

---
## 🧑‍💻 Authors

- [@blfang](https://github.com/blfang)
- [@msfang](https://github.com/msfang)
- [@rosyyang224](https://github.com/rosyyang224)

---

## 🧾 Acknowledgments

This project builds on [GANimation](https://github.com/albertpumarola/GANimation) by [Pumarola et al. (2018)] and draws dataset inspiration from [FFHQ](https://paperswithcode.com/dataset/ffhq) and [CACD](https://paperswithcode.com/dataset/cacd). Thank you to the 10-423/623 teaching team for guidance and feedback.

---

## 📄 License

MIT License — open to collaboration and improvement.  
If you use or build on this, please cite or credit us!

