
# windows 部署
安装模型
hub install deploy\hubserving\suntech_ocr\
启动
hub serving start -m suntech_ocr -p 8877
hub serving start -c deploy/hubserving/suntech_ocr/config.json

测试
python tools/test_hubserving.py http://127.0.0.1:8877/predict/suntech_ocr ./doc/imgs/
停止
hub serving stop -p 8877

卸载
hub uninstall ocr_system

paddlehub

# docker 部署

## Docker化部署服务
在日常项目应用中，相信大家一般都会希望能通过Docker技术，把PaddleOCR服务打包成一个镜像。


## 1.实施前提准备

需要先完成如下基本组件的安装：
a. Docker环境
b. 显卡驱动和CUDA 10.0+（GPU）
c. NVIDIA Container Toolkit（GPU，Docker 19.03以上版本可以跳过此步）
d. cuDNN 7.6+（GPU）

## 2.制作镜像
详见Dockerfile

c.生成镜像
```
docker build -t paddleocr:cpu . 
```

## 3.启动Docker容器
a. CPU 版本
```
docker run --name suntech-ocr -p 8866:8866 -d suntechocr:cpu
```
b. GPU 版本 (通过NVIDIA Container Toolkit)
```
sudo nvidia-docker run -dp 8866:8866 --name suntech-ocr suntechocr:gpu
```
c. GPU 版本 (Docker 19.03以上版本，可以直接用如下命令)
```
sudo docker run -dp 8866:8866 --gpus all --name suntech-ocr suntechocr:gpu
```
d. 检查服务运行情况（出现：Successfully installed ocr_system和Running on http://0.0.0.0:8866/等信息，表示运行成功）
```
docker logs -f paddle_ocr
```

## 4.测试服务
a. 计算待识别图片的Base64编码（如果只是测试一下效果，可以通过免费的在线工具实现，如：http://tool.chinaz.com/tools/imgtobase/）
b. 发送服务请求（可参见sample_request.txt中的值）
```
curl -H "Content-Type:application/json" -X POST --data "{\"images\": [\"填入图片Base64编码(需要删除'data:image/jpg;base64,'）\"]}" http://localhost:8866/predict/ocr_system
```
c. 返回结果（如果调用成功，会返回如下结果）
```
{"msg":"","results":[[{"confidence":0.8403433561325073,"text":"约定","text_region":[[345,377],[641,390],[634,540],[339,528]]},{"confidence":0.8131805658340454,"text":"最终相遇","text_region":[[356,532],[624,530],[624,596],[356,598]]}]],"status":"0"}
```
