---
layout: post
title: Unlock the power of AI on your laptop - an introduction to Hugging Face
date: 2023-06-06
description: a short tutorial given at the HCIL symposium 2023
tag: machine-learning
giscus_comments: false
related_posts: false
published: true
---

[![HitCount](https://hits.dwyl.com/tracyyxchen/https://tracyyxchengithubio/blog/2023/huggingface/.svg?style=flat-square)](http://hits.dwyl.com/tracyyxchen/https://tracyyxchengithubio/blog/2023/huggingface/)

During the [40th HCIL Symposium](https://hcil.umd.edu/hcil-symposium-2023/){:target="_blank" rel="noopener"}, I gave a tutorial on Hugging Face, a powerful machine learning (ML) platform that offers user-friendly APIs. In addition, incorporating UMD themes made the experience more enjoyable, which served as a fun backdrop.

As someone who has used Hugging Face for my research, I was impressed by its user-friendliness and the scope of its ML APIs. The tutorial targeted HCI/UX researchers and hobbyists with a limited computing budget but are interested in exploring new features with potential ML capabilities or in making inferences with pre-trained models.

**Outline:**
- HuggingChat
- Spaces: online ML applications
- Transformers, a Python package

## 1. HuggingChat

HuggingChat is a chatbot built on open-source models (currently, it’s built on OpenAssistant LLaMa 30B SFT 6). I also compared its output with closed-source models (OpenAI’s GPT-4 and Google’s Bard)

**Prompt:** *suggest some themed lunch table ideas for an HCI symposium*

**HuggingChat:**
{% include figure.html path="assets/img/hugging-face/fig1.png" class="img-fluid rounded z-depth-1" %}
I found those named ideas delightful.

**GPT-4:**
{% include figure.html path="assets/img/hugging-face/fig2.png" class="img-fluid rounded z-depth-1" %}
GPT-4 suggests broad discussion topics along with table decorations. However, it had its quirks — point 5, “make sure the table is accessible to everyone,” seemed more in line with on-screen table designs rather than physical tables!


**Google Bard:**
{% include figure.html path="assets/img/hugging-face/fig3.png" class="img-fluid rounded z-depth-1" %}
I’m surprised that Bard does not return any discussion points but focuses on decorations and food exclusively, offering suggestions right down to specifics like grilled cheese sandwiches 😅

**Humans**

On the human side of things, the HCIL symposium has three themed lunch tables this year:

- Robotics Education
- Teams/creativity
- LLMs and the future of AI as a writing assistant

It seems LLMs are too humble to suggest themselves as lunch table ideas! 🤷‍♀

## 2. Hugging Face Spaces
Next, we looked at Spaces, online demos of machine-learning applications.

### Object detection ([space link](https://huggingface.co/spaces/eddie5389/Object-Detection-With-DETR-and-YOLOS){:target="_blank" rel="noopener"})
{% include figure.html path="assets/img/hugging-face/fig6.png" class="img-fluid rounded z-depth-1" %}

For instance, I demonstrated an object detection Space by uploading a photo taken at the [Yahentamitsi dining hall](https://dining.umd.edu/yahentamitsi-virtual-tour){:target="_blank" rel="noopener"}. The model could identify objects within the photo, such as a backpack, pizza, and persons. (Note that it can’t return labels that are not in the training dataset, here is a [list of supported labels](https://gist.github.com/AruniRC/7b3dadd004da04c80198557db5da4bda){:target="_blank" rel="noopener"} in this Space)

### CLIP Interrogator ([space link](https://huggingface.co/spaces/pharma/CLIP-Interrogator){:target="_blank" rel="noopener"})
Another intriguing Space is the CLIP Interrogator, which generates potential text prompts based on an image. Let’s see what text prompts will be suggested for a photo of our [Testudo](https://goo.gl/maps/khGiycsZTLCFon4T8){:target="_blank" rel="noopener"} during final exam weeks😏
{% include figure.html path="assets/img/hugging-face/fig7.png" class="img-fluid rounded z-depth-1" %}

If I use the output verbatim as input for Stable Diffusion, one of the generated images is: 😆

{% include figure.html path="assets/img/hugging-face/fig4.png" class="img-fluid rounded z-depth-1" %}


## 3. Transformers, a Python package
Hugging Face offers a variety of Python packages, including Transformers, which can be used for numerous tasks, such as Natural Language Processing (NLP), Computer Vision (CV), and audio processing. I demonstrated how to use it for sentiment analysis, language modeling, zero-shot object detection, etc.

GitHub Repo: [huggingFaceTutorial](https://github.com/TracyYXChen/huggingFaceTutorial){:target="_blank" rel="noopener"}

First, download transformers:
```
pip install transformers
```
Import transformers:
```
from transformers import pipeline
```
### Sentiment Analysis
In sentiment analysis, the model predicts the emotional tone of the text: positive, negative, or neutral.

```
sentence = "We’ve done a lot since the lab launched in 1983 and we’re looking forward to what faculty and students contribute in the next 40 years"
classifier = pipeline(task="sentiment-analysis")
preds = classifier(sentence)
preds = [{"score": round(pred["score"], 4), "label": pred["label"]} for pred in preds]
print(preds)
```

output:
```
[{'score': 0.9998, 'label': 'POSITIVE'}]
```

### Language Modeling
Language modeling predicts masked words in sentences.
```
text = "2023 Commencement Honors the ‘Terrapin Grit’ of <mask> of UMD Graduates"
fill_mask = pipeline(task="fill-mask")
preds = fill_mask(text, top_k=3)
```

I asked the audience to guess the masked word, but no one had guessed it right. The ground truth is “thousands,” which is also the top-1 prediction returned by the transformer.

### Zero-shot Object Detection
Unlike traditional object detectors (e.g., the detector I showed before), zero-shot object detectors can predict labels not found in the training dataset, allowing us to identify custom labels without training a model on a new dataset.

```
import skimage
import numpy as np
from PIL import Image, ImageDraw

# load check-point
checkpoint = "google/owlvit-base-patch32"
detector = pipeline(model=checkpoint, task="zero-shot-object-detection")

# load image
imgPath = "./symposiumPoster.jpeg"
image = Image.open(imgPath).resize((600, 300))
image = Image.fromarray(np.uint8(image)).convert("RGB")

# provide candidate labels
candidate_labels = ["poster", "yellow sofa", "green chair", "blue table cloth", "yellow table cloth"]

predictions = detector(
    image,
    candidate_labels=candidate_labels,
)

# visualize predictions
draw = ImageDraw.Draw(image)

for prediction in predictions:
    box = prediction["box"]
    label = prediction["label"]
    score = prediction["score"]
    xmin, ymin, xmax, ymax = box.values()
    draw.rectangle((xmin, ymin, xmax, ymax), outline="red", width=1)
    draw.text((xmin, ymin), f"{label}: {round(score,2)}", fill="white")
```

output:
{% include figure.html path="assets/img/hugging-face/fig5.png" class="img-fluid rounded z-depth-1" %}

### Discussions
The tutorial ended with participants sharing their planned use cases for Hugging Face. Some exciting applications included food image analysis, survey response text analysis, hate speech classification, and image segmentation. I look forward to seeing more people unlock the potential of ML on their laptops!
