# 开发需求文档

## 项目名称
自动化Web操作的Python脚本

## 项目背景
为了提高工作效率，我们需要根据许多用户提前定义好的测试case，生成多个Python脚本。原则上，每个测试case生成一个操作Web的Python脚本。每个测试case放在一个 `TestcaseXXX` 的文件夹下，该文件夹下有一个 `TestcaseXXX.xlsx` 文件。注意：这里的 `XXX` 是数字编号，范围是从001到999。根据 `main.md` 生成的每个测试case对应的自动操作Web页的Python脚本，都放在对应的 `TestcaseXXX` 文件夹下。

## 需求描述
首先你要使用`analyzer.md` 中的提示，解析对应的TestcaseXXX.xlsx文件，得到操作web页的必要信息，然后使用你取得的Web页操作信息，生成Web页操作的python脚本。
该脚本能够自动执行以下Web操作：
1. 打开指定的URL。
2. 自动登录到网站（提供用户名和密码）。
3. 填写并提交表单。
4. 填写完所有表单（也就是设置好所有Web页元素和控件以后），需要整个页面截图后，再提交表单，截图放在自己对应的测试case的 `TestcaseXXX` 文件夹下，命名规则是 `capture_xxx.png`（xxx为累加的数字编号，从001开始）。
5. 使用 `pywinauto` 库解析表单填写完成的页面，并保存在本地。解析页面以及保存到本地的处理，由 `webpage_analyzer.py` 进行，`webpage_analyzer.py` 会事先准备好。

## 功能需求
1. **打开URL**：脚本应能够打开指定的URL。
2. **自动登录**：脚本应能够使用提供的用户名和密码自动登录到网站。
3. **填写表单**：脚本应能够自动填写并提交表单。
4. **页面截图**：填写完所有表单后，脚本应能够对整个页面进行截图，并保存到对应的 `TestcaseXXX` 文件夹下，命名规则是 `capture_xxx.png`（xxx为累加的数字编号，从001开始）。
5. **页面解析和保存**：使用 `pywinauto` 库解析表单填写完成的页面，并保存在本地。解析页面以及保存到本地的处理，由 `webpage_analyzer.py` 进行。

## 技术要求
1. 使用Python编写脚本。
2. 使用Selenium库进行Web自动化操作。
3. 使用 `pywinauto` 库解析表单填写完成的页面。
4. 只支持Edge浏览器。
5. 脚本应具有良好的错误处理机制，能够在操作失败时输出错误信息。
6. 对Web页的操作要求来自一个Excel文件。
7. 对Excel文件的解析规则由 `analyzer.md` 文件定义。
8. 根据每个测试case生成的自动操作Web页的Python脚本，都放在对应的 `TestcaseXXX` 文件夹下。
9. 生成的Web操作Python脚本中，每个函数需要追加注释，包括函数名、用途、入参、出参、返回值。

## 文件夹和文件结构
所有的 `TestcaseXXX` 文件夹一般都放在 `main.md` 同目录下的 `Testcase` 文件夹中。文件夹和文件结构如下：
```
/项目根目录
│
├── main.md
├── analyzer.md
├── webpage_analyzer.py
├── Testcase
│   ├── Testcase001
│   │   ├── Testcase001.xlsx
│   │   ├── script.py
│   │   └── capture_001.png
│   ├── Testcase002
│   │   ├── Testcase002.xlsx
│   │   ├── script.py
│   │   └── capture_001.png
│   └── ...
```
- `main.md`：主需求文档。
- `analyzer.md`：定义Excel文件解析规则的文档。
- `webpage_analyzer.py`：解析表单填写完成的页面并保存到本地的脚本。
- `Testcase` 文件夹：包含所有测试case的文件夹。
  - `TestcaseXXX` 文件夹：每个测试case的文件夹，包含对应的Excel文件、生成的Python脚本和截图文件。

## 交付物
1. 完整的Python脚本代码（最终的python脚本代码中只包含自动操作Web页面的代码，不包含解析过程的代码;TestcaseXXX.xlsx的解析过程由你来进行，我需要的是最终的自动化web操作的py脚本）。
2. 脚本的使用说明文档。