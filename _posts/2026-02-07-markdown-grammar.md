---

title: Markdown 语法全攻略：从入门到精通 (含 LaTeX 公式与高级图表)
date: 2026-02-07 20:00:00 +0800
categories: [Tools, Markdown]
tags: [markdown, documentation, latex, syntax]
math: true
mermaid: true
img_path: /assets/img/posts/
---

Markdown 是一种轻量级标记语言，创始人为 John Gruber。它允许人们使用易读易写的纯文本格式编写文档，然后将其转换成有效的 XHTML（或者 HTML）。

---

## 一、 基础语法 (Basic Syntax)

### 1. 标题
Markdown 支持 6 种级别的标题，对应 HTML 中的 `<h1>` 到 `<h6>`。

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
typora快捷键：ctrl + 1/2/3/4/5/6
```

### 2. 文本样式

通过简单的符号即可实现加粗、斜体等效果。

| **语法**             | **效果**        | **说明**         |
| -------------------- | --------------- | ---------------- |
| `*斜体*` 或 `_斜体_` | *斜体*          | 单星号或单下划线 |
| `**粗体**`           | **粗体**        | 双星号           |
| `***加粗斜体***`     | ***加粗斜体\*** | 三星号           |
| `~~删除线~~`         | ~~删除线~~      | 双波浪线         |
| `<u>下划线</u>`      | <u>下划线</u>   | HTML 标签实现    |

### 3. 分割线 

在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。

```markdown
***
---
___
```

## 二、 列表与引用

### 1. 无序列表

使用星号 `*`、加号 `+` 或是减号 `-` 作为列表标记：

```markdown
* 第一项
* 第二项
* 第三项
typora快捷键：ctrl + shift + ]
```

### 2. 有序列表

使用数字接着一个英文句点：

```markdown
1. 第一项
2. 第二项
3. 第三项
typora快捷键：ctrl + shift + [
```

## 三、 代码与图片

### 1. 行内代码

使用反引号 ` 包裹代码，例如：`print("Hello")`。

### 2. 代码块

使用三个反引号 ``` 包裹，并指定语言（如 python, java, cpp）以启用语法高亮：

````
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```
````

### 3. 图片与链接

- **链接**：`[链接名称](链接地址 "可选标题")`
- **图片**：`![图片描述](图片地址 "可选标题")`

> **提示**：Markdown 原生无法调整图片大小。如果需要调整，可以使用 HTML 标签： `<img src="url" width="50%" height="50%">`

## 四、 表格

Markdown 制作表格使用 `|` 来分隔不同的单元格，使用 `-` 来分隔表头和其他行。

**语法：**

Markdown

```markdown
| 左对齐 | 右对齐 | 居中对齐 |
| :----- | ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
typora快捷键：ctrl + t
```

**效果：**

| **左对齐** | **右对齐** | **居中对齐** |
| ---------- | ---------- | ------------ |
| Left       | Right      | Center       |
| Text       | Text       | Text         |

## 五、 LaTeX 数学公式 

Markdown 支持 LaTeX 语法，是科研写作的神器。

### 1. 行内与独行公式

- **行内公式**：使用 `$` 包裹，例如 $\Gamma(n) = (n-1)!$
- **独行公式**：使用 `$$` 包裹，居中显示。

### 2.数学公式

#### 2.1希腊字母

|     字母     |    实现    |     字母     |    实现    |
| :----------: | :--------: | :----------: | :--------: |
|      A       |    `A`     |  $$\alpha$$  |  `\alpha`  |
|      B       |    `B`     |  $$\beta$$   |  `\beta`   |
|  $$\Gamma$$  |  `\Gamma`  |  $$\gamma$$  |  `\gamma`  |
|  $$\Delta$$  |  `\Delta`  |  $$\delta$$  |  `\delta`  |
|  $$\exist$$  |  `\exist`  | $$\epsilon$$ | `\epsilon` |
| $$\Lambda$$  | `\Lambda`  |  $$\zeta$$   |  ``\zeta`  |
| $$\lambda$$  | `\lambda`  |   $$\eta$$   |   `\eta`   |
|  $$\Theta$$  |  `\Theta`  |  $$\theta$$  |  `\theta`  |
|   $$\mu$$    |   `\mu`    |  $$\iota$$   |  `\iota`   |
|   $$\nu$$    |   `\nu`    |  $$\kappa$$  |  `\kappa`  |
|   $$\Xi$$    |   `\Xi`    |   $$\xi$$    |   `\xi`    |
| $$\omicron$$ | `\omicron` |   $$\rho$$   |   `\rho`   |
|   $$\Pi$$    |   `\Pi`    |   $$\pi$$    |   `\pi`    |
|  $$\Sigma$$  |  `\Sigma`  |  $$\sigma$$  |  `\sigma`  |
|   $$\tau$$   |   `\tau`   |  $\forall$   | `\forall`  |
| $$\Upsilon$$ | `\Upsilon` | $$\upsilon$$ |  \upsilon  |
|   $$\Phi$$   |   `\Phi`   |   $$\phi$$   |    \phi    |
|   $$\Psi$$   |   `\Psi`   |   $$\psi$$   |    \psi    |
|  $$\Omega$$  |  `\Omega`  |  $$\omega$$  |   \omega   |

#### 2.2 运算符和关系表达

| 运算类型         | 符号表示                                                   |
| ---------------- | ---------------------------------------------------------- |
| 加法             | $$+$$                                                      |
| 减法             | $$-$$                                                      |
| 乘法             | $$\times$$   $$\ast$$    $$\cdot$$                         |
| 除法             | $$\div$$   $$\frac{x}{y}$$                                 |
| 累加             | $$\sum_{n=1}^{10}{a_n}$$                                   |
| 累乘             | $$\prod_{n=1}^{10}{a_n}$$                                  |
| n次方            | $$e^n$$  （n次方和指数函数都可以用上标`^`代替，下标：`_`） |
| 开n次方          | $$\sqrt[n]{y}$$                                            |
| 三角函数         | $$sin$$、$$cos$$                                           |
| 对数             | $$\ln 10$$   $$\log_e{10}$$                                |
| 正交(垂直)       | $$\bot$$                                                   |
| 角度             | $$\angle 90^\circ$$                                        |
| 大于             | $$\geq$$                                                   |
| 小于             | $$\leq$$                                                   |
| 不等于           | $$\neq$$                                                   |
| 恒等于（定义为） | $$\equiv$$                                                 |
| 约等于           | $$\approx$$                                                |
| 积分             | $$\int_1^2{x}dx$$                                          |
| 双重积分         | $$\iint_1^2{x}dxdy$$                                       |
| 无穷             | $$\infin$$                                                 |
| 求导             | $$\nabla$$   $$y\prime$$                                   |
| 偏导             | $$\partial$$                                               |
| 极限             | $$\lim_{n \rightarrow +\infin}{a_n}$$                      |
| 向量             | $$\vec{a}$$                                                |





