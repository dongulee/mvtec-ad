# MVTec-AD Dataset
[homepage](https://www.mvtec.com/company/research/datasets/mvtec-ad/)

# Introduction
[ipython notebook](intro_mvtec-ad.ipynb)

## preview ipynb
---
# Introduction to the MVTec-AD Dataset
## Required packages
* python=3.6
* matplotlib
* ipywidgets - an interactive ui for a demo. install with following commands.
```bash
pip install ipywidgets
jupyter nbextension enable --py widgetsnbextension
```
and restart the jupyter notebook

## File structure
1. There are 15 categories
2. Each category has "ground_truth", "test", "train" data
3. The "train" data has only normal data ("good" directory)
4. The "test" data has types of defects as sub-dir name e.g. ("broken_large", "broken_small", "contamination" for bottle category)
5. The "ground_truth" data has masking image of the defect on the corresponding image in "test"

```bash
/mvtec-ad/   
├── bottle (1st category)
│   ├── ground_truth
│   │   ├── broken_large
│   │   ├── broken_small
│   │   └── contamination
│   ├── license.txt
│   ├── readme.txt
│   ├── test
│   │   ├── broken_large
│   │   ├── broken_small
│   │   ├── contamination
│   │   └── good
│   └── train
│       └── good
├── ...
└── zipper (15th category)
```


```python
# import packages
%matplotlib inline
import matplotlib.pyplot as plt
from ipywidgets import interact
from collections import defaultdict
import os
from IPython.display import Image

```

## Categories


```python
# Constants
mvtec_home = '/lab_shared/datasets/mvtec-ad'
ignores = ['license.txt', 'readme.txt']
data_types = ['train', 'test', 'ground_truth']
```


```python
data_info = []
categories = list(set(os.listdir(mvtec_home)) - set(ignores))
for c in categories:
    defect_type = os.listdir(mvtec_home + '/' + c + '/ground_truth')
    data_info.append({'category': c, 'defects': defect_type})
#print(data_info)

print([c['category'] for c in data_info])
print([c for c in data_info if c['category']=='bottle'][0])
'''
data_info = [{category: 'bottle', defects: ['broken_large', 'broken_small', 'contamination'] }, {...}]

'''
```

    ['transistor', 'capsule', 'carpet', 'grid', 'tile', 'leather', 'zipper', 'hazelnut', 'wood', 'screw', 'toothbrush', 'bottle', 'pill', 'metal_nut', 'cable']
    {'category': 'bottle', 'defects': ['contamination', 'broken_small', 'broken_large']}





    "\ndata_info = [{category: 'bottle', defects: ['broken_large', 'broken_small', 'contamination'] }, {...}]\n\n"



## Sample showing (for bottle)
### Normal


```python
@interact
def show_images(file=os.listdir(mvtec_home + '/bottle/train/good')):
    display(Image(mvtec_home + '/bottle/train/good/'+file))
```


    interactive(children=(Dropdown(description='file', options=('097.png', '003.png', '089.png', '095.png', '201.p…
<<<<<<< HEAD

![normal](./sample_img/1.png)
=======
!(normal)[./sample_img/1.png]
>>>>>>> a294afbe0dd19d6c2be638b9d70d213e7f3df04f

### Abnormal - 'broken large'


```python
@interact
def show_images(file=os.listdir('/lab_shared/datasets/mvtec-ad/bottle/test/broken_large')):
    display(Image('/lab_shared/datasets/mvtec-ad/bottle/test/broken_large/'+file))

```


    interactive(children=(Dropdown(description='file', options=('003.png', '006.png', '016.png', '010.png', '015.p…

![abnormal](./sample_img/2.png)

### Masking label for abnormal - 'broken large'


```python
@interact
def show_images(file=os.listdir(mvtec_home + '/bottle/ground_truth/broken_large')):
    display(Image(mvtec_home + '/bottle/ground_truth/broken_large/'+file))
```


    interactive(children=(Dropdown(description='file', options=('001_mask.png', '010_mask.png', '014_mask.png', '0…

![mask](./sample_img/3.png)

### Abnormal - 'contamination'


```python
@interact
def show_images(file=os.listdir('/lab_shared/datasets/mvtec-ad/bottle/test/contamination')):
    display(Image('/lab_shared/datasets/mvtec-ad/bottle/test/contamination/'+file))
```


    interactive(children=(Dropdown(description='file', options=('003.png', '006.png', '016.png', '010.png', '015.p…

![abnormal2](./sample_img/4.png)

### Abnormal for another type = 'transistor/misplaced'


```python
@interact
def show_images(file=os.listdir('/lab_shared/datasets/mvtec-ad/transistor/test/misplaced')):
    display(Image('/lab_shared/datasets/mvtec-ad/transistor/test/misplaced/'+file))
```


    interactive(children=(Dropdown(description='file', options=('003.png', '006.png', '008.png', '001.png', '009.p…

![abnormal3](./sample_img/5.png)
