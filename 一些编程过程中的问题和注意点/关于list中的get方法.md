











# 问题：List 的 get() 方法返回的是对象的引用，可以解释一下这句话吗



###### 

当我们说 `List` 的 `get()` 方法返回的是对象的引用时，意思是：

- `List` 本身存储的是元素的 **引用**，而不是元素的实际值。
- `get()` 方法通过索引返回的是 **元素所在的引用**，而不是一个元素的拷贝。

我们来看几个例子来更好地理解这一点。

### 示例 1: 使用 `List<Integer>`

```
java复制编辑List<Integer> list = new ArrayList<>();
list.add(10);
list.add(20);

Integer num = list.get(0);
```

在这个例子中：

- `list.get(0)` 返回的是存储在索引位置 0 的元素的 **引用**，也就是 `Integer` 类型的引用。
- `Integer` 类型是不可变对象（immutable），这意味着虽然我们获取到了 `Integer` 的引用，但是不能通过这个引用修改它的值，因为它的值是不可变的。所以我们不能直接通过 `num = 30;` 来修改 `list` 中的元素。

但是，如果你尝试通过 `get()` 来修改 `Integer` 类型的元素，像下面这样：

```

list.get(0) = 30;  // 编译错误
```

会报错，因为 `list.get(0)` 返回的值是一个不可变对象，不能直接赋值。

### 示例 2: 使用 `List<int[]>`（数组是可变的）

```
java复制编辑List<int[]> list = new ArrayList<>();
list.add(new int[]{1, 2});
list.add(new int[]{3, 4});

int[] array = list.get(0);
array[0] = 10;  // 修改了数组的内容
```

在这个例子中：

- `list.get(0)` 返回的是索引位置 0 的数组（`int[]`）的 **引用**。
- 数组（`int[]`）是可变对象（mutable），所以你可以通过这个引用来修改数组的内容。例如，`array[0] = 10;` 就改变了数组的第一个元素。

### 关键点总结：

- **引用**是指向某个对象内存位置的地址，它并不包含对象的实际数据，而是指向该数据。

- 当你调用 

  ```
  get()
  ```

   方法时，返回的是列表中存储的对象的引用。

  - 对于不可变类型（如 `Integer`），即使你获得了对象的引用，你不能修改它的值。
  - 对于可变类型（如 `int[]`），你可以通过引用修改对象内部的内容。

因此，`List<Integer>` 返回的对象引用是不可修改的，而 `List<int[]>` 返回的是数组的引用，你可以通过这个引用修改数组中的元素。









# 问题：不可变类型和可变类型都有哪些



###### 

在 Java 中，类型分为 **可变类型**（mutable）和 **不可变类型**（immutable）。这两者的主要区别在于是否可以修改对象的状态（即其值）。

### 不可变类型（Immutable Types）

不可变类型的对象一旦被创建，它的状态（值）就不能再改变。如果你尝试修改它的值，实际上是创建了一个新的对象。

常见的不可变类型包括：

1. **String**
   - `String` 对象一旦创建，其值就无法改变。如果你对字符串进行修改，实际上是创建了一个新的字符串对象。
   - 例如，`String str = "hello";`，然后对它进行修改时，`str = str + " world";`，实际上是创建了一个新的字符串对象 `"hello world"`，并将其赋值给 `str`。
2. **Wrapper Classes（包装类）**
   - Java 中的包装类 `Integer`、`Double`、`Character`、`Long`、`Float`、`Byte`、`Short`、`Boolean` 都是不可变的。
   - 例如，`Integer` 是不可变的，`Integer num = 10;` 之后，不能修改 `num` 的值，只能创建新的 `Integer` 对象。
3. **LocalDate、LocalTime、LocalDateTime（java.time 包中的类）**
   - `java.time` 包中的许多类（如 `LocalDate`、`LocalTime` 和 `LocalDateTime`）都是不可变的。
   - 例如，`LocalDate date = LocalDate.of(2023, 2, 17);`，这个 `date` 对象一旦创建，它的日期值就无法修改。如果你想更改日期，必须使用类似 `date.plusDays(1)` 这样的方法来返回一个新的对象。
4. **BigInteger 和 BigDecimal**
   - `BigInteger` 和 `BigDecimal` 也是不可变的类。它们的每次计算（如加法、乘法）都会返回一个新的对象。

### 可变类型（Mutable Types）

可变类型的对象在创建后可以修改其内部的状态（值）。你可以通过改变对象的字段或状态来更新它。

常见的可变类型包括：

1. **数组（Array）**

   - 数组是可变的，可以修改其元素的值。即使你声明了一个数组，它的元素值也可以随时改变。
   - 例如：`int[] arr = {1, 2, 3};` 然后 `arr[0] = 10;`，数组的值被修改了。

2. **ArrayList 和其他 List 实现类**

   - `ArrayList`、`LinkedList` 等集合类是可变的，你可以修改它们的内容（如添加、删除或更改元素）。
   - 例如，`List<Integer> list = new ArrayList<>(); list.add(10);` 你可以添加、删除或更新列表中的元素。

3. **StringBuilder 和 StringBuffer**

   - `StringBuilder` 和 `StringBuffer` 类是可变的，允许你在字符串上进行修改（如插入、删除、替换字符等），而不会创建新的对象。
   - 例如：`StringBuilder sb = new StringBuilder("hello"); sb.append(" world");` 这时 `sb` 的内容被修改为 `"hello world"`。

4. **HashMap 和其他 Map 实现类**

   - `HashMap`、`TreeMap`、`LinkedHashMap` 等都可以修改它们的键值对，你可以动态添加、删除或修改它们的元素。
   - 例如：`Map<String, Integer> map = new HashMap<>(); map.put("apple", 1); map.put("banana", 2);` 你可以添加新的键值对，或修改已有键的值。

5. **自定义类**

   - 如果你创建一个自定义类，并且它的字段没有被声明为 `final`，那么你就可以修改该类的实例字段，从而使对象的状态发生变化。

   - 例如：

     ```
     java复制编辑class Person {
         String name;
         int age;
     
         Person(String name, int age) {
             this.name = name;
             this.age = age;
         }
     }
     ```

     你可以通过访问 

     ```
     Person
     ```

      对象的字段来修改它的状态。

### 不可变类型和可变类型的对比

| 特性             | 不可变类型                                     | 可变类型                                |
| ---------------- | ---------------------------------------------- | --------------------------------------- |
| 例子             | `String`, `Integer`, `LocalDate`, `BigInteger` | `ArrayList`, `HashMap`, `StringBuilder` |
| 是否可以修改内容 | 不可以                                         | 可以                                    |
| 修改方法         | 必须创建新对象                                 | 可以直接修改对象的内部状态              |
| 线程安全         | 因为不可修改，天生线程安全                     | 需要额外的同步机制才能保证线程安全      |
| 性能             | 在大量修改时性能较差（因为每次都创建新对象）   | 性能较高，但需要小心并发问题            |

### 为什么不可变类型有用？

- **线程安全**：由于不可变对象的状态不能被改变，它们天然是线程安全的。多个线程可以安全地共享同一个不可变对象而不需要加锁。
- **避免副作用**：不可变对象不会意外改变它们的状态，因此更容易理解和调试。
- **更好的缓存和哈希计算**：不可变对象通常可以在缓存中安全共享，因为它们的值在对象生命周期内保持不变。

总结来说，**不可变类型**的主要特点是它们的值一旦创建就不可改变，而**可变类型**允许对对象的内容进行修改。