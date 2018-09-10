#### MobX 会对什么作出反应?

MobX 会对在**追踪函数**执行**过程**中**读取**现存的可观察属性做出反应。 

- **“读取”** 是对象属性的间接引用，可以用过 `.` (例如 `user.name`) 或者 `[]` (例如 `user['name']`) 的形式完成。
- **“追踪函数”** 是 `computed` 表达式、observer 组件的 `render()` 方法和 `when`、`reaction` 和 `autorun` 的第一个入参函数。
- **“过程(during)”** 意味着只追踪那些在函数执行时被读取的 observable 。这些值是否由追踪函数直接或间接使用并不重要。

换句话说，MobX 不会对其作出反应:

- 从 observable 获取的值，但是在追踪函数之外

- 在异步调用的代码块中读取的 observable

  

####  MobX 追踪属性访问，而不是值

用一个示例来阐述上述规则，假设你有如下的observable数据结构（默认情况下observable会递归应用，所以本示例中的所有字段都是可观察的）

```
let message = observable({
    title: "Foo",
    author: {
        name: "Michel"
    },
    likes: [
        "John", "Sara"
    ]
})
```

在内存中看起来像下面这样。 绿色框表示**可观察**属性。 请注意，**值** 本身是不可观察的！ ![MobX reacts to changing references](https://cn.mobx.js.org/images/observed-refs.png)