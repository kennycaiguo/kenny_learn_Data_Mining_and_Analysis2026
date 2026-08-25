你可以通过在终端或 Notebook 中安装 `ipywidgets` 库来解决 `TqdmWarning: IProgress not found` 报错。 [[1](https://stackoverflow.com/questions/67998191/importerror-iprogress-not-found-please-update-jupyter-and-ipywidgets-although), [2](https://blog.csdn.net/MacwinWin/article/details/114369144)]

解决方法

大多数 users on [Stack Overflow](https://stackoverflow.com/questions/67998191/importerror-iprogress-not-found-please-update-jupyter-and-ipywidgets-although) 发生此问题是因为缺少 `ipywidgets` 或其版本未正确配置。请根据你的环境选择命令执行： []

- **使用 pip 安装：**

  bash

  ```
  pip install ipywidgets
  ```

  请谨慎使用此类代码。

- **使用 conda 安装：**

  bash

  ```
  conda install -c conda-forge ipywidgets
  ```