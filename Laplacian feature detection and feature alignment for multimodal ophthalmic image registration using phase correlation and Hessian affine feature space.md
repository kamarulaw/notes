## Laplacian feature detection and feature alignment for multimodal ophthalmic image registration using phase correlation and Hessian affine feature space

### Abstract
- Paper proposes a novel technique that overcomes the challenges of registering ophthalmic images using Laplacian features, Hessian affine feature space, and phase correlation
- Paper makes use of a novel qualitative measure, the mean registration error.  And it showed that the proposed approach is significantly better than the mix-and-match benchmark approach, as well as a cutting edge image registration technique

### Introduction
- Despite the importance of comparison between modalities, image registration between the multiple imaging modalities remains an unsolved challenge
- Many approaches to register multimodal ophthalmic images can be found in the literature; however, their core computational and algorithmic components rest on traditional image reg approaches, as well as on spatial data analysis and image processing tools
- These approaches may be divided into three groups: feature-based intensity-based, and hybrid approaches
- The literature review and current practices in ophthalmology suggest that extensive research in multimodal ophthalmic image registration is still required, since no technique yet exists that can register a large number of image modalities with high accuracy
- Focus of this paper is on parametric models since they enable learning strategies that are essential for developing machine learning models that can learn continuously with new modalities and disease signatures
- Goal of paper is to present a new computational framework that uses multimodal ophthalmic images, transforms them through a sequence of image processing and computational techniques with pairwise operations, and outputs registered images 
- In contrast to the traditional spatial domain techniques, which in general rely on key points detection and the bifurcation properties of blood vessels
- Main contribution is a computational approach that systematically integrates both intensity-based and feature-based approaches by deploying various parameterized image processing techniques, feature detection and alignment approaches
- In summary, this work provides a pairwise multimodal ophthalmic image registration technique that is performed and evaluated using three imaging modalities; color fungus photography, BAF SLO, and NR SLO

### Motivation
- Significant care must be taken when ML or computational methods are developed to perform multimodal ophthalmic image registration
- For example, deformations, such as scaling or rotation are very common in multimodal ophthalmic images
- The benchmark approach that is assembled and presented in this paper uses the SOA intensity-based and feature-based image registration techniques - SURF, KAZE, and FREAK
