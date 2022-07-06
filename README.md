# fairseq-on-docker

[Fairseq](https://github.com/pytorch/fairseq)をDocker上で動かすためのレポジトリ

# How to Train Transformer NMT Model on IWSLT 

```How to setup
git clone git@github.com:yukinon874/fairseq-on-docker-100knock.git
cd fairseq-on-docker-100knock

# write your username to .env file
echo "HOST_USERNAME=$(id -un)" > .env

# building container
docker compose build

# start container in background
docker compose up -d

# attach to running container
# **コンテナの名前は適宜 docker ps で確認すること**
docker exec -it fairseq_on_docker-fairseq-1 bash

# 以降はコンテナ内部で実行される
# プロンプトが
# root@c187cecdbc1c:/code/fairseq#
# みたいになっていればOK

```

# Repository Overview

- `fairseq` and `shell` directories are bind mounted to the container.
  - This means that your edits on these directories will be automatically synced to the running image.
  - This is really convenient when you are implementing your model, shell scripts, etc.

```
<project_root>
├── Dockerfile
├── README.md
├── build.sh
├── entrypoint.sh
├── fairseq  # binded to /code/fairseq
├── interactive.sh 
└── shell  # binded to /code/shell
```

- `/work00/<your_username>/fairseq_on_docker` is also bind mounted to the container
  - If you save something in `/work` of the container, then it will be synced to the host.
- If you want to bind another directory, edit `interactive.sh`

# Misc.

I have commented out `torch` and `torchaudio` requirements from fairseq's `setup.py`; 
otherwise it'll uninstall PyTorch from the container - This is the only modification that I have made from the original fariseq.
