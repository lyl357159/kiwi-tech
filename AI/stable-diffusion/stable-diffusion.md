# stable-diffusion

## Mac M1 安装 stable-diffusion
  - 安装运行环境
  ```shell
  #使用Homebrew安装Python版本管理器pyenv,pyenv是python的多版本管理器
  brew install pyenv
  #安装所需的Python版本
  pyenv install 3.10
  #pyenv install 3.9.0
  # 设置全局Python版本
  pyenv global 3.10.10
  # 还可以使用pyenv创建虚拟环境来管理不同的项目和依赖项。在终端中输入以下命令创建名为myenv的虚拟环境：
  # pyenv virtualenv 3.6.0 myenv
  #然后，您可以使用以下命令激活虚拟环境：
  # pyenv activate myenv
  # 然后可以在虚拟环境中安装所需的Python包和依赖项，并运行您的项目。

  brew install cmake protobuf rust wget
  ```
  - clone 项目
  ```shell
  cd /Users/liulu/tool/AI
  git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
  ```
  # lora插件
  https://github.com/kohya-ss/sd-webui-additional-networks

 启动
 ```shell
 cd /Users/liulu/tool/AI/stable-diffusion-webui
 ./webui.sh --share
 ```


---
## 参考资料
 - [github-stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon)