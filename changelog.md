# Changelog

## v3.0

### Functionality

#### Detection.
1.1 New face detection algorithm, processing arbitrary face orientations.
1.2 Face thumbnail in responses, cropped and properly aligned.
1.3 Rotation angle in response.
1.4 (beta) Face attributes: age(child/adult), emotion(neutral/smile).
#### Matching.
2.1 Face ROI in response for face matching for unambiguous results interpretation.
2.2 Face thumbnail in responses.
#### Liveness 3D.
3.1 Updated algorithms for liveness classification.
3.2 Rejecting faces with occlusions (medicine mask, sunglasses, hat).
3.3 Rejecting faces with art masks.
3.4 Rejecting faces with closed eyes.
#### Liveness Depth.
4.1 Updated model for classification.

### Web Server

#### Changed

- **WARNING**: app validation logic returns 200 instead of 400 http codes. 
Previously, if you supply bad images, or wrong params, you will get response with 400 http code.
Starting from this version, you will always get 200 result and should check **code** property inside json-body, to get operation result.
All possible codes listed [here](https://github.com/regulaforensics/FaceSDK-web-openapi/blob/1a682a5dae2eb5e39a329127ff9e01a5f9cc84f0/common.yml#L45)
- **WARNING**: all related to project libraries changed their name and artifacts names to **FaceSDK**. Now project name is consistent across all platforms. 
- **WARNING**: /detect now by default return only central face
- face compare operation renamed to face match operation. 
This leads to chaining models names in clients. /compare endpoint renamed to /match endpoint. 
Old models and routes are kept via type aliases.
- Migrate to python 3.8
- [docker] by default do not spam access log to stdout
- [docker] send all apps logs to stdout instead of stderr
- [docker] logs format changed
- [linux,win] by default store app logs into rotating "logs/app/facesdk-app.log" file


#### Added

- add FACEAPI_CERT_FILE, FACEAPI_KEY_FILE env options for https for all platforms(previously was ony in docker)
- process logs now support try_id params(beside custom id). 
Now operations inputs and results saved under next id `invocation_id = uuid.uuid4().hex if not id else id + '_' + try_id`
- add csharp client
- unify logging format across all platforms
- add ability to store logs into files with daily rotation
- json log formatting (useful for log collectors)

