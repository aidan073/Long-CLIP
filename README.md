# Long-CLIP
This repository is the official implementation of Long-CLIP

**[Long-CLIP: Unlocking the Long-Text Capability of CLIP](https://arxiv.org/abs/2403.15378)**\
[Beichen Zhang](https://github.com/beichenzbc), [Pan Zhang](https://panzhang0212.github.io/), [Xiaoyi Dong](https://lightdxy.github.io/), [Yuhang Zang](https://yuhangzang.github.io/), [Jiaqi Wang](https://myownskyw7.github.io/)

## 💡 Highlights
- 🔥 **Long Input length** Increase the maximum input length of CLIP from **77** to **248**.
- 🔥 **Strong Performace** Improve the R@5 of long-caption text-image retrieval by **20%** and traditional text-image retrieval by **6%**.
- 🔥 **Plug-in and play** Can be directly applied in **any work** that requires long-text capability.


## 📜 News
🚀 [2024/3/25] Our paper [Long-CLIP: Unlocking the Long-Text Capability of CLIP](https://arxiv.org/abs/2403.15378) is released!

🚀 [2024/3/23] The Inference code and models ([LongCLIP-B](https://huggingface.co/BeichenZhang/LongCLIP-B) and [LongCLIP-L](https://huggingface.co/BeichenZhang/LongCLIP-L)) are released!

## 👨‍💻 Todo
- [ ] Training code for Long-CLIP based on OpenAI-CLIP
- [x] Evaluation code for Long-CLIP
- [x] evaluation code for zero-shot classification and text-image retrieval tasks.
- [x] Usage example of Long-CLIP
- [x] Checkpoints of Long-CLIP


## 🛠️ Usage

### Installation

Our model is based on [CLIP](https://github.com/openai/CLIP), please prepare environment for CLIP.


### how to use

Please first clone our [repo](https://github.com/beichenzbc/Long-CLIP) from github by running the following command.

```shell
git clone https://github.com/beichenzbc/Long-CLIP.git
cd Long-CLIP
```

Then, download the checkpoints of our model [LongCLIP-B](https://huggingface.co/BeichenZhang/LongCLIP-B) and/or [LongCLIP-L](https://huggingface.co/BeichenZhang/LongCLIP-L) and place it under `./checkpoints`

```python
from model import longclip
import torch
from PIL import Image

device = "cuda" if torch.cuda.is_available() else "cpu"
model, preprocess = longclip.load("./checkpoints/longclip-B.pt", device=device)

text = longclip.tokenize(["A man is crossing the street with a red car parked nearby.", "A man is driving a car in an urban scene."]).to(device)
image = preprocess(Image.open("./img/demo.png")).unsqueeze(0).to(device)

with torch.no_grad():
    image_features = model.encode_image(image)
    text_features = model.encode_text(text)
    
    logits_per_image, logits_per_text = model(image, text)
    probs = logits_per_image.softmax(dim=-1).cpu().numpy()

print("Label probs:", probs) # prints: [[0.982  0.01799]]
```

### Evaluation
#### Zero-shot classification

To run zero-shot classification on imagenet dataset, run the following command after preparing the data
```shell
cd eval/classification/imagenet
python imagenet.py
```

Similarly, run the following command for cifar datset
```shell
cd eval/classification/cifar
python cifar10.py               #cifar10
python cifar100.py              #cifar100
```

#### Retrieval
To run text-image retrieval on COCO2017 or Flickr30k, run the following command after preparing the data
```shell
cd eval/retrieval
python coco.py                  #COCO2017
python flickr30k.py             #Flickr30k
```

## ⭐ Demos
### Long-caption text-image retrieval 
<p align="center"> <a>  
<img src="./img/retrieval.png"  width="900" />
</a> </p>

### Plug-and-Play text to image generation 
<p align="center"> <a>  
<img src="./img/generation.png"  width="900" />
</a> </p>

## ✒️ Citation
If you find our work helpful for your research, please consider giving a star ⭐ and citation 📝
```bibtex
@article{zhang2024longclip,
      title={Long-CLIP: Unlocking the Long-Text Capability of CLIP}, 
      author={Beichen Zhang, Pan Zhang, Xiaoyi Dong, Yuhang Zang, Jiaqi Wang},
      year={2024},
      journal={arXiv preprint arXiv:2403.15378},
}
