---
title: threejs与webgl
date: 2020-09-29 10:48:22
tags: 
  - 技术
  - 百宝箱
---
## 最小环境

Threejs在底层其实还是调用html5中的canvas api来实现绘图的，Threejs在底层使用的是canvas的webgl context来实现3D绘图，其中比绘制2D图像更直接对gpu操作。

Threejs在顶层对3D绘图所需的各种元素（例如场景，摄影机，灯光，几何图像，材质等）进行了封装，如果我们需要使用Threejs来绘图，需要创建一个最小绘图环境

一个最小绘图环境包含了三个要素：

1. 场景–包含所有需要显示的3D物体以及其他相关元素的容器
2. 摄像机–决定3D场景如何投影到2D画布之上
3. 渲染器–用于最后绘制的画笔

```
import { Scene, PerspectiveCamera, WebGLRenderer } from ‘three’;

var scene = new Scene(); // 创建场景

var camera = new PerspectiveCamera(45, window.innerWidth / window.innerHeight, 1, 1000); // 创建摄影机
camera.position.z = 8;

var renderer = new WebGLRenderer(); // 创建渲染器
renderer.setSize(window.innerWidth, window.innerHeight); // 设置画布大小
renderer.setPixelRatio(window.devicePixelRatio); // 设置像素比，针对高清屏
renderer.setClearColor(0x000000, 1); // 设置默认背景色

document.body.appendChild(renderer.domElement); // 把画笔插入到dom中Copy
```

简单的几句代码，就可以建立起一个最小绘图环境

我们还需要告诉Threejs，到底需要显示什么物体。

```
import { Mesh, MeshBasicMaterial, BoxGeometry } from ‘three’;

var geometry = new BoxGeometry(1, 1, 1); // 创建一个长方体，用来定义物体的形状

var material = new MeshBasicMaterial({ color: 0xff0000 }); // 创建一个材质，用来定义物体的颜色

var mesh = new Mesh(geometry, material); // 使用形状和素材，来定义物体

scene.add(mesh);

renderer.render(scene, camera);Copy
```

Threejs在定义一个3D物体时，需要提供两个信息：

1. 形状信息，也就是这个物体上每一个点，每一个面的坐标信息
2. 材质信息，用于告诉Threejs物体的颜色，纹理，反光等信息。

`new BoxGeometry(1, 1, 1)`是告诉Threejs要显示一个长宽高各为1的长方形，而`new MeshBasicMateriall({ color: 0xff0000})`是告诉Threejs要显示的长方形颜色是红色，最后根据形状和素材`new Mesh(geometry, material)`，生成需要显示的物体。

物体的材质用于确定物体的颜色，纹理，以及反光等属性。Threejs里的灯光设置需要设置光源

```
import { SpotLight } from ‘three’;
var light = new SpotLight(0xffffff);、
light.position.z = 5;
light.position.x = 5;
light.position.y = 5;

scene.add(light);Copy
```

MeshLambertMaterial的材质，这种材质的特点是漫反射强烈，主要用来模拟真实环境下的物体，例如木材，石料等物质的反光情况。另外Threejs还有另外一种材质叫MeshPhongMaterial，这种材质主要是镜面反射强烈，用来模拟镜子，金属等拥有高光的物体就比较合适。

如果绘制3D物体时，只能使用纯色，那也未免太单调了，没关系，Threejs提供了接口来帮忙解决这个问题。Threejs的材质，除了可以设置颜色，还支持纹理贴图，我们可以把一个图片，覆盖在3D物体上作为他的纹理

```
import { TextureLoader, MeshLambertMaterial } from ‘three’;

var texture = new TextureLoader().load(‘./assets/texture/crate.gif’);

var material = new MeshLambertMaterial({ map: texture });Copy
```

能绘图了，但如何加入动画？

Threejs是通过渲染器来绘图的，我们在场景中摆好灯光，摆好道具，渲染器就绘制画面。那如果要做成动画，只需要在渲染器来个定时连拍。

```
function render() {
    requestAnimationFrame(render);
    update();
    renderer.render(scene, camera);
}

function update() {
    // update your view
}Copy
```

通过requestAnimationFrame接口，来做定时刷新，每次进入render方法，都会先去执行update方法（用于更新场景），然后让渲染器拍照（renderer.render(scene, camera)），最后等待下一次render。

在update方法中，我们可以修改场景中所有物体的参数

```
function update() {
    box.rotation.x += 0.005;
    box.rotation.y += 0.01;
}Copy
```

这样就可以让盒子在屏幕中转动

Threejs还有很多额外的能力，例如刚刚我们使用图片作为纹理，那么我们也可以使用视频作为纹理，把这个纹理贴到一个盒子上，通过陀螺仪来控制摄像机的拍摄方向，Threejs也支持粒子系统，模型数据导入，自定义着色器等一系列高级功能

