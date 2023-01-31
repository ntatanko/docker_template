# Docker-based Data Science Template

- Python
- Docker
- Jupyter
- TensorFlow
- TensorBoard
- GPU Support

## Starting Jupyter Lab and TensorBoard

`$ docker/docker-forever.sh [--jupyter_port=####|8888] [--tensorboard_port=####|6006]`


## Misc

- TensorBoard log dir inside container is at `/app/.tensorboard`.
- Edit `JUPYTER_TOKEN` argument in `docker/Dockerfile` to set Jupyter access token
