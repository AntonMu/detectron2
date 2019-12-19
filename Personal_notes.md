# Personal Notes
## Run Minimal Inference Example

```
python demo/demo.py --config-file configs/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x.yaml \
  --input test_data/input_img/input1.jpg test_data/input_img/input2.jpg \
  test_data/input_img/input3.jpg \
  --output test_data/output_img \
  --opts MODEL.WEIGHTS detectron2://COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/model_final_f10217.pkl MODEL.DEVICE cpu

```

## Run Training Inference Example
