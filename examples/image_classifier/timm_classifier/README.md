### Sample commands to transit from Timm pytorch-image-models to torch serve

1. [Convert your pth file into torchscript](https://github.com/jinmingteo/pytorch-image-models/blob/master/torchscript_model_converter.py)

2. Build serve docker image
  - Refer to docker/README.md and run their ```build_image.sh```

3. Mount your pt file into the docker
Example: resnet50.pt

  - Run docker
  ```
  docker run --rm -it --gpus all -p 8080:8080 -p 8081:8081 -p 8082:8082 -p 7070:7070 -p 7071:7071 -v /drive/fish/serve/:/home/model-server/serve pytorch/torchserve:dev-gpu bash
  ```

  - Run converter.
  
  - **NOTE:** Before running, Change your classes in index_to_name.json; Change your preprocessing at image_classifier handle
  ```
  torch-model-archiver --model-name resnet50 --version 1.0  --serialized-file resnet_best.pt --extra-files examples/image_classifier/timm_classifier/index_to_name.json --handler image_classifier


  torchserve --start --model-store model-store/ --models classification=resnet18.mar

  ```

  - Test your API
  ```
  curl http://127.0.0.1:8080/predictions/classification -T examples/image_classifier/kitten.jpg
  ```
