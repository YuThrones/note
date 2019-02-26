# Unity笔记

## Unity基础

### 界面

* 使用shift加方向键可以加速拖动
* 必须具有 `Mesh Render` 才可以在场景中渲染显示.

### Prfabs
* 把Unity中一个物体拖动到project中，可以形成一个Prfabs中，相当于一个模板，主要作用有两个：1.重复利用  2. 同时修改
* 只需要把保存好的Prfabs拖到场景中即可使用，来自Prfabs的物体字体是蓝色的。选中物体后点击 `select` 可以同时修改场景中所有来自同一Prafabs的物体，选中 `revert` 可以恢复修改， 点击 `apply` 可以保存修改。

### 资源管理
* Unity会把场景中使用的图片打包到游戏中，如果是代码中使用的需要自己进行管理。

### 脚本基础
* Unity脚本可以使用`C#` 和 `UnityScript`。
* 凡是能挂在`gameobject`上面的都是 `Component`。
* `Script` 也可以作为一个 `Component`。 
* `Script` 想要挂在 `GameObject` 上面必须继承 `MonoBehaviour` 。

#### 脚本驱动游戏
1. `Instantiate()` 创建 `GameObject`。
2. 通过 `Awakte()` 和 `Start()` 来做初始化。
3. `Update`, `LateUpdate` 和 `FixedUpdate` 更新逻辑

#### 逻辑更新顺序
1. 场景启动时调用所有的 `Awake()`，要注意这个函数在挂的物体如果初始为隐藏的话是不会执行的，等到被设置为可见的时候才执行。
2. 调用所有脚本的 `Start()`
3. 调用 `Update()`（每帧一次）
4. 调用 `LateUpdate()`（每帧一次）
5. 调用 `FixedUpdate()`（根据时间决定调用次数）

#### 对象销毁
1. 调用 `Destroy` 销毁 `GameObject`
2. 销毁对象时调用 `OnDestroy`


#### 脚本间的通信
1. 通过 `GetComponent` 来找到其他脚本
2. 通过 `GameObject.Find` 来找到其他物体

#### 游戏输入
1. 在 `Edit->Project Settings->Input` 设置游戏输入
2. 在脚本中利用 `Input` 类来检测输入状态
3. `Input.GetAxis` 返回的值是 -1 到 1 之间，0表示没有输入

### 动态生成物体
1. 把一个`prefab`做成模板，运行时用 `Instantiate` 生成一个实例，不需要的时候使用 `Destroy` 将 `GameObject` 销毁。

### 碰撞检测
1. 使用 `Collider` 组件来实现碰撞检测，类型分很多种，其中 `MeshCollider` 是根据物体形状生成碰撞区域的，效果最好，也最消耗性能，平时用一般的即可。
2. 勾选 `IsOnTrigger` 会触发碰撞信息，一共有三种，分别是`OnTriggerEnter`, `OnTriggerStay`, `OnTriggerExit`。
3. 如果给 `Collider` 加上 `Rigidbod`, 则不会触发 `OnTrigger` 事件，转而触发 `OnCollision`事件。
4. 使用了 `Rigidbod`的组件会受到物理系统的管理，其中有很多参数可以调整。 `Mass` 是物体的质量，`Drag` 是运动时受到的阻力，`Angular Drag` 是角速度的阻力，即转圈时的阻力， `Is Kinematic`可以把控制权暂时交给脚本。
5. 可以通过设置不同的 `tag` 来区分 `Collider` 。
6. Unity 使用 `NavMeshAgent` 类帮我们寻找路径。
7. Unity会为静态对象设置缓存，但是在移动、旋转、缩放的时候缓存会重新计算。被设置刚体的对象被视为动态对象。

### Unity中的自动寻路
1. 设置导航网络
2. 设置 `Nav Mesh Agent`
3. 调用 `Nav Mesh Agent` 的方法设置路径。

### 改变Image的显示内容

* 可以使用 `Texture2d` 生成一份纹理，然后使用 `sprite.Create` 函数生成一个 `sprite` ，然后替换 `Image` 的 `sprite`，示例代码如下
  ```c#
  Texture2D texture = (Texture2D)AssetDatabase.LoadAssetAtPath<Texture2D>(string.Format(IconFormat, Icon.Data[icon].icon));
  PlayerHeadIcon.sprite = Sprite.Create(texture, new Rect(0, 0, texture.width, texture.height), new Vector2(0.5f, 0.5f), 100);
  ```

### UI

* UI使用 `SetParent` 函数的时候要注意，可能会导致子控件的 `transform` 数据适配世界改变，为了使用 `prefab` 中设置好的数据，调用 `SetParent` 的时候要给第二个参数 `worldPositionStays` 传 `false` ，这样就不会缩放、位置等乱掉的情况。