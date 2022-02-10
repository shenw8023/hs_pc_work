- 单阶段联合抽取？
- 暴漏偏差：指在训练阶段是gold实体输入进行关系预测，而在推断阶段是上一步的预测实体输入进行关系判断；导致训练和推断存在不一致。
   #todo 跟transformer的mask关联


- 联合抽取两种范式：
	- 多任务学习：即实体和关系任务共享同一个编码器，但通常会依赖先后的抽取顺序：关系判别通常需要依赖实体抽取结果。这种方式会存在暴漏偏差，会导致误差积累。
	- 结构化预测：即统一为全局优化问题进行联合解码，只需要一个阶段解码，解决暴漏偏差。


- 跟以前看的一篇指针网络联合抽取的方式有点类似