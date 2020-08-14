# 惰性(lazy) vs 严格(strict)

一个计算可以是惰性的，也可以是严格的。在给出下面的表格之前，我们先举一个实际的例子。

这就是“严格计算(Strict evaluation)”。你的父母让你立即去做一些家务（如洗碗等）。你就去做了。有时，你了解到这是必要的。另一些时候，你得知碗筷已经被别人洗过了。尽管告诉你的父母不需要洗，但你的父母还是坚持要洗，并且凌驾于你之上。尤其是在“洗碗”会花很长时间的日子里，这让你很恼火。

这就是“懒惰计算(Lazy evaluation)”。你的父母让你记得做一些家务事，但在他们告诉你之前，不要开始做。在某些日子里，他们从不告诉你开始，因为这个任务毕竟不需要。你喜欢那些日子。在其他日子里，他们告诉你在上午、下午或晚上开始。

| 术语 | 定义 | 优点 | 缺点
| - | - | - | - |
| 严格(Strict) | 立即计算其结果 | 可以在最理想的时间进行昂贵的计算 | 在存储/评估不需要/不使用的昂贵计算上浪费 CPU 周期和内存 |
| 惰性(Lazy) | 推迟计算，直到需要时才进行 | 节省 CPU 周期和内存：不需要/不使用的计算永远不会被计算 | 当每次都要进行计算时，会增加不必要的开销

为了让某件事情变得更懒，我们把它变成一个函数。这个函数接受一个参数（`Unit`），然后返回我们想要的值。这就是所谓的 `thunk`：一个我们知道如何做但还没有执行的计算。为了运行存储在 `thunk` 中的代码，我们使用了 `forcing the thunk` 这个短语。

```purescript
-- Given an Int, I can return another Int
strictlyCompute :: Int -> Int
strictlyCompute x = x + 4

-- otherwise known as 'thunking'
-- Given an Int, I can return a 'thunk.' When
-- this thunk is evaluated, it will return an Int.
lazilyCompute :: Int -> (Unit -> Int)
lazilyCompute x = (\unitValue__neverUsed -> x + 4)

forceThunk :: (Unit -> Int) -> Int
forceThunk thunk = thunk unit

-- somewhere in our code
thunk = lazilyCompute 5

-- somewhere else in our code, when we finally need it
result = forceThunk thunk
```
