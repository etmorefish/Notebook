# AI

#####  数据标注工具[LabelImg](https://github.com/HumanSignal/labelImg)

- ###### Installation

  ```bash
  # 1、创建一个python虚拟环境，要求python版本不能太新，个人3.8、3.9成功，3.12安装报错
  
  # 使用conda新建一个python3.9的虚拟环境
  conda create --name py39 --python=3.9
  
  # 激活进入python虚拟环境
  conda activate py39
  
  pip3 install labelImg
  
  labelImg
  # 携带路径启动 labelImg
  labelImg [IMAGE_PATH] [PRE-DEFINED CLASS FILE]
  ```

- ###### Hotkeys

  | Ctrl + u         | Load all of the images from a directory   |
  | ---------------- | ----------------------------------------- |
  | Ctrl + r         | Change the default annotation target dir  |
  | Ctrl + s         | Save                                      |
  | Ctrl + d         | Copy the current label and rect box       |
  | Ctrl + Shift + d | Delete the current image                  |
  | Space            | Flag the current image as verified        |
  | w                | Create a rect box                         |
  | d                | Next image                                |
  | a                | Previous image                            |
  | del              | Delete the selected rect box              |
  | Ctrl++           | Zoom in                                   |
  | Ctrl--           | Zoom out                                  |
  | ↑→↓←             | Keyboard arrows to move selected rect box |