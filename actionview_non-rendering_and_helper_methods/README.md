# Action View 非渲染 & 非 helper 方法

下面这些方法，和渲染没有直接关联，并且严格意义上讲不属于 helper 方法。

- Base

- Routing Url For

- Record Identifier

- Railtie

- Layouts

- Digestor<br>
对视图进行加密，是片段缓存的组成部分。

- ~~Dependency Tracker~~<br>
供 Digestor 使用，digest 的时候可以以数组的形式传 dependencies 作为其可选参数。
