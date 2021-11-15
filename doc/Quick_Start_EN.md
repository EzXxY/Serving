## Paddle Serving Quick Start Examples

(English|[简体中文](./Quick_Start_CN.md))

This quick start example is mainly for those users who already have a model to deploy, and we also provide a model that can be used for deployment. in case if you want to know how to complete the process from offline training to online service, please refer to the AiStudio tutorial above.

### Boston House Price Prediction model

get into the Serving git directory, and change dir to `fit_a_line`
``` shell
cd Serving/python/examples/fit_a_line
sh get_data.sh
```

Paddle Serving provides HTTP and RPC based service for users to access

### RPC service

A user can also start a RPC service with `paddle_serving_server.serve`. RPC service is usually faster than HTTP service, although a user needs to do some coding based on Paddle Serving's python client API. Note that we do not specify `--name` here. 
``` shell
python3 -m paddle_serving_server.serve --model uci_housing_model --thread 10 --port 9292
```
<center>

| Argument                                       | Type | Default | Description                                           |
| ---------------------------------------------- | ---- | ------- | ----------------------------------------------------- |
| `thread`                                       | int  | `4`     | Concurrency of current service                        |
| `port`                                         | int  | `9292`  | Exposed port of current service to users              |
| `model`                                        | str  | `""`    | Path of paddle model directory to be served           |
| `mem_optim_off`                                | -    | -       | Disable memory / graphic memory optimization          |
| `ir_optim`                                     | bool | False   | Enable analysis and optimization of calculation graph |
| `use_mkl` (Only for cpu version)               | -    | -       | Run inference with MKL                                |
| `use_trt` (Only for trt version)               | -    | -       | Run inference with TensorRT                           |
| `use_lite` (Only for Intel x86 CPU or ARM CPU) | -    | -       | Run PaddleLite inference                              |
| `use_xpu`                                      | -    | -       | Run PaddleLite inference with Baidu Kunlun XPU        |
| `precision`                                    | str  | FP32    | Precision Mode, support FP32, FP16, INT8              |
| `use_calib`                                    | bool | False   | Only for deployment with TensorRT                     |

</center>

```python
# A user can visit rpc service through paddle_serving_client API
from paddle_serving_client import Client
import numpy as np
client = Client()
client.load_client_config("uci_housing_client/serving_client_conf.prototxt")
client.connect(["127.0.0.1:9292"])
data = [0.0137, -0.1136, 0.2553, -0.0692, 0.0582, -0.0727,
        -0.1583, -0.0584, 0.6283, 0.4919, 0.1856, 0.0795, -0.0332]
fetch_map = client.predict(feed={"x": np.array(data).reshape(1,13,1)}, fetch=["price"])
print(fetch_map)
```
Here, `client.predict` function has two arguments. `feed` is a `python dict` with model input variable alias name and values. `fetch` assigns the prediction variables to be returned from servers. In the example, the name of `"x"` and `"price"` are assigned when the servable model is saved during training.


### WEB service
Users can also put the data format processing logic on the server side, so that they can directly use curl to access the service, refer to the following case whose path is `python/examples/fit_a_line`

```
python3 -m paddle_serving_server.serve --model uci_housing_model --thread 10 --port 9292 --name uci
```
for client side,
```
curl -H "Content-Type:application/json" -X POST -d '{"feed":[{"x": [0.0137, -0.1136, 0.2553, -0.0692, 0.0582, -0.0727, -0.1583, -0.0584, 0.6283, 0.4919, 0.1856, 0.0795, -0.0332]}], "fetch":["price"]}' http://127.0.0.1:9292/uci/prediction
```
the response is
```
{"result":{"price":[[18.901151657104492]]}}
```
<h3 align="center">Pipeline Service</h3>

Paddle Serving provides industry-leading multi-model tandem services, which strongly supports the actual operating business scenarios of major companies, please refer to [OCR word recognition](./python/examples/pipeline/ocr).

we get two models
```
python3 -m paddle_serving_app.package --get_model ocr_rec
tar -xzvf ocr_rec.tar.gz
python3 -m paddle_serving_app.package --get_model ocr_det
tar -xzvf ocr_det.tar.gz
```
then we start server side, launch two models as one standalone web service
```
python3 web_service.py
```
http request
```
python3 pipeline_http_client.py
```
grpc request
```
python3 pipeline_rpc_client.py
```
output
```
{'err_no': 0, 'err_msg': '', 'key': ['res'], 'value': ["['土地整治与土壤修复研究中心', '华南农业大学1素图']"]}
```