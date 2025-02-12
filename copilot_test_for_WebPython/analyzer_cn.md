# analyzer.md

## 目的
`analyzer.md` 文件的作用是解析 `TestcaseXXX.xlsx` 文件。`TestcaseXXX.xlsx` 文件包含多个页面块，每个页面块描述了一个Web页面的操作步骤。

## 文件格式
`TestcaseXXX.xlsx` 文件的内容格式如下：

| 0            | 1                    |
|--------------|-----------------------|
| URL          | http://example.com    |
| identifier   | value                 |
| 名前         | 張三                  |
| 性別         | 男                    |
| 国           | 中国                  |
| オプション   | option_value          |
| 提出         | クリック              |
|              |                       |
| URL          | 前ページに移動        |
| identifier   | value                 |
| 名前         | 李四                  |
| 性別         | 女                    |
| 国           | アメリカ              |
| オプション   | another_option_value  |
| 提出         | クリック              |
|              |                       |
| URL          | http://anotherpage.com|
| identifier   | value                 |
| 名前         | 王五                  |
| 性別         | 男                    |
| 国           | 日本                  |
| オプション   | third_option_value    |
| 提出         | クリック              |

## 解析规则

### 页面块
- **URL行**：每个页面块的第一行是URL行，包含URL字符串。URL字符串所在的列的右侧的列中，记载的既可能是一个实际的URL地址，也可能是“上一个页面跳转”这样的字符串。
- **键值对表**：URL行下面的多行（一直到一个空行为止）是一个键值对表。键值对表的第一行是表头，包含 `identifier` 和 `value` 两列。`identifier` 列记载需要操作的控件名称，`value` 列记载对该控件的操作值。

### 操作模块
- 一个 `TestcaseXXX.xlsx` 文件中可能包含多个操作模块。每个操作模块之间至少有一个空行隔开。
- 每个操作模块的第一行是URL行，用于指示打开或跳转该页面。URL字符串既可能是一个实际的网络地址，也可能是上一个页面中相应的跳转指令。
- 每个操作模块的第二行通常为 `identifier` 和 `value`，分别指向页面中需要操作的控件及对控件需要进行的操作。

### identifier 列
- `identifier` 列记载的内容有以下可能：
  1. 完整且准确的控件名称。
  2. 大致的控件名称（需要在页面内查询最大概率的控件）。
  3. 控件的功能（需要结合页面具体分析最合适的控件）。
- 需要根据 `identifier` 在页面中找到对应的控件，然后进行相对应的操作。`identifier` 可能是Web页控件的input名称，也可能是该Web页控件的label的名称，也可能是部分匹配的名称，甚至可能是一个意思相同的别名名称。因此，需要进行模糊匹配或智能推测用户想操作的页面控件。

### value 列
- `value` 列记载的是对该控件的操作值。操作值包括输入框的输入值、radiobutton的选择值、checkbox的操作值、下拉列表的选择值、按钮的点击操作等。

## 解析步骤
1. 打开 `TestcaseXXX.xlsx` 文件。
2. 逐行读取文件内容，识别每个页面块。
3. 对于每个页面块，循环处理：
   - 读取URL行，获取页面的URL或跳转指令。
   - 读取键值对表的表头。
   - 读取键值对表，获取需要操作的控件及其操作值。
   - 根据 `identifier` 列的内容，智能推测并找到页面中对应的控件。
   - 根据 `value` 列的内容，对控件进行相应的操作。
   - 遇到空行，表示当前页面块结束，继续处理下一个页面块。

## 解析的结果
解析后的结果是一个包含多个页面块的列表，每个页面块包含一个URL和一组操作：
[
    {
        'url': 'http://example.com',
        'actions': [
            {'identifier': '姓名', 'value': '张三'},
            {'identifier': '性别', 'value': '男'},
            {'identifier': '国家', 'value': '中国'},
            {'identifier': '选项', 'value': 'option_value'},
            {'identifier': '提交', 'value': '点击'}
        ]
    },
    {
        'url': '前页面跳转',
        'actions': [
            {'identifier': '姓名', 'value': '李四'},
            {'identifier': '性别', 'value': '女'},
            {'identifier': '国家', 'value': '美国'},
            {'identifier': '选项', 'value': 'another_option_value'},
            {'identifier': '提交', 'value': '点击'}
        ]
    }
]

## 示例
假设 `Testcase001.xlsx` 文件的内容如下：

| 0            | 1                    |
|--------------|-----------------------|
| URL          | http://example.com    |
| identifier   | value                 |
| 名前         | 張三                  |
| 性別         | 男                    |
| 国           | 中国                  |
| オプション   | option_value          |
| 提出         | クリック              |
|              |                       |
| URL          | 前ページに移動        |
| identifier   | value                 |
| 名前         | 李四                  |
| 性別         | 女                    |
| 国           | アメリカ              |
| オプション   | another_option_value  |
| 提出         | クリック              |
|              |                       |
| URL          | http://anotherpage.com|
| identifier   | value                 |
| 名前         | 王五                  |
| 性別         | 男                    |
| 国           | 日本                  |
| オプション   | third_option_value    |
| 提出         | クリック              |

解析步骤如下：
1. 读取第一行，识别为URL行，URL为 `http://example.com`。
2. 读取第二行，识别为键值对表的表头。
3. 读取第三行到第七行，识别为键值对表，包含以下键值对：
   - 名前: 張三
   - 性別: 男
   - 国: 中国
   - オプション: option_value
   - 提出: クリック
4. 读取第八行，识别为空行，表示第一个页面块结束。
5. 读取第九行，识别为URL行，URL为 `前ページに移動`。
6. 读取第十行，识别为键值对表的表头。
7. 读取第十一行到第十五行，识别为键值对表，包含以下键值对：
   - 名前: 李四
   - 性別: 女
   - 国: アメリカ
   - オプション: another_option_value
   - 提出: クリック
8. 读取第十六行，识别为空行，表示第二个页面块结束。
9. 读取第十七行，识别为URL行，URL为 `http://anotherpage.com`。
10. 读取第十八行，识别为键值对表的表头。
11. 读取第十九行到第二十三行，识别为键值对表，包含以下键值对：
    - 名前: 王五
    - 性別: 男
    - 国: 日本
    - オプション: third_option_value
    - 提出: クリック
12. 读取第二十四行，识别为空行，表示第三个页面块结束。

通过以上解析步骤，可以获取每个页面块的URL和键值对表，并根据 `identifier` 和 `value` 列的内容，对页面进行相应的操作。

