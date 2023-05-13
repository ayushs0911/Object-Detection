
## Dataset

```
!kaggle datasets download -d andrewmvd/face-mask-detection
```


## Installations 
```
!python -m pip install pyyaml==5.1
import sys, os, distutils.core
# See https://detectron2.readthedocs.io/tutorials/install.html for full installation instructions
!git clone 'https://github.com/facebookresearch/detectron2'
dist = distutils.core.run_setup("./detectron2/setup.py")
!python -m pip install {' '.join([f"'{x}'" for x in dist.install_requires])}
sys.path.insert(0, os.path.abspath('./detectron2'))
```
## Detectron Setup 
```
# Setup detectron2 logger
import detectron2
from detectron2.utils.logger import setup_logger
setup_logger()

# import some common libraries
import numpy as np
import os, json, cv2, random
from google.colab.patches import cv2_imshow

# import some common detectron2 utilities
from detectron2 import model_zoo
from detectron2.engine import DefaultPredictor
from detectron2.config import get_cfg
from detectron2.utils.visualizer import Visualizer
from detectron2.data import MetadataCatalog, DatasetCatalog
```
## Registering my data and metadata 
```
for d in ["annotations"]:
    DatasetCatalog.register("5" + d, lambda d=d: convert_xml_to_coco("/content/" + d + "/"))
    MetadataCatalog.get("5" + d).set(thing_classes=["with_mask", "without_mask", "mask_weared_incorrect"])
mask_metadata = MetadataCatalog.get("5annotations")
```

## Training 
```

from detectron2.engine import DefaultTrainer

cfg = get_cfg()
cfg.merge_from_file(model_zoo.get_config_file("COCO-Detection/faster_rcnn_R_50_FPN_3x.yaml"))
cfg.DATASETS.TRAIN = ("4annotations",)
cfg.DATASETS.TEST = ()
cfg.DATALOADER.NUM_WORKERS = 2
cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url("COCO-Detection/faster_rcnn_R_50_FPN_3x.yaml")   
cfg.SOLVER.IMS_PER_BATCH = 8  
cfg.SOLVER.BASE_LR = 0.00025   
cfg.SOLVER.MAX_ITER = 2000   
cfg.SOLVER.STEPS = []        
cfg.MODEL.ROI_HEADS.BATCH_SIZE_PER_IMAGE = 128   
cfg.MODEL.ROI_HEADS.NUM_CLASSES = 3  
# NOTE: this config means the number of classes, but a few popular unofficial tutorials incorrect uses num_classes+1 here.

os.makedirs(cfg.OUTPUT_DIR, exist_ok=True)
trainer = DefaultTrainer(cfg) 
trainer.resume_or_load(resume=False)
trainer.train()
```






## Test Results <br>
 
![download (1)](https://github.com/ayushs0911/Object-Detection/assets/122048067/acb43297-8180-4193-882c-83c6abc4eb17)
![download (2)](https://github.com/ayushs0911/Object-Detection/assets/122048067/499609d6-f646-489a-ad94-a547d8561f5a)
![download](https://github.com/ayushs0911/Object-Detection/assets/122048067/2e0f7130-ee81-4414-94b5-2d37a7e77c7e)
