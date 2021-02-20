[toc]

### 为什么重写equals的同时还要重写hashCode？

保证equal判断相等的同时，hashCode的值也一定要相等。

场景举例：在HashMap、HashSet中插入对象，需要先比较对象的hashcode值从而找到key，如果value存在值，再用equals判断相等。