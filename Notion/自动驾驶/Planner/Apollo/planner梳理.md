
场景切换
场景更新切换在 `apollo::planning::ScenarioManager::Update` 中进行

#### reference line

参考线是planning规划算法的基础，ReferenceLineProvider根据车辆的实时位置，计算车辆前后一定范围内（几百米）的参考线信息，相对于全局路由线路来说，参考线是局部路线信息，但参考线中还附加了车辆周围的动态信息，如障碍物，交通灯等。

参考线相关重要的两个数据：

- **ReferenceLine**: 据全局路由线路生成的原始路径信息，包含地图上道路之间的静态关系，并且经过平滑之后的结果。
- **ReferenceLineInfo**: `ReferenceLine`的基础上添加了动态信息，如决策信息，ST图等，planning的规划操作基本都在这个数据结 构上进行。可以建立理解为ReferenceLine提供的是轨迹信息，而ReferenceLineInfo在ReferenceLine的基础上新添加了决策信息。