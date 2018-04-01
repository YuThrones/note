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
3. `Uptate`, `LateUpdate` 和 `FixedUpdate` 更新逻辑

#### 逻辑更新顺序
1. 场景启动时调用所有的 `Awake()`
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
