pnc_map在Apollo中属于相对独立的一块内容，作为hd_map与planning的中间层，在生成reference_line即规划参考线的时候会使用到。换句话说，pnc_map就是解析routing结果，再由每个路由段转换为reference_line的数据形式，这是它的主要功能。实现这一模块的目的也是为了让规划模块的独立性更高，避免与高精地图的数据格式所耦合，导致可迁移性变差。

[Apollo 6.0 pnc_map解析 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/419350318)
