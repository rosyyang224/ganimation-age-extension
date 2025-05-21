# GANimation-Age-Extension

This project extends the [GANimation](https://github.com/albertpumarola/GANimation) framework to incorporate age progression and regression alongside Action Unit (AU)-driven facial animation. It enables realistic and identity-preserving aging while maintaining dynamic facial expressions.

> Developed for the 10-423/623 Generative AI Course @ Carnegie Mellon University  
> By Brendon Fang, Max Fang & Rosemary Yang

---

## Project Overview

Traditional GAN-based facial animation focuses on expressions. Our work introduces a novel age modulation network that allows simultaneous control over emotion (via AUs) and age transformation (via scalar conditioning), all while preserving facial structure and identity.

### Key Contributions:
- **Age-Expression Co-Control:** Jointly manipulate facial expressions and age
- **Age Modulation Network:** Separate module to encode age features
- **VGG-based Perceptual Loss:** For identity and high-level consistency
- **DisPatchGAN Discriminator:** For patch-based realism (e.g., wrinkles)
- Trained on **FFHQ** and **CACD** datasets

---
## Architecture

Our architecture introduces an age-aware conditioning mechanism, enhanced loss functions, and a discriminator capable of multi-attribute supervision.

#### Inputs and Conditioning

The model takes three inputs: a high-resolution input image, an Action Unit (AU) vector that encodes facial expressions, and a scalar age value indicating the target age. To enable disentangled conditioning, the age scalar is passed through a lightweight Age Modulation Network that transforms it into a 128-dimensional style embedding. This embedding is then concatenated with the AU vector to form a unified condition vector. After spatial expansion, this condition vector is concatenated with the input image and passed to the generator.

#### Generator

The generator produces two outputs: a synthesized image and an attention mask. The attention mask focuses the transformation on facial regions relevant to the target AUs and age. To guide training, the generator is optimized using a combination of loss functions:
- **AU Consistency Loss** (mean squared error between target and predicted AUs)
- **Age Consistency Loss** (MSE between predicted and target age)
- **Cycle Consistency Loss** and **Mask Smoothness Loss** (from original GANimation)
- **VGG Perceptual Loss** (for high-level structural fidelity)
- **GAN Loss** (adversarial training)

These losses ensure the generated face accurately reflects the conditioning inputs while maintaining identity and coherence.

#### Discriminator Enhancements

The discriminator was adapted into a multi-task architecture that simultaneously evaluates:
- Whether the image is real or fake
- Whether the facial expression matches the AU vector
- Whether the predicted age aligns with the target scalar

To improve spatial realism, we integrated **DisPatchGAN**, a patch-based variant of the discriminator that evaluates local image regions for high-frequency details like wrinkles and texture. A toggle allows switching between standard and patch-based discriminators for experimentation.

#### Perceptual Supervision

A pre-trained **VGG network** is used to compute a perceptual loss by comparing the high-level feature representations of real and synthesized images. This encourages the generator to preserve identity and facial structure across transformations.

#### Training Setup

Training was conducted in two stages:
- Stage 1: 10 epochs on the FFHQ dataset with batch size 8 and learning rate 1e-4
- Stage 2: 15 fine-tuning epochs on 1024×1024 images with batch size 2 and learning rate 1e-5

The model was trained on an AWS EC2 GPU instance, totaling ~70 hours. Validation used qualitative visual assessment.

#### Observed Limitations

Despite architectural improvements, some aging effects were applied uniformly across the face, lacking spatial specificity. The small training subset (1,000 images) limited generalization, and combining AU and age sometimes led to visual inconsistencies. Additionally, the attention mechanism was not always effective at prioritizing age-relevant regions.

---

## Results Summary

The model produces coherent age transformations while preserving facial identity and expressions. Key takeaways from our experiments:

- Wrinkle patterns and skin tone changes were successfully generated, with consistency across the age range (25–69).
- Facial expressions remained stable under AU conditioning, even when combined with age transformations.
- Using DisPatchGAN and VGG loss led to notable improvements in spatial detail and facial realism.

However, limitations included uniform wrinkle application and some image blurriness due to training on a small data subset (1,000 images). Aging effects could be further localized with better supervision and larger datasets.

---
## Authors

- [@blfang](https://github.com/blfang)
- [@msfang](https://github.com/msfang)
- [@rosyyang224](https://github.com/rosyyang224)

---

## Acknowledgments

This project builds on [GANimation](https://github.com/albertpumarola/GANimation) by [Pumarola et al. (2018)] and draws dataset inspiration from [FFHQ](https://paperswithcode.com/dataset/ffhq) and [CACD](https://paperswithcode.com/dataset/cacd). Thank you to the 10-423/623 teaching team for guidance and feedback.

---

## License

MIT License — open to collaboration and improvement.  
If you use or build on this, please cite!
