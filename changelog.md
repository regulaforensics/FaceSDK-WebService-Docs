# Changelog

## v3.0

### Core Functionality

#### Detection

* New face detection algorithm, processing arbitrary face orientations. 
* Face thumbnail in responses, cropped and properly aligned.
* Rotation angle in response. 
* Face attributes: age\(child/adult\), emotion\(neutral/smile\).

#### Matching

* Face ROI in response for face matching for unambiguous results interpretation. 
* Face thumbnail in responses.

#### Liveness 3D

* Updated algorithms for liveness classification. 
* Rejecting faces with occlusions \(medicine mask, sunglasses, hat, hands\). 
* Rejecting art face masks. 
* Rejecting faces with closed eyes.

#### Liveness Depth

* Updated model for classification.

### Web Server

#### Changed

* **WARNING**: app validation logic returns 200 instead of 400 http codes. Previously, if you supply bad images, or wrong params, you will get response with 400 http code. Starting from this version, you will always get 200 result and should check **code** property inside response json-body to get operation result. All possible codes are listed [here](https://github.com/regulaforensics/FaceSDK-web-openapi/blob/1a682a5dae2eb5e39a329127ff9e01a5f9cc84f0/common.yml#L45)
* **WARNING**: all project related libraries changed their names and artifacts names to **FaceSDK**. Now project name is consistent across all platforms. 
* **WARNING**: _/detect_ now by default return only central face.
* _face compare_ operation renamed to _face match_ operation. This leads to changing model names in clients. _/compare_ endpoint renamed to _/match_ endpoint. Old models and routes are still present via type aliases for backward compatibility.
* Migrate to python 3.8.
* \[docker\] by default do not spam access log to stdout.
* \[docker\] send all apps logs to **stdout** instead of stderr.
* \[docker\] logs format changed.
* \[linux,win\] by **default** store app logs into daily rotating "logs/app/facesdk-app.log" file.

#### Added

* HTTPS certs **custom path** options for all platforms \(previously was only in docker\).
* Process logs now support **try\_id** params\(beside custom id\). Now operations inputs and results saved under next id `invocation_id = uuid.uuid4().hex if not id else id + '_' + try_id`.
* **C\#** client published.
* Logging format unified across all platforms.
* Added ability to store logs into files with daily **rotation**.
* **JSON** log formatting \(useful for log collectors\).

