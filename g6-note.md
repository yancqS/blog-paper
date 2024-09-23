---
title: G6 图可视化小记
date: '2024-03-01'
cover: 'https://img.up-4ever.site/infinity-9909115.jpg'
top_img: 'http://img.up-4ever.site/infinity-9909115.jpg'
tags:
  - JavaScript
  - 可视化
categories:
  - 可视化
abbrlink: 58c191cf
---
# G6 图可视化小记

## 什么情况下使用 G6？为什么不是 X6?
`antv G6` 和 `antv X6` 都是蚂蚁集团（Ant Group）开源的图形可视化库，但它们各自针对的使用场景有所不同。`antv G6` 是一个专注于**关系数据（图数据）的可视化**引擎，适用于构建关系图、网络拓扑、流程图等场景。它提供了丰富的图算法、图布局、交互方式和可视化能力，侧重于处理和展示节点之间的复杂连接关系。`G6` 的常见应用场景包括社交网络分析、知识图谱、程序依赖图、组织结构图等。而 `antv X6` 则是一个针对开发者快速构建**图编辑应用**的 JavaScript 图形库，适用于构建流程图、ER 图、甘特图和其他图表编辑相关的应用。`X6` 提供了更为丰富的交互组件和图形编辑能力，支持拖拽、缩放、对齐、撤销/重做等图编辑功能，并且其设计更为通用，不仅限于关系数据可视化。

-  **antv G6**： 
   - 适合关系数据可视化
   - 更强的图算法和布局算法支持
   - 专注于图表本身的可视化和交互
-  **antv X6**： 
   - 适合图形编辑器的构建
   - 提供完整的图形编辑工具和用户交互能力
   - 更为通用，可以构建多种类型的图形编辑应用

在选择这两个库时，应根据具体的项目需求来决定。如果重点在于处理和展现大量关系型数据，如网络分析或知识图谱，那么`G6` 可能是更合适的选择。如果项目需要开发一款图形编辑器（类似 Visio、draw.io ）的应用，`X6` 则更加适合。
![](https://img.up-4ever.site/20240302100449.png)
## 什么是 G6？
[G6](https://github.com/antvis/g6) 是一个图可视化引擎。它提供了图的绘制、布局、分析、交互、动画等图可视化的基础能力。旨在让关系变得透明，简单。
G6 作为一款专业的图可视化引擎，具有以下特性：

- **优秀的性能：**支持大规模图数据的交互与探索；
- **丰富的元素：**内置丰富的节点与边元素，自由配置，支持自定义；
- **可控的交互：**内置 10+ 交互行为，支持自定义交互；
- **强大的布局：**内置了 10+ 常用的图布局，支持自定义布局；
- **便捷的组件：**优化内置组件功能及性能；
- **友好的体验：**根据用户需求分层梳理文档，支持 TypeScript 类型推断。

除了默认好用、配置自由的内置功能，元素、边、交互、布局均具有高可扩展的自定义机制。
![](https://img.up-4ever.site/20240302100546.png)
## 什么是『图（Graph）』？
中文字“图”在大家的传统认知里指的是图画、图像，而图论与可视化中的“图”—— Graph 则有着更精确的定位：主体（objects）与关系（relationships）的组成。
> 主体：指的是节点，它们代表实体或对象。
> 关系：关系在图中由边（连接线）表示，表示两个节点之间的某种联系或互动。

图(Graph)是一种比线性表和树更为复杂的数据结构。
线性结构：是研究数据元素之间的一对一关系。在这种结构中，除第一个和最后一个元素外，任何一个元素都有唯一的一个直接前驱和直接后继。
树结构：是研究数据元素之间的一对多的关系。在这种结构中，每个元素对下(层)可以有0个或多个元素相联系，对上(层)只有唯一的一个元素相关，数据元素之间有明显的**层次**关系。
图结构：是研究数据元素之间的多对多的关系。在这种结构中，任意两个元素之间可能存在关系。即结点之间的关系可以是任意的，**图中任意元素之间都可能相关**。
> 应用分组之间的关系就是这样一种图关系。

## 什么是『图元素』？
图的元素（Item）包含图上的**节点 Node** 、**边 Edge** 和 **Combo（节点分组）** 三大类。
### 节点
#### 节点通用属性
![](https://img.up-4ever.site/20240302100617.png)
#### 内置节点
> **具体配置可点击链接，在 G6 官网查看。**

- [**circle 圆形**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/circle)
- [**rect 矩形**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/rect)
- [**ellipse 椭圆**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/ellipse)
- [**diamond 菱形**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/diamond)
- [**triangle 三角形**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/triangle)
- [**star 星形**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/star)
- [**image 图片**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/image)
- [**modelRect 卡片**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/model-rect)
- [**donut 圆环**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/built-in/donut)
#### 自定义节点
若内置节点无法满足需求，还可以通过 **G6.registerNode** 进行自定义节点，方便开发更加定制化的节点，包括含有复杂图形的节点、复杂交互的节点、带有动画的节点等。
由于[**自定义节点**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/custom-node)的复杂度较高，因此官方先后推出了 [**JSX 方案**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/jsx-node) 和 [**React 方案**](https://g6.antv.antgroup.com/manual/middle/elements/nodes/react-node)** **（需要额外应用一个包：**@antv/g6-react-node**）来降低自定义节点的复杂度。本次实时拓扑图采用的是 React 方案，相对于 React 开发者来说此方案的上手复杂度较低，相比于原始的自定义节点的写法理解难度也有明显的下降。因此接下来的重点说明的也会是 React 方案。
##### 自定义节点
先了解下自定义节点 API：
```typescript
G6.registerNode(
  'nodeName',
  {
    options: {
      style: {},
      stateStyles: {
        hover: {},
        selected: {},
      },
    },
    /**
     * 绘制节点，包含文本
     * @param  {Object} cfg 节点的配置项
     * @param  {G.Group} group 图形分组，节点中图形对象的容器
     * @return {G.Shape} 返回一个绘制的图形作为 keyShape，通过 node.get('keyShape') 可以获取。
     * 关于 keyShape 可参考文档 核心概念-节点/边/Combo-图形 Shape 与 keyShape
     */
    draw(cfg, group) {},
    /**
     * 绘制后的附加操作，默认没有任何操作
     * @param  {Object} cfg 节点的配置项
     * @param  {G.Group} group 图形分组，节点中图形对象的容器
     */
    afterDraw(cfg, group) {},
    /**
     * 更新节点，包含文本
     * @override
     * @param  {Object} cfg 节点的配置项
     * @param  {Node} node 节点
     */
    update(cfg, node) {},
    /**
     * 更新节点后的操作，一般同 afterDraw 配合使用
     * @override
     * @param  {Object} cfg 节点的配置项
     * @param  {Node} node 节点
     */
    afterUpdate(cfg, node) {},
    /**
     * 响应节点的状态变化。
     * 在需要使用动画来响应状态变化时需要被复写，其他样式的响应参见下文提及的 [配置状态样式] 文档
     * @param  {String} name 状态名称
     * @param  {Object} value 状态值
     * @param  {Node} node 节点
     */
    setState(name, value, node) {},
    /**
     * 获取锚点（相关边的连入点）
     * @param  {Object} cfg 节点的配置项
     * @return {Array|null} 锚点（相关边的连入点）的数组,如果为 null，则没有控制点
     */
    getAnchorPoints(cfg) {},
  },
  // 继承内置节点类型的名字，例如基类 'single-node'，或 'circle', 'rect' 等
  // 当不指定该参数则代表不继承任何内置节点类型
  extendedNodeType,
);
```
##### jsx 自定义节点
在 G6 3.7.0 及以后的版本中，用户以使用类似 JSX 的语法来定义节点。只需要在使用 G6.registerNode 自定义节点时，将第二个参数设置为字符串或一个返回值为 string 的 function。
```typescript
G6.registerNode(
  'rect-xml',
  (cfg) => `
  <rect style={{
    width: 100, height: 20, fill: '#1890ff', stroke: '#1890ff', radius: [6, 6, 0, 0]
  }} keyshape="true" name="test">
    <text style={{ 
			marginTop: 2, 
			marginLeft: 50, 
      textAlign: 'center', 
      fontWeight: 'bold', 
      fill: '#fff' }} 
			name="title">${cfg.label || cfg.id}</text>
  </rect>
`,
);
```
对应节点样式：
![](https://img.up-4ever.site/20240302100700.png)
##### React 自定义节点
定义 React 组件节点的时候，你不能使用任何的 hook 或者异步获取的逻辑，因为目前节点绘制需要是一个同步的过程，并且，我们推荐把所有状态以及数据信息放在节点本身 data 中，这样可以更方便的进行管理。在React组件节点中，所有的数据流动都应该是：节点数据 -> react 组件 props(cfg) -> 节点内容变化。组件本身需要是没有任何副作用的，**所有对于节点数据的改变，都是基于 `graph.updateItem()` 来完成的**。
关于 React 方案自定义节点以下是需要读的文档：

- [https://dicegraph.github.io/g6-react-node/example/card](https://dicegraph.github.io/g6-react-node/example/card)
- [https://www.yuque.com/antv/g6-blog/osythv](https://www.yuque.com/antv/g6-blog/osythv)
- [https://g6.antv.antgroup.com/manual/middle/elements/nodes/react-node](https://g6.antv.antgroup.com/manual/middle/elements/nodes/react-node)
- 自定义节点的事件：[https://dicegraph.github.io/g6-react-node/example/event](https://dicegraph.github.io/g6-react-node/example/event)
- 自定义节点需要与外部组件通信：利用 Bus 总线机制，监听事件进行通信。比如：eventemitter3
```typescript
<Button
  onClick={() => { bus.emit(NODE_FOLD, data, id) }}
  color="#4976e4"
>
  收起节点
</Button>
```
```typescript
bus.on(NODE_FOLD, (data: INodeItemType['data'], id: string) => {
  console.log(data, id);
});
```
**注意事项：**

1. 写完自定义节点后，一定要记得**注册**
```typescript
G6.registerNode(NODE_NAME, {
  ...createNodeFromReact(Card),
  setState(name, value, item) {
    if (name === NodeStateEnum.SELECTED) {
      const { data } = item!.getModel();
      const group = item!.getContainer();
      const keyShape = group.get('children')[0];
      const headerRectShape = keyShape.get('children')[0];
      // 节点选中状态
      if (value) {
        headerRectShape.attr({
          shadowColor: '#BFBFBF',
          shadowBlur: 8,
          // @ts-ignore
          fill: data?.warningDetailInfo?.level === StatusTypeEnum.NORMAL ?
            '#F6FFED' :
            // @ts-ignore
            data?.warningDetailInfo?.level === StatusTypeEnum.ERROR ?
            '#FFF1F0' : '#FFFDE7',
          shadowOffsetX: 2,
          shadowOffsetY: 2,
        })
      } else {
        headerRectShape.attr({
          shadowColor: '#DDD',
          shadowBlur: 8,
          fill: '#FFF',
          shadowOffsetX: 2,
          shadowOffsetY: 2,
        })
      }
    }
  },
  getAnchorPoints() {
    return [
      [0, 0.5],
      [1, 0.5],
    ];
  },
});
```

2. createNodeFromReact 是 **@antv/g6-react-node **提供的一个内置方法，其本质就是将 React 组件转换为 **draw **方法，其他的方法还是可以继续自定义的，比如：** setState、getAnchorPoints等。**
3. 自定义的节点（或边）在文件中**引入方式：**
```typescript
import '@/pages/Topology/customNode'; // 直接在 G6 初始化的文件中引入即可
import '@/pages/Topology/customEdge';
```

4. 设置节点中的**渐变色**

  参考文档：[https://g6.antv.antgroup.com/manual/middle/elements/advanced-style/gradient](https://g6.antv.antgroup.com/manual/middle/elements/advanced-style/gradient)

5. 使用了事件后，需要使用函数 `appenAutoShapeListener(graph)`对所进行对图进行事件挂载才可以生效，该方法可以直接从 `@antv/g6-react-node` 包引出。

**总之，无论是哪种方案，其本质还是通过 G6.registerNode 注册自定义节点并使用。**
#### 使用自定义节点
在初始化 Graph 图实例时，将注册的节点名称（`NODE_NAME，一个字符串`）配置到 defaultNode 选项中即可。比如：
```typescript
const graph = new G6.Graph({
  container: containerRef.current,
  width: containerRef.current.scrollWidth,
  height: containerRef.current.scrollHeight - 100,
  defaultNode: {
    type: NODE_NAME, // 再次提醒：别忘了注册
  },
});
```
上面给出了默认节点的使用例子。G6 还提供了根据数据动态更新节点类型的方法，即修改节点数据的 **type** 字段，将处理好的节点数据传给 Graph 实例即可。
### 边
#### 边的通用属性
![](https://img.up-4ever.site/20240302100736.png)
#### 内置边
G6 提供了 9 种[内置边](https://g6.antv.antgroup.com/manual/middle/elements/edges/default-edge)：

- [**line**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/line)**：直线，不支持控制点；**
- [**polyline**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/polyline)**：折线，支持多个控制点；**
- [**arc**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/arc)**：圆弧线；**
- [**quadratic**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/quadratic)**：二阶贝塞尔曲线；**
- [**cubic**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/cubic)**：三阶贝塞尔曲线；**
- **cubic-vertical：垂直方向的三阶贝塞尔曲线，不考虑用户从外部传入的控制点；**
- **cubic-horizontal：水平方向的三阶贝塞尔曲线，不考虑用户从外部传入的控制点；**
- [**loop**](https://g6.antv.antgroup.com/manual/middle/elements/edges/built-in/loop)**：自环。**

这些内置边的默认样式分别如下图所示。
![](https://img.up-4ever.site/20240302100806.png)
#### 自定义边
不同于自定义节点，自定义边的方式比较单一，多通过内置边的继承以及对状态、动画、事件的自定义来实现复杂边的自定义工作。
建议参考下面文档：

- [https://g6.antv.antgroup.com/manual/middle/elements/edges/custom-edge](https://g6.antv.antgroup.com/manual/middle/elements/edges/custom-edge)

举个例子：
```typescript
// 基于 line 扩展出新的边
G6.registerEdge(
  'custom-edge',
  {
    // 响应状态变化
    setState(name, value, item) {
      const group = item.getContainer();
      const shape = group.get('children')[0]; // 顺序根据 draw 时确定
      if (name === 'active') {
        if (value) {
          shape.attr('stroke', 'red');
        } else {
          shape.attr('stroke', '#333');
        }
      }
      if (name === 'selected') {
        if (value) {
          shape.attr('lineWidth', 3);
        } else {
          shape.attr('lineWidth', 2);
        }
      }
    },
  },
  'line',
);

// 点击时选中，再点击时取消
graph.on('edge:click', (ev) => {
  const edge = ev.item;
  graph.setItemState(edge, 'selected', !edge.hasState('selected')); // 切换选中
});

graph.on('edge:mouseenter', (ev) => {
  const edge = ev.item;
  graph.setItemState(edge, 'active', true);
});

graph.on('edge:mouseleave', (ev) => {
  const edge = ev.item;
  graph.setItemState(edge, 'active', false);
});
```
上面的例子实现的效果为：

- 点击边时边变粗，再点击变成细；
- 鼠标移动上去变成红色，离开变成 '#333' 。
#### 使用自定义边
在初始化 Graph 图实例时，将注册的自定义边名称（`EDGE_NAME，一个字符串`）配置到 defaultEdge 选项中即可。比如：
```typescript
const graph = new G6.Graph({
  container: containerRef.current,
  width: containerRef.current.scrollWidth,
  height: containerRef.current.scrollHeight - 100,
  defaultEdge: {
    type: EDGE_NAME,
    style: {
      endArrow: true,
      stroke: LINE_COLOR,
      lineAppendWidth: 15,
    },
  },
});
```
G6 还提供了根据数据动态更新边类型的方法，即修改边数据的 **type** 字段，将处理好的边数据传给 Graph 实例即可，比如：
```typescript
export const processData = (edges: ICustomEdge[], nodes: ICustomNode[]) => {
  const edgeMap = new Map();
  // 统计两点之间边数大于 1 个的集合
  edges?.forEach((edge) => {
    const {source, target} = edge;
    if (edgeMap.has(`${source}${target}`)) {
      edgeMap.set(`${source}${target}`, edgeMap.get(`${source}${target}`) + 1);
    } else if(edgeMap.has(`${target}${source}`)) {
      edgeMap.set(`${source}${target}`, edgeMap.get(`${target}${source}`) + 1);
    } else {
      edgeMap.set(`${source}${target}`, 1);
    };
  });
  edges?.forEach((edge) => {
    const edgeData = edge.data;
    edge.style = {
      lineWidth: 2,
    };
    edge.label = `qps=${edgeData.qps}\nrt=${edgeData.rt}`;
    edge.labelCfg = labelCfg;
    // 针对两点之间边数大于 1 的情况，使用 quadratic 并处理重叠边
    if (edgeMap.get(`${edge.source}${edge.target}`) > 1 || edgeMap.get(`${edge.target}${edge.source}`) > 1) {
      edge.type = EDGE_QUADRATIC_NAME; // 二阶贝塞尔曲线
      edge.curveOffset = 50;
    };
    // 和上面的判断条件顺序不可调换
    if (edge.source === edge.target) {
      edge.type = EDGE_LOOP_NAME; // loop
      edge.loopCfg = {
        dist: 100,
        position: edgeData.type === NodeTypeEnum.MSG ? 'top' : 'bottom',
      };
    };
    // 消息类服务为虚线
    if (edgeData.type === NodeTypeEnum.MSG) {
      edge.style.lineDash = [2, 4];
    };
  });
};
```
这样处理后数据会表现为：

- source 和 target 一样的节点连线，边的类型为 `EDGE_LOOP_NAME`类型。
- source 和 target 不一样的节点，但是存在多条边的类型为 `EDGE_QUADRATIC_NAME`类型。
- 其他情况，还是会默认为 **defaultEdge** 中配置的 `EDGE_NAME`类型。
##### 常见问题

- 两个节点之间存在多个边，多个边是重叠的，解决办法可参考：[https://g6.antv.antgroup.com/manual/middle/elements/methods/multi-line](https://g6.antv.antgroup.com/manual/middle/elements/methods/multi-line)
## 图布局
图布局是指图中节点的排布方式，根据图的数据结构不同，布局可以分为两类：一般图布局、树图布局。我们在本文档只讨论**一般图布局**。和自定义节点、自定义边一样，除了内置布局方法外，一般图布局还支持 [自定义布局](https://g6.antv.antgroup.com/zh/docs/manual/middle/layout/custom-layout) 机制。
G6 的布局是自由的，内置布局算法仅仅是操作了数据中节点的 x 和 y 值。因此，除了使用内置布局以及自定义的一般图布局外，用户还可以使用外部图布局算法，计算节点位置后赋值到数据中节点的 x 和 y 字段上，G6 便可以根据该位置信息进行绘制。
本次需求中未使用自定义布局，并不是 G6 提供的内置图布局完全满足我们的需求，而是自定义图布局更多的是依赖图布局算法，面对简单的情况，图布局算法相对简单，但是本次需求的节点较多且情况复杂。在这种情况下完成一个图布局算法是一个非常大的工作量，需要借鉴相关的算法文章，成本相对较高，因此采用了 G6 内置的 [Dagre Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#dagre)（层次布局）。
### 内置布局

- [Random Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#random)：随机布局；
- [Force2 Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#force2)：G6 4.7.0 后支持力导向布局，与 gForce 相比性能更强；
- [GForce Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#gForce)：G6 4.0 支持的经典力导向布局，支持 GPU 并行计算；
- [Force Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#force)：引用 d3 的经典力导向布局；
- [Fruchterman Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#fruchterman)：Fruchterman 布局，一种力导布局；
- [Circular Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#circular)：环形布局；
- [Radial Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#radial)：辐射状布局；
- [MDS Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#mds)：高维数据降维算法布局；
- [Dagre Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#dagre)：层次布局；
- [Concentric Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#concentric)：同心圆布局；
- [Grid Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#grid)：网格布局；
- [Combo Force Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#combo-force)：*V3.5 新增。*适用于带有 combo 图的力导向布局，推荐有 combo 的图使用该布局。
- [Combo Combined Layout](https://g6.antv.antgroup.com/manual/middle/layout/graph-layout#combo-combined)：*V4.6 新增。*适用于带有 combo 的图，可自由组合内外布局，默认情况下可以有较好的效果，推荐有 combo 的图使用该布局。
## 事件
G6 内置的事件可参考：[https://g6.antv.antgroup.com/api/event](https://g6.antv.antgroup.com/api/event)，这里涵盖了 G6 内部所有的事件（或者说是钩子），比如：
![](https://img.up-4ever.site/20240302100852.png)
## 动画
G6 中的动画分为两个层次：

- **全局动画**：全局性的动画，图整体变化时的动画过渡；
- **元素（边和节点）动画**：节点或边上的独立动画。
### 全局动画
G6 的全局动画指通过图实例进行某些全局操作时，产生的动画效果。例如：

- graph.updateLayout(cfg) 布局的变化
- graph.changeData() 数据的变化

通过实例化图时配置 `animate: true`，可以达到每次进行上述操作时，动画效果变化的目的。配合 animateCfg 配置动画参数，animateCfg 具体配置参见 [animateCfg](https://g6.antv.antgroup.com/manual/middle/animation#animateCfg)
```typescript
const graph = new G6.Graph({
  // ...                   // 图的其他配置项
  animate: true, // Boolean，切换布局时是否使用动画过度，默认为 false
  animateCfg: {
    duration: 500, // Number，一次动画的时长
    easing: 'linearEasing', // String，动画函数
  },
});
```
### 元素动画
这一部分我强烈推荐有需求的同学看下 G6 官网提供的[元素动画](https://g6.antv.antgroup.com/manual/middle/animation#%E5%85%83%E7%B4%A0%E5%8A%A8%E7%94%BB)这一小结内容，这一部分是把图做精细化跳不过去的一步。
下面是一个边动画的例子：
> Line 47 和 Line 98 都是在相应的元素上增加动画效果。

```typescript
const setState = (name?: string, value?: string | boolean, item?: Item) => {
  const group = item!.getContainer();
  const model = item!.getModel();
  const { data: targetNodeData } = (item as IEdge).getTarget().getModel() as INodeItemType;
  // const isNormalNode = targetNodeData?.warningDetailInfo?.level === StatusTypeEnum.NORMAL;
  const originLineWidth = (model.style?.lineWidth || 2) as number;
  const isReal = !Boolean(model.style?.lineDash);
  const shape = group.find((ele) => ele.get('name') === 'edge-shape');
  const back = group.find((ele) => ele.get('name') === 'back-line');
  const breatheShape = group.find((ele) => ele.get('name') === 'breathe-shape');
  if (name === EdgeStateEnum.SELECTED) {
    if (back) {
      back.stopAnimate();
      back.remove();
      back.destroy();
    };
    if (breatheShape) {
      breatheShape.stopAnimate();
      breatheShape.remove();
      breatheShape.destroy();
    };
    const { path, lineDash } = shape.attr();
    if (value) {
      shape.attr({
        // stroke: isNormalNode ? LINE_SELECTED_COLOR : LINE_ERROR_SELECTED_COLOR ,
        stroke: LINE_SELECTED_COLOR,
        lineWidth: originLineWidth + 1,
      });
      if (isReal) {
        const back = group.addShape('path', {
          attrs: {
            path,
            stroke: LINE_SELECTED_COLOR,
            lineDash,
            lineWidth: originLineWidth + 2,
            opacity: 0.3,
            shadowColor: '#000',
            shadowBlur: 6,
            shadowOffsetX: 0,
            shadowOffsetY: 4,
          },
          name: 'back-line',
        });
        back.toBack();
        shape.animate(
          (ratio: number) => {
            // @ts-ignore
            const length = shape.getTotalLength();
            const startLen = ratio * length;
            return {
              lineDash: [startLen, length - startLen],
            };
          },
          {
            repeat: true,
            duration: 2000,
          },
        );
      } else {
        let index = 0;
        const lineDash = shape.attr('lineDash');
        const totalLength = lineDash[0] + lineDash[1];
        const breatheShape = group.addShape('path', {
          attrs: {
            path,
            stroke: '#E6F9EF',
            shadowColor: '#000',
            shadowBlur: 20,
            shadowOffsetX: 0,
            shadowOffsetY: 6,
            strokeOpacity: 0.5,
            strokeWidth: 5,
            lineWidth: originLineWidth + 2,
          },
          name: 'breathe-shape',
        });
        breatheShape.animate(
          {
            strokeOpacity: 0.3,
          },
          {
            duration: 2000,
            repeat: true,
            callback: () => {
              breatheShape.animate(
                {
                  strokeOpacity: 0.5,
                },
                {
                  duration: 2000,
                  repeat: true,
                }
              );
            },
          }
        );
        breatheShape.toBack();
        shape.animate(
          () => {
            index++;
            if (index > totalLength) {
              index = 0;
            }
            const res = {
              lineDash,
              lineDashOffset: -index,
            };
            return res;
          },
          {
            repeat: true,
            duration: 2000,
          },
        );
      }
    } else {
      shape.stopAnimate();
      shape.attr({
        // stroke: isNormalNode ? LINE_COLOR : LINE_ERROR_COLOR,
        stroke: LINE_COLOR,
        lineDash: model.style?.lineDash,
        lineWidth: originLineWidth,
      })
    }
  }
  if (name === EdgeStateEnum.HOVER) {
    if (value) {
      shape.attr({
        // stroke: isNormalNode ? LINE_HOVER_COLOR : LINE_ERROR_HOVER_COLOR,
        stroke: LINE_HOVER_COLOR,
        lineWidth: originLineWidth + 1,
      });
    } else {
      const edgeStates = item!.getStates();
      shape.attr({
        // stroke: isNormalNode ?
        //   (edgeStates?.includes(EdgeStateEnum.SELECTED) ? LINE_SELECTED_COLOR : LINE_COLOR) :
        //   LINE_ERROR_COLOR,
        stroke: edgeStates?.includes(EdgeStateEnum.SELECTED) ? LINE_SELECTED_COLOR : LINE_COLOR,
        lineWidth: edgeStates?.includes(EdgeStateEnum.SELECTED) ? originLineWidth + 1 : originLineWidth,
      });
    };
  };
};
```
## 插件
G6 提供了一些可插拔的组件，比如

- [网格插件](https://g6.antv.antgroup.com/api/plugins#grid)：Grid，在画布上绘制了网格。

![](https://img.up-4ever.site/20240302100938.png)

- [Menu 插件](https://g6.antv.antgroup.com/api/plugins#menu)：用于配置节点上的右键菜单。
- ...

其他更多的插件可以参考：[https://g6.antv.antgroup.com/api/plugins](https://g6.antv.antgroup.com/api/plugins)
## 常用 API

- **graph.read(data):** 接收数据，并进行渲染
- **graph.changeData(data, stack):** 更新数据源，根据新的数据**重新渲染视图（节点位置、缩放会被重置）**
- **graph.updateItem(item, model, stack):** 更新元素，包括更新数据、样式等
- **graph.getNodes():** 获取图中所有节点的实例
- **node.getID():** 获取节点 ID
- **node.getModel()**: 获取节点的数据模型
- **graph.getEdges(): **获取图中所有边的实例
- **edge.getSource(): **获取边的 source 节点
- **edge.getTarget(): **获取边的 target 节点
- **edge.getModel():** 获取边的数据模型
- **graph.findById(id):** 根据 ID，查询对应的元素实例
- **graph.setItemState(item, state, value):** 设置元素状态
- **graph.clearItemStates(item, states):** 清除元素状态，可以一次性清除多个状态
## 其他

- `linkPoints`区分于 `anchorPoints`： `anchorPoints` 是真正用于指定该节点相关边的连入位置的「**数组**」，见 [anchorPoints](https://g6.antv.antgroup.com/zh/docs/manual/middle/elements/nodes/anchorpoint)；而 `linkPoints` 仅是指定是否「**绘制**」出四个圆点，**不起实际的连接相关边的作用**。二者常常配合使用。
- **坐标转换**：[https://g6.antv.antgroup.com/api/graph-func/coordinate](https://g6.antv.antgroup.com/api/graph-func/coordinate)
## 参考文章

- [Hello World 图可视化 · 语雀](https://www.yuque.com/antv/g6-blog/rg0xxs)
- [https://zhuanlan.zhihu.com/p/35864291](https://zhuanlan.zhihu.com/p/35864291)
- [G6](https://g6.antv.antgroup.com/manual/middle/elements/overview)
- [https://dicegraph.github.io/g6-react-node/example/card](https://dicegraph.github.io/g6-react-node/example/card)
- [https://www.yuque.com/antv/g6-blog/osythv](https://www.yuque.com/antv/g6-blog/osythv)
- [https://g6.antv.antgroup.com/manual/middle/elements/nodes/react-node](https://g6.antv.antgroup.com/manual/middle/elements/nodes/react-node)
- [https://dicegraph.github.io/g6-react-node/example/event](https://dicegraph.github.io/g6-react-node/example/event)
- [https://g6.antv.antgroup.com/manual/middle/elements/methods/multi-line](https://g6.antv.antgroup.com/manual/middle/elements/methods/multi-line)
- [https://g6.antv.antgroup.com/manual/middle/elements/advanced-style/gradient](https://g6.antv.antgroup.com/manual/middle/elements/advanced-style/gradient)
