# suntech_ocr
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
