# MNIST 数字识别 - 机器学习项目

这个项目使用**PyTorch**训练一个简单的**CNN模型**来识别MNIST数字，并通过**GitHub Actions**自动发布模型到**Hugging Face**。

本项目演示了一个完整的ML工作流：数据准备 → 模型训练 → 自动部署。

---

## 📁 项目结构

```
digit_recognition/
├── train_model.py                   # 训练脚本
├── requirements.txt                 # Python依赖
├── .github/
│   └── workflows/
│       └── train_and_deploy.yml    # GitHub Actions CI/CD配置
├── .gitignore                       # Git忽略文件
└── README.md                        # 项目说明（本文件）
```

---

## 🚀 快速开始

### 前置条件
- GitHub 账户
- Hugging Face 账户
- Python 3.8+ （本地开发用）

### 第1步：创建GitHub仓库

1. 访问 https://github.com/new
2. 创建新仓库：
   - **Repository name**: `digit-recognition`
   - **Description**: `MNIST digit recognition with GitHub Actions CI/CD`
   - **Public** ✓
   - 勾选 "Add a README file" ✓
   - 点击 **Create repository**

3. 克隆到本地：
```bash
git clone https://github.com/你的用户名/digit-recognition.git
cd digit_recognition
```

### 第2步：设置Hugging Face

1. 访问 https://huggingface.co 注册或登录
2. 生成访问令牌：
   - 进入 https://huggingface.co/settings/tokens
   - 点击 **New token**
   - 名称：`github-actions`
   - Role: **Write** ✓
   - 点击 **Generate**
   - **复制Token**（后面需要）

3. 记住你的**Hugging Face用户名**（后面需要）

### 第3步：配置GitHub Secrets

1. 打开你的GitHub仓库页面
2. 进入 **Settings** → **Secrets and variables** → **Actions**
3. 点击 **New repository secret**，添加两个Secret：

**Secret 1:**
- Name: `HF_TOKEN`
- Value: （粘贴你在第2步复制的Token）
- 点击 **Add secret**

**Secret 2:**
- Name: `HF_USERNAME`  
- Value: （粘贴你的Hugging Face用户名）
- 点击 **Add secret**

### 第4步：推送代码到GitHub

1. **复制项目文件到本地仓库**（或手动创建）
   - `train_model.py`
   - `requirements.txt`
   - `.github/workflows/train_and_deploy.yml`
   - `.gitignore`

2. **推送到GitHub**：
```bash
git add .
git commit -m "Add MNIST digit recognition project with CI/CD"
git push origin main
```

> 如果默认分支是 `master`，改为 `git push origin master`

### 第5步：等待GitHub Actions完成

1. 打开你的仓库 GitHub Actions 页面：
   👉 https://github.com/你的用户名/digit-recognition/actions

2. 你应该看到 **"Train and Deploy Model"** 工作流正在运行（黄色 🟡）

3. **等待完成**（约15-25分钟）：
   - 安装依赖 ⏱️ 2分钟
   - **训练模型** ⏱️ 10-15分钟 (最长)
   - 上传到Hugging Face ⏱️ 2分钟

4. 完成时会变成绿色 ✅

### 第6步：验证模型

1. 打开 Hugging Face 模型页面：
   👉 `https://huggingface.co/你的用户名/digit-recognition-model`

2. 你应该看到：
   - ✓ 模型文件 `pytorch_model.pt`
   - ✓ README 文档

🎉 **完成！你的模型已经成功训练并发布到Hugging Face！**

---

## 💻 本地开发

### 本地训练模型

```bash
# 1. 安装依赖
pip install -r requirements.txt

# 2. 运行训练脚本
python train_model.py
```

训练完成后，模型保存在 `model/mnist_cnn.pt`

### 本地推理示例

```python
import torch
from train_model import Net
from torchvision import transforms
from PIL import Image

# 加载模型
model = Net()
model.load_state_dict(torch.load('model/mnist_cnn.pt'))
model.eval()

# 准备图片
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.1307,), (0.3081,))
])

# 这里用你自己的数字图片（28x28像素的灰度图）
img = Image.open('digit.png').convert('L')
img_tensor = transform(img).unsqueeze(0)

# 预测
with torch.no_grad():
    output = model(img_tensor)
    prediction = output.argmax(dim=1).item()

print(f"预测数字: {prediction}")
```

---

## 📊 模型详情

| 项目 | 说明 |
|------|------|
| **架构** | CNN: Conv2d(1,32) → Conv2d(32,64) → FC(9216,128) → FC(128,10) |
| **训练数据** | MNIST (60,000张图片) |
| **测试数据** | MNIST (10,000张图片) |
| **目标精度** | ~99% 在测试集上 |
| **框架** | PyTorch |
| **训练时间** | ~10-15分钟 (CPU) |
| **输入** | 28×28 灰度图 |
| **输出** | 10个数字类别的概率分布 (softmax) |

---

## 🐛 故障排除

### GitHub Actions没有运行
1. ✓ 检查仓库 Settings → Actions 是否启用
2. ✓ 尝试手动触发：Actions → Workflows → Run workflow
3. ✓ 检查workflow文件是否在 `.github/workflows/` 目录
4. ✓ 查看工作流日志找出具体错误

### HF_TOKEN 错误
- ✓ 确认Token已在 https://huggingface.co/settings/tokens 生成
- ✓ 检查Secret名称是否正确：`HF_TOKEN`
- ✓ 尝试重新生成Token
- ✓ 在工作流日志中查看错误信息

### 模型上传失败
- ✓ 确保 `HF_TOKEN` 和 `HF_USERNAME` 都已设置
- ✓ 检查Token有 "Write" 权限
- ✓ 确保Hugging Face账户状态正常
- ✓ 查看GitHub Actions日志了解具体错误

### 本地训练失败
```bash
# 清理旧数据
rm -rf data/
rm -rf model/

# 重新训练
python train_model.py
```

---

## 📚 学习资源

- [PyTorch官方文档](https://pytorch.org/docs/)
- [MNIST数据集](http://yann.lecun.com/exdb/mnist/)
- [Hugging Face Model Hub](https://huggingface.co/models)
- [GitHub Actions文档](https://docs.github.com/en/actions)

---

## 📝 许可证

MIT License - 随意使用、修改和分发

---

## ✨ 项目亮点

- ✅ 完整的ML工程工作流演示
- ✅ 自动化CI/CD流程
- ✅ 模型自动发布到Hugging Face
- ✅ 详细的文档说明
- ✅ 可扩展的项目结构

---

## 🤝 贡献

欢迎提交Issue和Pull Request！

---

**Happy Machine Learning! 🚀**
