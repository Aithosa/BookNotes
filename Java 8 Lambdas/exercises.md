# 练习

## Ch02

### 1-c

下列哪些 Lambda 表达式有效实现了 `Function<Long,Long>` ？

- `x -> x + 1;`
- `(x, y) -> x + 1;`
- `x -> x == 1;`

`Function<Long, Long>` 它的抽象方法签名大致为：`Long apply(Long t)`。 这意味着 Lambda 表达式必须满足：

- 接收 1 个参数（类型为 Long）。
- 返回 1 个结果（类型为 Long）。

✅ `x -> x + 1`

- 参数：接收 1 个参数 x (推断为 Long)。
- 返回：`x + 1` 的结果是 Long。
- 结论：完全匹配。

❌ `(x, y) -> x + 1`

- 参数：接收 2 个参数。
- 错误原因：Function 接口只能接收 1 个参数。这个 Lambda 匹配的是 `BiFunction<Long, ?, Long>` 或 `BinaryOperator<Long>` 等双参数接口。

❌ `x -> x == 1`

- 参数：接收 1 个参数。
- 返回：`x == 1` 的结果是 boolean。
- 错误原因：接口要求返回 Long，但它返回了布尔值。这个 Lambda 匹配的是 `Predicate<Long>` 或 `Function<Long, Boolean>`。

补充：

在 Function<T, R> 接口中，这两个泛型参数分别代表输入类型和输出类型。

对于 Function<Long, Long>：

- 第一个 Long (T)：代表 输入 (Input) 参数的类型。
- 第二个 Long (R)：代表 输出 (Result) 返回值的类型。

> 因为 Function 接口被 Java 官方硬性定义成了“一个输入，一个输出”。其他类型用的是：BiFunction（Bi 代表 Binary，二元）、BiConsumer 等。

## Ch05

### 2-c

```java
// ❌ 原代码风格
private final static Set<Characteristics> characteristics = new HashSet<>();
static {
    characteristics.add(Characteristics.IDENTITY_FINISH);
}

// ✅ 改进
private final static Set<Characteristics> characteristics =
    Collections.singleton(Characteristics.IDENTITY_FINISH);

// ✅ 改进后（Java 8）
private final static Set<Characteristics> characteristics =
    Collections.unmodifiableSet(EnumSet.of(Characteristics.IDENTITY_FINISH));

// ✅ 改进后（Java 9+）
private final static Set<Characteristics> characteristics =
    Set.of(Characteristics.IDENTITY_FINISH);
```

为什么原代码这样写？

可能的原因：

- 老习惯 - 早期 Java 没有便捷的集合初始化方法
- 便于扩展 - 如果后续需要添加更多特性，修改方便（但这不是好理由）
- 代码风格不统一 - 注意 StringCollector 就用了更好的 Collections.emptySet()
