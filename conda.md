# conda 指令

## 管理 conda 自身

- `conda --version` 查看 conda 版本
- `conda config --show` 查看 conda 环境配置
- `conda update conda` 更新 conda 到最新版本
- `conda update Anaconda` 更新 Anaconda 整体

## 虚拟环境操作

- `conda create -n <environment-name> python=<edition>` 使用 conda 创建名为 **environment-name** ， Python 版本为 **edition** 虚拟环境
- `conda info -e` 查看目前 conda 已有的虚拟环境，其中带 \* 号的表示为当前活动环境
- `conda activate <environment-name>` 激活名为 **environment-name** 的虚拟环境
- `conda deactivate` 退出当前虚拟环境，回到 **base environment**
- `conda remove -n <environment-name> --all` 可以将指定虚拟环境及其中所安装的包都删除
- `conda remove -n <environment-name> <package-name>` 可以将指定虚拟环境中的某个包删除
- `conda env creat -f <environment-name>.yml` 可以导出环境

## 包的管理

- `conda list` 查询包的安装情况
- `conda list <package-name>` 查询是否安装某个包
- `conda instal <package-name>(=<edition>)` 在当前虚拟环境安装一个包，同时可以补充特定版本型号
