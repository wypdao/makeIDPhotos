<div align="center">

<h1>makeIDPhoto</h1>

</div>

<br>

# 项目简介

makeIDPhoto 旨在开发一种实用、系统性的证件照智能制作算法。

它利用一套完善的AI模型工作流程，实现对多种用户拍照场景的识别、抠图与证件照生成。

**makeIDPhoto 可以做到：**

1. 轻量级抠图（纯离线，仅需 **CPU** 即可快速推理）
2. 根据不同尺寸规格生成不同的标准证件照、六寸排版照
3. 支持 纯离线 或 端云 推理
4. 美颜
5. 智能换正装（waiting）

<br>


# 🚀 Python 推理

核心参数：

- `-i`: 输入图像路径
- `-o`: 保存图像路径
- `-t`: 推理类型，有idphoto、human_matting、add_background、generate_layout_photos可选
- `--matting_model`: 人像抠图模型权重选择
- `--face_detect_model`: 人脸检测模型选择

更多参数可通过`python inference.py --help`查看

## 1. 证件照制作

输入 1 张照片，获得 1 张标准证件照和 1 张高清证件照的 4 通道透明 png

```python
python inference.py -i demo/images/test0.jpg -o ./idphoto.png --height 413 --width 295
```

## 2. 人像抠图

输入 1 张照片，获得 1张 4 通道透明 png

```python
python inference.py -t human_matting -i demo/images/test0.jpg -o ./idphoto_matting.png --matting_model hivision_modnet
```

## 3. 透明图增加底色

输入 1 张 4 通道透明 png，获得 1 张增加了底色的 3通道图像

```python
python inference.py -t add_background -i ./idphoto.png -o ./idphoto_ab.jpg  -c 4f83ce -k 30 -r 1
```

## 4. 得到六寸排版照

输入 1 张 3 通道照片，获得 1 张六寸排版照

```python
python inference.py -t generate_layout_photos -i ./idphoto_ab.jpg -o ./idphoto_layout.jpg  --height 413 --width 295 -k 200
```

## 5. 证件照裁剪

输入 1 张 4 通道照片（抠图好的图像），获得 1 张标准证件照和 1 张高清证件照的 4 通道透明 png

```python
python inference.py -t idphoto_crop -i ./idphoto_matting.png -o ./idphoto_crop.png --height 413 --width 295
```

<br>

# 🐳 Docker 部署

## 1. 拉取或构建镜像


**方式一：拉取最新镜像：**

```bash
docker pull registry.cn-beijing.aliyuncs.com/wypdao/makeidphotos
```

## 2. 运行服务

**启动 Gradio Demo 服务**

运行下面的命令，在你的本地访问 [http://127.0.0.1:7860](http://127.0.0.1:7860/) 即可使用。

```bash
docker run -d -p 7860:7860 wypdao/makeidphotos
```

**启动 API 后端服务**

```bash
docker run -d -p 8080:8080 wypdao/makeidphotos python3 deploy_api.py
```

<br>

# 📜 Lincese

This repository is licensed under the [Apache-2.0 License](LICENSE).
