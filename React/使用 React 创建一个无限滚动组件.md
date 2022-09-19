# 使用 React 创建一个无限滚动组件

无限滚动内容是处理大量数据的常用方法。它使您能够仅加载数据的一小部分，节省带宽并避免大列表带来的渲染速度慢的问题。

在本文中，我们将创建一个 `InfiniteScroll` 组件。

## 设定边界

虽然 `InfiniteScroll` 组件看起来是一项艰巨的任务，但事实证明，组件本身并不需要做很多事情，我们只需要非常清楚它的职责即可。一些现有的解决方案可能会让组件做更多的事情，但我认为最好让组件专门做以下任务：

- 检测用户何时滚动到页面底部
- 当检测到页面底部时，调用父级提供的函数
- 不要冗余地调用该函数（即每次触底只调用一次）
- 如果父函数说我们已经完成了（即没有更多的数据），则不调用该函数
- 在安装阶段进行初始数据加载（如果用户不希望，也可以不加载）

您可能想知道为什么组件不提供分页，以及它将如何渲染提供的数据。我的想法是，为了使这个组件尽可能灵活，它不应该知道任何关于分页的信息（这将对我们如何获取数据做出很大的假设），它也不应该为其子组件提供任何渲染逻辑。这只是一个辅助组件，以促进我们要进行的交互。

## 定义组件接口

我们可以使用前面提到的列表来定义组件的接口。

```tsx
type Props = {
  onBottomHit: () => void
  isLoading: boolean
  hasMoreData: boolean
  loadOnMount: boolean
}

const InfiniteScroll: React.FC<Props> = ({
  onBottomHit,
  isLoading,
  hasMoreData,
  children,
  loadOnMount
}) => {}
```

## 填充组件

我们可以通过向窗口 `scroll` 事件添加滚动处理程序来检测触底。我们还需要知道可滚动内容的位置。为了了解我们的内容在哪里，我们可以使用 React ref！

我将把这些放在一起，并解释我们在编写代码之后所做的工作。

```tsx
type Props = {
  onBottomHit: () => void
  isLoading: boolean
  hasMoreData: boolean
  loadOnMount: boolean
}

function isBottom(ref: React.RefObject<HTMLDivElement>) {
  if (!ref.current) {
    return false
  }
  return ref.current.getBoundingClientRect().bottom <= window.innerHeight
}

const InfiniteScroll: React.FC<Props> = ({
  onBottomHit,
  isLoading,
  hasMoreData,
  loadOnMount,
  children
}) => {
  const [initialLoad, setInitialLoad] = useState(true)
  const contentRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (loadOnMount && initialLoad) {
      onBottomHit()
      setInitialLoad(false)
    }
  }, [onBottomHit, loadOnMount, initialLoad])

  useEffect(() => {
    const onScroll = () => {
      if (!isLoading && hasMoreData && isBottom(contentRef)) {
        onBottomHit()
      }
    }
    document.addEventListener('scroll', onScroll)
    return () => document.removeEventListener('scroll', onScroll)
  }, [onBottomHit, isLoading, hasMoreData])

  return <div ref={contentRef}>{children}</div>
}
```

我们创建了一个 `isBottom` 函数，它将 React ref 对象作为参数。如果 ref 所指向的 `div` 的底部小于或等于我们窗口的 `innerHeight`，我们就可以知道滚动位置在 `div` 下方了，那么是时候加载更多数据了！

为了将此逻辑附加到窗口的 `scroll` 事件，我们使用了 `useEffect` 钩子。我们确保只在当前没有加载数据、还有更多数据以及实际触底时执行 `onBottomHit` 函数。

我们还确保通过在卸载时从窗口中删除 `scroll` 事件来清除副作用。

最后，我们处理了组件加载时的一个重要边缘情况。组件负载与触底相同的处理可能是更可取的。换句话说，我们希望加载一些初始数据！为此，我们确保让用户指定此行为并相应地采取行动。

## 测试效果

我们可以通过创建一个简单的数字列表来测试 `InfiniteSroll` 组件。它一次显示 100 个数字，最后显示 1000 个数字。我们可以添加 300 ms 的加载延迟以实现效果。

```tsx
const NUMBERS_PER_PAGE = 100

function App() {
  const [numbers, setNumbers] = useState<number[]>([])
  const [loading, setLoading] = useState(false)
  const [page, setPage] = useState(0)

  const hasMoreData = numbers.length < 1000

  const loadMoreNumbers = () => {
    setPage((page) => page + 1)
    setLoading(true)
    setTimeout(() => {
      const newNumbers = new Array(NUMBERS_PER_PAGE)
        .fill(1)
        .map((_, i) => page * NUMBERS_PER_PAGE + i)
      setNumbers((nums) => [...nums, ...newNumbers])
      setLoading(false)
    }, 300)
  }

  return (
    <InfiniteScroll
      hasMoreData={hasMoreData}
      isLoading={loading}
      onBottomHit={loadMoreNumbers}
      loadOnMount={true}
    >
      <ul>
        {numbers.map((n) => (
          <li key={n}>{n}</li>
        ))}
      </ul>
    </InfiniteScroll>
  )
}
```

如果我们检查一下实际效果，就会发现它工作得很好：

![无限滚动演示](https://upload-images.jianshu.io/upload_images/18281896-a79a165312f2fc5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[查看效果](https://codepen.io/lio-zero/pen/NWdVgLV)
