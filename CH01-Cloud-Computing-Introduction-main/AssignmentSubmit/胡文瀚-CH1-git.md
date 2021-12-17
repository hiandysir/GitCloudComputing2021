# git

## 安装git
windows端操作，配置vscode（略）

## 配置git
```command
# 设置用户名
git config --global user.name 'kranehuan'

# 设置邮箱
git config --global user.email 'kranehuan@outlook.com'

# 生成密钥
ssh-keygen -t rsa -C 'kranehuan@outlook.com'
```
然后去github配置ssh密钥

## 使用git
```command
# 进入git路径
cd /

# 创建存储库
git init

# 克隆存储库（本课程）
git clone https://github.com/hiandysir/GitCloudComputing2021.git
```