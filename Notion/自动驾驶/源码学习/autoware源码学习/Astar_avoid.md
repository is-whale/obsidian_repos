  

该函数用于获取给定路径点和车辆当前位置的局部最接近点坐标。根据给定的搜索区域大小，在给定路径点中搜索局部最接近点。如果最接近的点尚未找到，则在所有路径点中搜索。找到最接近的点后，返回其索引。

用于判断是否到达目标点

```JavaScript
int AstarAvoid::getLocalClosestWaypoint(const autoware_msgs::Lane &waypoints, const geometry_msgs::Pose &pose, const int &search_size)
{
  static autoware_msgs::Lane local_waypoints; // around self-vehicle
  const int prev_index = closest_local_index_;

  // search in all waypoints if lane_select judges you're not on waypoints
  if (closest_local_index_ == -1)
  {
    closest_local_index_ = getClosestWaypoint(waypoints, pose);
  }
  // search in limited area based on prev_index
  else
  {
    // get neighborhood waypoints around prev_index
    int start_index = std::max(0, prev_index - search_size / 2);
    int end_index = std::min(prev_index + search_size / 2, (int)waypoints.waypoints.size());
    auto start_itr = waypoints.waypoints.begin() + start_index;
    auto end_itr = waypoints.waypoints.begin() + end_index;
    local_waypoints.waypoints = std::vector<autoware_msgs::Waypoint>(start_itr, end_itr);

    // get closest waypoint in neighborhood waypoints
    closest_local_index_ = start_index + getClosestWaypoint(local_waypoints, pose);
  }

  return closest_local_index_;
}
```