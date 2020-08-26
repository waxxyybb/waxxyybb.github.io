# 动漫头像生成
一直对生成对抗网络比较感兴趣,于是参见李宏毅老师的[视频](https://www.youtube.com/playlist?list=PLJV_el3uVTsMq6JEFPW35BCiOQTsoqwNw)和[课件](http://speech.ee.ntu.edu.tw/~tlkagk/courses_MLDS18.html),做了一个生成动漫头像的Demo.
主要做的是*无规则的头像生成*和*有约束的头像生成(Conditional Gan)*

目前只完成了随机图像的生成( Homework 3-1)



## Homework 3-1
无规则头像的生成

### 模型结构

采用的网络结构是[DCGAN](DCGAN),最终的结果

### Future Work
DCGAN的画面不够精细，未来会考虑引入ResNet残差网络提高模型的拟合效果

### 一些细节问题
1.  为什么中间Conv2d和Conv2dTranspose都会设置```bias = False```
2.  如何达到tensorflow中```padding='same'```的效果
3.  bn和activation层的顺序问题
4.  ```same_random```中部分语句的作用

### 收获
学会了新的网络结构Conv2Transpose

## Homewordk 3-2
Conditional Gan( Homewrok 3-2)效果不理想，等我新工作入职了再来完善


## Data Link
[Anime Dataset](https://drive.google.com/drive/folders/1mCsY5LEsgCnc0Txv0rpAUhKVPWVkbw5I?usp=sharing)

## Reference
[https://blog.mythsman.com/2017/01/14/1/](https://blog.mythsman.com/2017/01/14/1/)  

[https://blog.csdn.net/quantumenergy/article/details/78244003](https://blog.csdn.net/quantumenergy/article/details/78244003)  

[http://whatbeg.com/2018/12/05/jupyternotebook-1.html](http://whatbeg.com/2018/12/05/jupyternotebook-1.html)

[Unsupervised Representation Learning With Deep Convolutional Generative Adversarial Networks](https://arxiv.org/pdf/1511.06434.pdf)

[DCGAN TUTORIAL/Pytorch](https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html)

[基于GAN的动漫头像生成](https://zhuanlan.zhihu.com/p/76340704)