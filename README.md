# 2021_Object_Detection_Foscar

# 학습 재현 # (40000장 epoch : 19번 학습 완료)  
```
cd yolov5 
python train.py --img 1280 --batch 4 --epochs 19 --data 'data/dataset_train.yaml' --cfg 'models/yolov5m.yaml' --weights best.pt --device 0
```
# 결과 재현 #
```
[mAP 측정]
cd yolov5/data/test
#해당 경로에 test img, 변환된 txt 추가후에
cd yolov5
python inference.py --data 'data/dataset.yaml' --weights 'runs/train/final/weights/best.pt' --verbose --iou-thres 0.6 --imgsz 1280 --task test --device 0
#Results saved to [경로]에서 PR_curve.png 확인
```
```
[결과 XML 변환]
cd yolov5 
bash inference.sh
```
## labeled img 
![label1](https://user-images.githubusercontent.com/92678942/146318234-31dc3d79-8ab9-4c94-b2a5-40d40bacec40.png)
## predicted img
![pred1](https://user-images.githubusercontent.com/92678942/146318252-62e1736f-a055-47d6-8435-a7600f44c7a0.png)

## Precision Recall Curve
![PR_curve](https://user-images.githubusercontent.com/92678942/146318303-fe363033-c793-4116-ab8c-845607efe29b.png)






## 1. jpg와 txt파일명을 맞추기 위한 코드로 이미지가 들어있는 폴더에서 실행 ##
``` for f in *.jpg; do mv -- "$f" "${f%.jpg}_v001_1.jpg"; done; ```

## 2. XML -> YOLO form ##
```
cd ~/convert2Yolo/example/voc/label   #xml 파일들 추가, cls_list_file.txt class name 추가 
cd convert2Yolo 
python3 convert2Yolo.py
python3 example.py --voc --img_path '이미지 경로' --label 'xml 경로' --convert_output_path ./ --img_type ".jpg" --manifest_path ./ --cls_list_file names.txt 
```

## 3. Class reduction(Vehicle, Pedestrian, Traffic_Light, Traffice_Sign) ##
```
cd ~/yolov5/conver2class
python3 retext.py
```

## 4.학습 재현 ##
```
cd yolov5
python train.py --img 1280 --batch 4 --epochs 200 --data 'data/dataset.yaml' --cfg 'models/yolov5m0.yaml' --weights final.pt --device 0
```

## 5. 결과 재현 ##
```
[결과 XML 변환]
cd yolov5 
bash contest.sh
```
```
[mAP 측정]
cd yolov5/data/test    # 해당 경로에 test img, 변환된 txt 추가후에
cd yolov5
python val.py --data 'data/dataset.yaml' --weights 'runs/train/final/weights/best.pt' --verbose --iou-thres 0.6 --imgsz 1280 --task test --device 0
Results saved to [경로]에서 PR_curve.png 확인
```