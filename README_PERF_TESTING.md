## Performance Testing

As part of this README, I am putting together some additional commands that I used to test performance.

Not necessary to clone this fork but see uncommented cudnn and TransformerEngine installation related commands in [the Dockerfile](https://github.com/pramodk/Megatron-MoE-ModelZoo/blob/0c4b6c6be8be540f8c8a6f9b70228bc7c97e6a13/dockers/Dockerfile#L43).

```bash
git clone https://github.com/pramodk/Megatron-MoE-ModelZoo.git
cd Megatron-MoE-ModelZoo/
```

Next, download respective cudnn package:


```bash
wget https://developer.download.nvidia.com/compute/cudnn/redist/cudnn/linux-x86_64/cudnn-linux-x86_64-9.11.0.98_cuda12-archive.tar.xz
tar -xvJf cudnn-linux-x86_64-9.11.0.98_cuda12-archive.tar.xz
```

And now build an image:

```bash
docker build --target base -f dockers/Dockerfile --build-arg SSH_PRIVATE_KEY="$(cat ~/.ssh/id_rsa)" --tag nvcr.io/nvidian/megatron-moe-modelzoo-github:latest .
```

Optionally, push the image to registry:

```bash
# storing image
docker login -u \$oauthtoken -p <your_token>  nvcr.io
docker push nvcr.io/nvidian/megatron-moe-modelzoo-github:latest
```

To execute a job on Enroot+Pyxis based cluster, create a sqsh file:

```bash

# token setup
mkdir -p ~/.config/enroot
echo "machine nvcr.io login \$oauthtoken password nvapi-token" > ~/.config/enroot/.credentials

enroot import --output megatron-moe-modelzoo-github_latest.sqsh docker://nvcr.io/nvidian/megatron-moe-modelzoo-github:latest
```
