# 数字识别机器学习项目

这个项目使用PyTorch训练一个简单的CNN模型来识别MNIST数字，并通过GitHub Actions自动发布到Hugging Face。

## 项目结构

- `train_model.py`: 训练脚本，使用MNIST数据集训练CNN模型。
- `requirements.txt`: Python依赖。
- `.github/workflows/train_and_deploy.yml`: GitHub Actions workflow。

## 完整步骤

### 1. 设置Hugging Face账户

1. 访问 [Hugging Face](https://huggingface.co) 并注册账户。
2. 创建一个新的模型仓库，命名为 `digit-recognition-model`。
3. 生成一个访问令牌（Token）：
   - 转到 Settings > Access Tokens。
   - 创建一个新的Token，选择 "Write" 权限。

### 2. 设置GitHub Secrets

1. 在GitHub仓库中，转到 Settings > Secrets and variables > Actions。
2. 添加一个新的Secret：
   - Name: `HF_TOKEN`
   - Value: 你的Hugging Face Token。

### 3. 运行项目

1. 确保代码在 `main` 分支上。
2. 推送代码到GitHub，这将触发GitHub Actions自动训练模型并上传到Hugging Face。

### 4. 本地测试（可选）

如果你想本地运行：

```bash
pip install -r requirements.txt
python train_model.py
```

这将下载MNIST数据，训练模型，并保存为 `mnist_cnn.pt`。

## 注意事项

- 训练可能需要几分钟，取决于硬件。
- 确保仓库是公开的，以便Hugging Face上传。
- 如果需要GPU训练，可以修改workflow使用GPU runner。
