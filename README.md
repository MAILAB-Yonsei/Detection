## INSTALLATION

1. conda create -n yolor python=3.8
2. conda activate yolor
3. conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
4. cd detection/yolor
5. pip install -r requirements.txt

install mish-cuda if you want to use mish activation
https://github.com/thomasbrandon/mish-cuda
https://github.com/JunnYu/mish-cuda

git clone https://github.com/JunnYu/mish-cuda
cd mish-cuda
python setup.py build install
cd ..

install pytorch_wavelets if you want to use dwt down-sampling module
https://github.com/fbcotter/pytorch_wavelets
git clone https://github.com/fbcotter/pytorch_wavelets
cd pytorch_wavelets
pip install .
cd ..


## Preprocessing

cd ../
yolo_preproceesing.ipynb 파일을 실행시켜 yolo format에 맞게 데이터 전처리 진행

##, YoloR 
### TRAIN
1. cd yolor

2. python train.py --batch-size 16 --img-size 576 576 --data ../endoscopy.yaml --cfg cfg/yolor_w6.cfg --device 0 --sync-bn --name yolor_p6 --hyp hyp.scratch.1280.yaml --epochs 600 --weights ./runs/train/yolor_p626/weights/yolor_all_data.pt

multi scale을 적용하고 싶을 경우
2. python train.py --batch-size 16 --img-size 576 576 --data ../endoscopy.yaml --cfg cfg/yolor_w6.cfg --device 0 --sync-bn --name yolor_p6 --hyp hyp.scratch.1280.yaml --epochs 600 --weights ./runs/train/yolor_p626/weights/yolor_all_data.pt --multi-scale

### DETECT

tta 적용 x
1. python detect.py --save-txt --source ../Data/DACON/yolo/images/test --weights ./runs/train/yolor_p626/weights/yolor_all_data.pt --cfg ./cfg/yolor_w6.cfg --device 0 --img-size 576 --output ../inference/output32

tta 적용 o 
1. python detect.py --save-txt --source ../Data/DACON/yolo/images/test --weights ./runs/train/yolor_p626/weights/yolor_all_data.pt --cfg ./cfg/yolor_w6.cfg --device 0 --img-size 576 --output ../inference/output32 --augment

##, Test map csv 파일 생성
cd ../
python test_scores.py --data <data path> --save <save file path>

예시) python test_scores.py --data ./inference/output32 --save ./final_submission_yolor_full.csv
