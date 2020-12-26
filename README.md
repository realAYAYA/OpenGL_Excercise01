# OpenGL
learnopengl.com

#### 部署教程：
#### 1.vs安装glsl扩展
#### 2.项目属性c/c++和链接中，常规加入对应的include和lib文件夹
#### 3.把stb_image.h放到项目目录直接用
#### 4.项目属性-链接库-输入里加入 opengl32.lib,glfw3.lib,glew32s.lib这三个库
#### 5.glm在c/c++中加入目录glm/glm，一层glm还不是，要里面那个
#### 6.导入Assimp库，是一个可以载入3D模型的库，工具-NuGet，自行搜索下载

#### 源：
#### https://www.glfw.org/download.html	    从这个网站下载GLFW库
#### http://glew.sourceforge.net/index.html	从这个网站下载GLEW库




#### 重大笔记 2020/12/26 ---立方体贴图---章节
我按照源码将教程做完，发现在最后的优化部分（即令skyboxShader的深度缓冲固定设为1，再令skybox和人物的绘制顺序相反）
但没能成功，skybox绘制成功，人物却绘制失败。我检索代码几天。最后严格按照教程代码的 执行顺序 ，最终发现：
代码在绘制人物之前，先执行一次shader.use()后在将 矩阵转换(Mat) 传入。成功；
而我，则是在不经意间将shader.use()封装进我自己的model类里的Draw()方法中进行调用，然后在Draw()之前，进行的矩阵转换(Mat)传入，然后我手动将shader.use()拿出来，单独先执行，再Draw()，发现成功！

综上所述，我犯的错误在于，我先绘制skybox，也就是先进行skyboxShader->use()，然后传入人物矩阵变换，错误的将矩阵数据传入到了错误的shader里，以至于人物在绘制的时候没能正常进行矩阵变换而导致绘制失败。
用了四天时间，补一个漏掉的知识点(或者叫常识)，在往shader程序中传值的时候，一定要先use(),再传值!!!一定要先use(),再传值!!!一定要先use(),再传值!!! 重要说三遍!!!
细心真可怕，不细心，要命!!!