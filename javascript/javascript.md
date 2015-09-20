Promise

原生的 Promise 实现，不再需要 bluebird 或 Q+。

Generator

Generator 生成器允许你通过写一个可以保存自己状态的的简单函数来定义一个迭代算法。和 Generator 一起出现的通常还有 yield。

Generator 是一种可以停止并在之后重新进入的函数。生成器的环境（绑定的变量）会在每次执行后被保存，下次进入时可继续使用。generator 字面上是“生成器”的意思，在 ES6 里是迭代器生成器，用于生成一个迭代器对象。