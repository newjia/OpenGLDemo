
## 一、搭建环境
### 1.1 创建项目
注意，我们使用Mac 应用来熟悉OpenGL
![](https://nilsli.com/p/7b6be80a/001.png) 
### 1.2 添加库
这里需要添加的是GLUT和OpenGL,这些都是苹果内置的。
**步骤**：在Build Phrases - Link Binary With Libraries 里，点击加号后，搜索GLUT 和OpenGL添加，如下图：
![](https://nilsli.com/p/7b6be80a/002.png)
![](https://nilsli.com/p/7b6be80a/003.png)
![](https://nilsli.com/p/7b6be80a/004.png)
### 1.3 引入第三方库
1. 将文件包内的`include` 文件夹里拖入拷贝至工程内
    ![](https://nilsli.com/p/7b6be80a/005.png)
2. 把静态库 `libGTools.a` 拖入Framework 文件夹下
    ![](https://nilsli.com/p/7b6be80a/006.png)
3. 结果的文件夹结构如下：
    ![](https://nilsli.com/p/7b6be80a/007.png)
### 1.4 单独引用 GLTools.h 和glew.h：
    具体操作：在Build Phrases —— Complie Sources 添加GLTools.h 和glew.h文件。结果如下：
    ![](https://nilsli.com/p/7b6be80a/008.png)
### 1.5 删掉多余代码：

    需要删除的有：
    * AppDelegate.h
    * AppDelegate.m
    * ViewController.h
    * ViewController.m
    * Main.stroyboard
    * main.m
目录如图所示：
    ![](https://nilsli.com/p/7b6be80a/009.png)
    
### 1.5 添加启动文件 main.cpp
1. 创建c++ 文件：
    ![](https://nilsli.com/p/7b6be80a/010.png)
2. 添加 main 文件，注意关掉创建头文件的对勾
    ![](https://nilsli.com/p/7b6be80a/011.png)
3. 创建基础代码，如下：
    
    ```
    int main(int argc, char * argv[])
    {
        return 0;
    }
    ```
至此，本Demo 已经可以独立运行了

## 二、扩展：绘制三角形
### 2.1 业务代码
上面的环境搭建好了，何不来个小小的Demo 来验证一下，输入业务代码如下：

```
#include "GLTools.h"

#include <GLUT/GLUT.h>

GLBatch triangleBatch;

GLShaderManager shaderManager;

void ChangeSize(int w, int h)
{
    glViewport(0, 0, w, h);
}

void SetupRC()
{
    glClearColor(0.0f, 0.0f, 1.0f, 1.0f);
    
    shaderManager.InitializeStockShaders();
    
    GLfloat vVerts[] = {
        -0.5f, 0.0f, 0.0f,
        0.5f, 0.0f, 0.0f,
        0.0f, 0.5f, 0.0f,
    };
    
    triangleBatch.Begin(GL_TRIANGLES, 3);
    
    triangleBatch.CopyVertexData3f(vVerts);
    
    triangleBatch.End();
}

void RenderScene(void)
{
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT | GL_STENCIL_BUFFER_BIT);
    
    GLfloat vRed[] = {
        1.0f, 0.0f, 0.0f, 1.0f
    };
    
    shaderManager.UseStockShader(GLT_SHADER_IDENTITY, vRed);
    
    triangleBatch.Draw();
    
    glutSwapBuffers();
}

int main(int argc, char * argv[])
{
    gltSetWorkingDirectory(argv[0]);
    
    glutInit(&argc, argv);
    
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGBA | GLUT_DEPTH | GLUT_STENCIL);
    
    glutInitWindowSize(800, 600);
    
    glutCreateWindow("Triangle");
    
    glutReshapeFunc(ChangeSize);
    
    glutDisplayFunc(RenderScene);
    
    GLenum err = glewInit();
    
    if (GLEW_OK != err) {
        fprintf(stderr, "glew error: %s\n", glewGetErrorString(err));
        return 1;
    }
    
    SetupRC();
    
    glutMainLoop();
    
    return 0;
}

```
### 2.2 小bug 解决
运行后发现会出现如下bug，

![](https://nilsli.com/p/7b6be80a/012.png) 

这里提示的是饮用方式的不对，修改根据提示修改，结果如下：

![](https://nilsli.com/p/7b6be80a/013.png) 

其他的错误提示也算大同小异，也一样的根据系统提示做修改。

### 2.3 结果展示

最终结果如下：

![](https://nilsli.com/p/7b6be80a/014.png) 

在这里画了一个简单的三角形，并用红色进行和内部渲染。

## 三、小结
本文介绍了OpenGL环境搭建，并简单的通过业务代码创建了一个红色三角形，由于是初次接触，内部的语法不妨略过，享受一下结果，会在以后的章节进行步骤的详细讲解。


* 附：Demo的 [Github下载](https://github.com/newjia/OpenGLDemo)
    * include 文件夹
    * libGLTools.a 静态文件