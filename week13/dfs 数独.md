# P1784 数独
[[搜索]]
## 题目描述

数独是根据 $9 \times 9$ 盘面上的已知数字，推理出所有剩余空格的数字，并满足每一行、每一列、每一个粗线宫内的数字均含 $1 - 9$ ，不重复。每一道合格的数独谜题都有且仅有唯一答案，推理方法也以此为基础，任何无解或多解的题目都是不合格的。

芬兰一位数学家号称设计出全球最难的“数独游戏”，并刊登在报纸上，让大家去挑战。

这位数学家说，他相信只有“智慧最顶尖”的人才有可能破解这个“数独之谜”。

据介绍，目前数独游戏的难度的等级有一到五级，一是入门等级，五则比较难。不过这位数学家说，他所设计的数独游戏难度等级是十一，可以说是所有数独游戏中，难度最高的等级。他还表示，他目前还没遇到解不出来的数独游戏，因此他认为“最具挑战性”的数独游戏并没有出现。

## 输入格式

一个未填的数独。

## 输出格式

填好的数独。

## 输入输出样例 #1

### 输入 #1

```
8 0 0 0 0 0 0 0 0 
0 0 3 6 0 0 0 0 0 
0 7 0 0 9 0 2 0 0 
0 5 0 0 0 7 0 0 0 
0 0 0 0 4 5 7 0 0 
0 0 0 1 0 0 0 3 0 
0 0 1 0 0 0 0 6 8 
0 0 8 5 0 0 0 1 0 
0 9 0 0 0 0 4 0 0
```

### 输出 #1

```
8 1 2 7 5 3 6 4 9 
9 4 3 6 8 2 1 7 5 
6 7 5 4 9 1 2 8 3 
1 5 4 2 3 7 8 9 6 
3 6 9 8 4 5 7 2 1 
2 8 7 1 6 9 5 3 4 
5 2 1 9 7 4 3 6 8 
4 3 8 5 2 6 9 1 7 
7 9 6 3 1 8 4 5 2
```

## 说明/提示

2022-04-17 @farteryhr 贡献了三组 hack 数据。加入了其中两组。第三组过强（来源：<https://www.dcc.fc.up.pt/~acm/sudoku.pdf>），放在下边供自测。

```
9 0 0 8 0 0 0 0 0
0 0 0 0 0 0 5 0 0 
0 0 0 0 0 0 0 0 0 
0 2 0 0 1 0 0 0 3
0 1 0 0 0 0 0 6 0
0 0 0 4 0 0 0 7 0
7 0 8 6 0 0 0 0 0 
0 0 0 0 3 0 1 0 0 
4 0 0 0 0 0 2 0 0 
```

输出

```
9 7 2 8 5 3 6 1 4 
1 4 6 2 7 9 5 3 8 
5 8 3 1 4 6 7 2 9 
6 2 4 7 1 8 9 5 3 
8 1 7 3 9 5 4 6 2 
3 5 9 4 6 2 8 7 1 
7 9 8 6 2 1 3 4 5 
2 6 5 9 3 4 1 8 7 
4 3 1 5 8 7 2 9 6 
```

## 分析：
核心：一个格子一个格子填，纯暴力；
从1-9中找到合法的形式，然后再填下一个格子；

需要进行大量的剪枝；

剪枝：
三乘三的格子，每一列，每一行即可；

![[1765885915132.png]]
标记每一行每一列采用两个二维哈希表


![[1765886005779.png]]
特殊的，判断每个九宫格：
将大的方格划分，每个大格子给一个数组下标；
如何转化：
将i , j 位置的数 i / 3 , j / 3即可知道属于哪个方格[[哈希表 和 模拟实现]]
[[小技巧]]
## 要点注意：
[[错误汇总]]
在每一个for循环最后的return false不可以忘记，表示回到之前的路径；

![[Pasted image 20251217220752.png]]
这个地方如果写成：
```cpp
if(a[i][j]) 
{
	if(dfs(i, j + 1)) return true; 会出现巨大错误；
}
```

## 错误详细分析

### 原始错误代码：
```cpp
if (a[x][y])  // 如果原位置有数字
{
    if (dfs(x, y + 1)) return true;  // 只考虑了成功情况
    // 这里缺少处理失败情况的代码
}
```

### 问题解析：

**实际发生了什么：**
1. 当`dfs(x, y + 1)`返回`true`时：`return true`，正确传递结果
2. 当`dfs(x, y + 1)`返回`false`时：**没有返回语句**，程序会继续执行下面的循环代码
3. 这样就会尝试在已经有数字的位置（`a[x][y] != 0`）上填写其他数字（1-9），这是完全错误的

**举例说明：**
```cpp
// 假设当前位置(0,0)已经有数字5
if (a[0][0])  // 条件成立
{
    if (dfs(0, 1)) return true;  // 探索下一个位置
    // 如果dfs(0, 1)返回false，这里没有return，继续往下...
}

// 程序继续执行到这里
for (int i = 1; i <= 9; i++)  // 尝试在已经有数字5的位置上填写1-9
{
    // 这是错误的！不能修改原数独的数字
}
```

### 正确逻辑应该是：
```cpp
if (a[x][y] != 0)  // 原位置有数字
{
    // 不能修改这个位置，只能继续处理下一个位置
    // 结果完全取决于后续的搜索
    return dfs(x, y + 1);  // 无论成功失败，都直接返回结果
}
```

## 如何在以后避免类似错误

### 1. **明确回溯算法的三种状态**

```cpp
bool dfs(state)
{
    // 1. 终止条件：成功情况
    if (is_success(state)) return true;
    
    // 2. 终止条件：失败情况（可选）
    if (is_impossible(state)) return false;
    
    // 3. 做出选择并回溯
    for (每个可能的选项)
    {
        if (选项无效) continue;  // 剪枝
        
        做出选择;
        if (dfs(新状态)) return true;  // 如果成功，直接返回
        撤销选择;  // 回溯
    }
    
    // 4. 所有选项都试过但失败
    return false;
}
```

### 2. **区分"必须处理"和"可以选择"的位置**

对于数独这类有固定初始值的回溯问题：
- **固定位置**：初始有值，必须跳过，不能修改
- **空白位置**：可以选择填入数字

```cpp
// 错误模式：没有处理固定位置的边界情况
if (fixed_position) {
    // 只处理成功，没处理失败
    if (dfs(next)) return true;
    // 缺少 return false;
}

// 正确模式：固定位置直接传递结果
if (fixed_position) {
    // 不能修改，直接看后面的结果
    return dfs(next);
}
```

### 3. **使用状态机思维**

把每个位置看作一个状态机：
- 状态1：已经填好数字 → 只能转移，不能修改
- 状态2：未填数字 → 可以尝试各种选择

```cpp
bool dfs(int x, int y)
{
    // 边界检查
    if (到达终点) return true;
    
    // 状态判断
    if (位置已填数字) {  // 状态1：已填数字
        // 不能修改，只能前进
        return dfs(下一个位置);  // 直接返回结果
    }
    else {  // 状态2：未填数字
        // 可以尝试各种选择
        for (选项) {
            尝试;
            if (dfs(下一个位置)) return true;
            回溯;
        }
        return false;  // 所有选项都失败
    }
}
```

### 4. **编写防御性代码**

**反模式：**
```cpp
// 容易忘记处理分支
if (condition) {
    // 只处理了true分支
    do_something();
    // 忘记处理false分支
}
```

**好习惯：**
```cpp
// 明确处理所有分支
if (condition) {
    // 处理true分支
    return do_something();
} else {
    // 明确处理false分支
    return handle_else_case();
}

// 或者用if-return模式
if (condition) return do_something();
// 这里明确是else情况
return handle_other_case();
```

### 5. **使用辅助函数提高可读性**

```cpp
bool dfs(int x, int y)
{
    if (is_end(x, y)) return check_solution();
    
    if (is_fixed(x, y)) {
        // 固定位置，不能修改
        return dfs_next(x, y);  // 调用辅助函数，逻辑清晰
    }
    
    // 空白位置，尝试填写
    return try_all_numbers(x, y);
}

// 辅助函数：处理固定位置的下一位置
bool dfs_next(int x, int y)
{
    int nx = x, ny = y + 1;
    if (ny == 9) { nx++; ny = 0; }
    return dfs(nx, ny);
}

// 辅助函数：尝试所有可能的数字
bool try_all_numbers(int x, int y)
{
    for (int num = 1; num <= 9; num++) {
        if (!is_valid(x, y, num)) continue;
        
        set_number(x, y, num);
        if (dfs_next(x, y)) return true;
        remove_number(x, y, num);  // 回溯
    }
    return false;
}
```

### 6. **添加详细的注释和断言**

```cpp
bool dfs(int x, int y)
{
    // 参数合法性检查
    assert(x >= 0 && x < 9);
    assert(y >= 0 && y < 9);
    
    // 终止条件：所有位置处理完毕
    if (x == 9) return true;
    
    // 计算下一个位置
    int nx = x, ny = y + 1;
    if (ny == 9) { nx++; ny = 0; }
    
    // 当前位置已有数字（固定位置）
    if (board[x][y] != 0) {
        // 重要：固定位置不能修改，直接处理下一个位置
        // 不需要回溯，因为这个数字是题目给定的
        return dfs(nx, ny);
    }
    
    // 当前位置是空白，尝试1-9
    for (int num = 1; num <= 9; num++) {
        if (can_place(x, y, num)) {
            board[x][y] = num;
            mark_used(x, y, num, true);
            
            if (dfs(nx, ny)) return true;  // 成功找到解
            
            // 回溯：撤销选择
            mark_used(x, y, num, false);
            board[x][y] = 0;
        }
    }
    
    // 所有数字都尝试过但失败
    return false;
}
```

### 7. **单元测试思维**

```cpp
// 测试固定位置的行为
void test_fixed_position()
{
    // 设置一个固定位置
    board[0][0] = 5;
    
    // 调用dfs
    bool result = dfs(0, 0);
    
    // 验证：固定位置的值不应该改变
    assert(board[0][0] == 5);  // 这个断言会失败，如果代码有错误
}
```

### 8. **常见的容易犯错场景**

**场景1：忘记返回失败情况**
```cpp
// ❌ 错误
if (fixed) {
    if (dfs(next)) return true;
    // 忘记 return false;
}

// ✅ 正确
if (fixed) return dfs(next);
```

**场景2：错误地进入了循环**
```cpp
// ❌ 错误
if (fixed) {
    dfs(next);  // 没有返回值处理
    // 继续执行循环...
}

// ✅ 正确
if (fixed) return dfs(next);
```

## 总结要点

1. **固定位置原则**：如果位置已经确定，不能修改，只能传递搜索结果
2. **完整返回原则**：每个条件分支都要有明确的返回值
3. **提前返回原则**：使用`return dfs

# 为什么`return false`不能去掉：函数返回值逻辑的完整性

## 深入理解DFS的返回机制

### 1. **基本事实：C++函数必须返回声明类型的值**

```cpp
bool dfs(...) {  // 声明返回bool类型
    // 函数体
    // 必须有返回bool值的语句
}
```

如果你去掉最后的`return false`，**函数在某些执行路径下可能没有返回值**，这是未定义行为(UB)，编译器会警告或报错。

### 2. **递归函数的值传递链**

想象一下DFS的调用链：

```cpp
dfs(0,0)  // 第一层
  → dfs(0,1)  // 第二层
    → dfs(0,2)  // 第三层
      → ... → 某个dfs返回true/false
```

**关键点**：每一层递归都必须向上一层返回`true`或`false`，形成一条完整的返回值传递链。

### 3. **为什么去掉`return false`会报错？**

让我们分析所有可能的执行路径：

```cpp
bool dfs(int x, int y)
{
    // 路径1：到达终点
    if (x == 9) return true;
    
    // 路径2：原位置有数字
    if (a[x][y] != 0) {
        return dfs(x, y+1);  // ✅ 有返回值
    }
    
    // 路径3：尝试1-9
    for (int i = 1; i <= 9; i++) {
        if (有效) {
            填入数字;
            if (dfs(x, y+1)) return true;  // ✅ 如果成功，返回true
            回溯;
        }
    }
    
    // 路径4：所有数字都试过但失败
    // 如果没有下面的return false，这里就没有返回值！
    // return false;  // 必须存在！
}
```

**问题**：如果程序执行到路径4（所有数字都尝试失败），函数没有返回值就结束了，这是未定义行为。

### 4. **实际案例说明**

假设我们在`(8,8)`位置，前面所有位置都填对了，但`(8,8)`位置没有有效数字：

```
调用链：dfs(0,0) → ... → dfs(8,8)
```

在`dfs(8,8)`中：
- 不是终止条件（x!=9）
- 位置是空的（a[8][8]==0）
- 尝试数字1-9，每个都无效
- 循环结束...如果没有`return false`，函数结束，没有返回值！

此时`dfs(8,8)`返回给`dfs(8,7)`一个**未定义的值**（可能是内存中的随机值）。`dfs(8,7)`将这个值当作bool处理，程序行为完全不可预测。

### 5. **报错的原因**

现代编译器有**路径分析**功能，会检查函数所有可能的执行路径是否都有返回值。

```cpp
// 编译器分析：
bool dfs(...) {
    if (条件1) return true;      // 路径1：有返回值
    if (条件2) return dfs(...);  // 路径2：有返回值
    for (...) {
        if (dfs(...)) return true;  // 路径3：有返回值
    }
    // 编译器发现：如果循环结束，没有返回值！
    // 编译错误：not all control paths return a value
}
```

### 6. **如何思考递归的返回值**

**关键思想**：把递归函数看作黑盒，只关心输入输出

```cpp
bool dfs(当前位置) {
    // 基础情况：直接能判断成功/失败
    if (成功条件) return true;
    if (失败条件) return false;
    
    // 递归情况：尝试各种选择
    for (每个选择) {
        做出选择;
        bool 子问题结果 = dfs(下一个位置);  // 子问题
        if (子问题结果 == true) {
            // 子问题成功 = 当前问题成功
            return true;
        }
        撤销选择;
    }
    
    // 所有选择都试过，子问题都失败了
    return false;  // 当前问题失败
}
```

### 7. **正确的思维方式**

**错误的思考**："这一层的dfs会自己结束"
- 函数不会"自己结束"，它必须通过`return`语句结束
- 递归函数必须通过返回值向上传递信息

**正确的思考**：每个dfs调用都要回答一个问题："从当前位置开始，能找到解吗？"
- 答案只有两种：`true`（能）或`false`（不能）
- 必须在所有可能的执行路径上都给出答案

### 8. **测试你的理解**

考虑这个简化版本：
```cpp
bool findPath(int pos) {
    if (pos == 目标) return true;
    
    for (每个方向) {
        移动;
        if (findPath(新位置)) return true;
        撤回;
    }
    
    // 如果这里没有return false，会发生什么？
}
```

问题：如果所有方向都走不通，函数返回什么？

答案：没有返回值！调用者会得到一个随机值，程序行为不确定。

### 9. **避免错误的实用技巧**

**技巧1：先写返回语句框架**
```cpp
bool dfs(...) {
    // 先写好所有必须的返回语句
    if (终止条件1) return true;
    if (终止条件2) return false;
    
    // 尝试各种选择
    for (...) {
        if (dfs(...)) return true;
    }
    
    return false;  // 不要忘记这个！
}
```

**技巧2：使用变量存储结果**
```cpp
bool dfs(...) {
    bool found = false;  // 默认没找到
    
    if (终止条件) {
        found = true;
    } else if (需要尝试) {
        for (每个选择) {
            尝试;
            found = dfs(下一步);
            if (found) break;  // 找到了，提前结束
            回溯;
        }
    } else {
        // 其他情况...
        found = dfs(下一步);
    }
    
    return found;  // 统一返回
}
```

**技巧3：编译器警告是你的朋友**
- 开启编译器警告：`-Wall -Wextra -Werror`
- 编译器会帮你发现未返回值的路径

### 10. **类比理解**

把递归调用看作公司层级：
- 每个员工（dfs调用）需要向经理汇报结果
- 员工只有两种回答：任务完成（true）或任务失败（false）
- 如果员工什么都不说（没有返回值），经理不知道发生了什么，无法决策

```cpp
// 就像这样：
bool 员工汇报(任务) {
    if (任务简单) return true;  // 完成
    
    // 把任务分给下属
    for (每个下属) {
        if (下属汇报(子任务)) return true;  // 下属完成了
    }
    
    return false;  // 必须告诉上级：我和下属都搞不定
}
```

## 总结

**为什么必须要有`return false`？**

1. **语法要求**：非void函数必须在所有执行路径上返回值
2. **逻辑完整性**：必须告诉调用者"所有尝试都失败了"
3. **递归的完整性**：每个递归层都需要向上一层报告成功/失败
4. **程序正确性**：没有明确的返回值会导致未定义行为

**记住这个模式**：
```cpp
bool 递归函数(参数) {
    // 1. 处理可以直接返回的情况
    if (成功条件) return true;
    if (失败条件) return false;
    
    // 2. 尝试各种选择
    for (每个选择) {
        做出选择;
        if (递归函数(新参数)) return true;  // 子问题成功
        撤销选择;
    }
    
    // 3. 所有选择都失败了
    return false;  // 绝对不能少！
}
```

这是回溯算法的标准模式，少了最后一步，整个算法就无法正确工作。

## 代码：
```cpp
#include <iostream>

using namespace std;

const int N = 10;

int n = 9;

int a[N][N];

bool row[N][N], col[N][N], st[N][N][N];

bool dfs(int i, int j)

{

if(j == n)

{
// 当这⼀⾏填满之后
i++;

}

if(i == n) return true; // 找到⼀种合法的情况，就停⽌递归

if(a[i][j]) return dfs(i, j + 1);

for(int x = 1; x <= 9; x++)

{

if(row[i][x] || col[j][x] || st[i / 3][j / 3][x]) continue; // 剪枝

row[i][x] = col[j][x] = st[i / 3][j / 3][x] = true;

a[i][j] = x;

if(dfs(i, j + 1)) return true;

// 恢复现场

row[i][x] = col[j][x] = st[i / 3][j / 3][x] = false;

a[i][j] = 0;

}

return false;

}

int main()

{

for(int i = 0; i < n; i++)

{

for(int j = 0; j < n; j++)

{

cin >> a[i][j];

int x = a[i][j];

if(x)

{

// 标记⼀下

row[i][x] = col[j][x] = st[i / 3][j / 3][x] = true;

}

}

}

dfs(0, 0);

for(int i = 0; i < n; i++)

{

for(int j = 0; j < n; j++)

{

cout << a[i][j] << " ";

}

cout << endl;

}

return 0;

}
```