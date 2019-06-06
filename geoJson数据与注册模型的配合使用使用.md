# geoJson数据与注册模型的配合使用使用 - 2019-06-06



geojson是一种对各种地理数据结构进行编码的格式，基于Javascript对象表示法的地理空间信息数据交换格式。

通俗来讲就是相关的地理信息,包括地形地貌,边界,建筑分布,高度等一系列相关数据,以JSON形式存入,以建筑geojson数据为例:

```javascript
{"type":"Feature","geometry":{"type":"Polygon","coordinates":[[[117.90308105700001,24.53265697430004],[117.90294156200014,24.532657010300056],[117.90293082900007,24.532686297900113],[117.902705491,24.53265706939999],[117.90246941800001,24.532657125900016],[117.90246941500004,24.532696172400108],[117.90258745100004,24.53271566760003],[117.90258744599998,24.53278399880014],[117.90289862899999,24.532822968500113],[117.90290936300005,24.532764396200037],[117.90307031800012,24.532774116299947],[117.90308105700001,24.53265697430004]]]},"properties":{"Id":0,"Floor":3}},
```

上示数据中,一条json便是一个建筑的数据,包括俯视投影集合多边形的每一个顶点坐标,Floor就是楼层,3就是三楼,在实际操作中可以一层表示三米,直接3*3具现化模型高度

得益于HT的建模功能API,可以直接调用 ht.Default.setShape3dModel() 和 ht.Default.createExtrusionModel() 方法

将经过处理后的数据一数组形式放进去,之后 new ht.Node() 生成创建模型

代码示例如下:

```javascript
for (var items in pointArray) {
    var housePosArray = []
    var numX = 0
    var numY = 0
    for (var k = 0; k < pointArray[items].length; k++) {
        for (var k2 = 0; k2 < 2; k2++) {
            housePosArray.push(pointArray[items][k][k2] * 1)
        }
    }
    // console.log(housePosArray)
    for (var m = 0; m < housePosArray.length; m++) {
        if (m % 2 === 0) {
            housePosArray[m] = 110000 - (119 - housePosArray[m]) * 50000
        }
        else {
            housePosArray[m] = 50000 - (25 - housePosArray[m]) * 50000
        }
    }
    // console.log(housePosArray)
    // console.log(items)
    for (var nums = 0; nums < housePosArray.length; nums++) {
        if (nums % 2 === 0) {
            numX += housePosArray[nums]
        }
        else {
            numY += housePosArray[nums]
        }
    }

    	// console.log(items)
        ht.Default.setShape3dModel('houses' + items, ht.Default.createExtrusionModel(housePosArray, [], true, true, 4))

        createNode('houses' + items, [numX / (housePosArray.length / 2) - 115200, items.split('-')[0] * 1.5, numY / (housePosArray.length / 2) - 51200], [1, items.split('-')[0] * 3, 1]).setTag('house' + items)
        var housess = dataModel.getDataByTag('house' + items)
	}
}
    function createNode(shape, p3, s3, color) {
        var node = new ht.Node();
        node.s({
            'shape3d': shape,
            'shape3d.color': color,
            // 'shape3d.reverse.flip': true
        });
        node.setPosition3d(p3);
        node.setSize3d(s3);
        dataModel.add(node);
        return node;
    }
```

操作核心就是数据摘取和性能优化,这个就是经验积累了

效果图下图:

![1559810562767](C:\Users\14753\AppData\Roaming\Typora\typora-user-images\1559810562767.png)

![1559810581227](C:\Users\14753\AppData\Roaming\Typora\typora-user-images\1559810581227.png)

