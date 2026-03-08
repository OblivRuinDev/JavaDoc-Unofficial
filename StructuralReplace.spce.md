# Java API 文档结构化替换规范

本文档定义基于 IntelliJ IDEA 结构化替换（Structural Replace）功能进行 Java API 文档批量翻译的规范，包括结构化替换的命名、描述文档格式、更新流程等内容。<br>
所有贡献者应严格遵守，以确保结构化替换模板的可维护性和文档的统一。

模板遵循[《Java 翻译术语规范》](./translate.spec.md)中的术语规范。<br>
模板文件保存在项目 [.idea/structuralSearch.xml](.idea/structuralSearch.xml) 中，可通过 `Edit/编辑`-`Find/查找`-`Replace Structurally/结构化替换` 自动导入或手动配置。

> [!IMPORTANT]
> **我们强烈不建议手动导入配置**，因为这可能会导致配置不完整。请始终通过 IDEA 的结构化替换对话框进行模板的创建、修改和保存。

---

## 1. 总体原则

- **特异性**：搜索模板必须具有高度特异性，避免误匹配无关内容。
- **结构保留**：替换模板应尽可能保留原始 HTML 结构，仅调整语序或替换文本，不破坏文档布局与链接。
- **可追溯**：每个模板的变更必须通过**单独**的 Git 提交记录关联，确保修改历史清晰。

---

## 2. 结构化替换模板命名规范

每个结构化替换模板在 IDEA 配置文件和本文档中均有唯一标识，命名方式如下：

### 2.1 IDEA 配置文件中的 `name` 属性（纯英文）

`<replaceConfiguration name="...">` 中的 `name` 属性采用以下格式：

```
<作用域或文档类型>(可选): <英文模板简述>
```

- **作用域或文档类型**：用小写英文单词描述模板适用的文档范围，例如 `index-files`（索引文件）、`class-detail`（类详细页）、`package-summary`（包摘要页）。
   若模板通用，可省略冒号及作用域。
- **英文模板简述**：用英文简要概括匹配的原始结构，例如 `Static variable in class ...`。

**示例**：
`index-files: Static variable in class ...` 
表示该模板适用于索引页，匹配“Static variable in class ...”结构。

### 2.2 Markdown 文档中的标题

在本文档中，每个模板的标题格式为以下两种之一：

```
<作用域或文档类型>：<中文模板简述>  <-  <英文模板简述>
```
或
```
<中文模板简述>  <-  <英文模板简述>
```

- **中文模板简述**：用中文简要描述模板完成的任务，例如“类中的静态变量”。
- **作用域或文档类型**：为 IDEA 配置中 `作用域或文档类型` 所对应的中文翻译。
- **英文模板简述**：与 IDEA 配置中的`英文模板简述`一致。

**示例**：
`索引文件：类...中的静态变量  <-  Static variable in class ...`

---

## 3.结构化替换描述规范

每个模板在本文档中应包含以下部分：

- **标题**：按上述命名规范。
- **搜索模板**：用于匹配结构的模板，应当具有高度特异性。
- **替换模板**：用于在找到的结果中进行结构化替换的模板。
- **变量说明**：简述模板中每个变量的含义，并注明必要的约束（如文本约束、正则表达式、脚本等）。
- **特殊变量处理**：若使用了脚本变量，需详细说明脚本的作用及编写原理。
- **相关提交**：记录该模板最后一次修改及应用的 Git commit hash，格式为 `相关提交：<commit-hash> ...`。若后续对搜索模板进行了更改并重新执行替换，则应当附加新的 commit hash。

> [!NOTE]  
> 模板更新后，必须同步更新描述文档，并更新“相关提交”字段为新的 commit hash。

---

## 4. 模板文件管理

`structuralSearch.xml` 由 IntelliJ IDEA 自动生成和管理，其内容遵循以下结构：

```xml
<replaceConfiguration name="模板名称" text="搜索模板内容" ... replacement="替换模板内容">
  <constraint name="变量1" ... />
  <constraint name="变量2" ... />
</replaceConfiguration>
```

- 每个 `<replaceConfiguration>` 对应一个模板，其 `name` 属性必须唯一且符合命名规范。
- `<constraint>` 用于定义变量的约束条件（如文本约束、正则表达式等）。

> [!WARNING]  
> **切勿手动编辑此文件**，所有修改应通过 IDEA 的结构化替换对话框进行，以免破坏文件格式。

---

## 5. 模板开发与更新流程

为确保模板变更的可追溯性，所有操作必须遵循以下 Git 提交规范。

### 5.1 新增模板

1. 在 IDEA 中通过结构化替换对话框创建新模板，配置搜索模板、替换模板及所有约束。
2. 在对话框的左上角选择保存模板，选择`在IDE或项目中保存模板`，勾选`保存在项目中(通过VCS共享)`，并根据规范为该模板命名。
3. 在结构化替换窗口中执行一次查找，并选择一个匹配项预览替换，确保模板正确无误，并检查搜索结果是否包含不相关内容。
4. 使用该模板进行结构化替换。
5. 通过项目历史记录等方式审阅修改，确保无误。
6. 在本文档中按照规范添加模板描述，包括标题、变量说明、脚本解释等。
7. 提交一个独立的 Git commit，包含：
   - `structuralSearch.xml` 的变更（新增的 `<replaceConfiguration>`）。
   - 本文档中对应新增的描述内容。
   - 使用该模板进行结构化替换后所有被替换的文件的变更。
   - Commit message 格式：`Structural-Replace|NEW: <模板名称>`  
     例如：`Structural-Replace|NEW: index-files: Static variable in class ...`

### 5.2 修改模板

模板的修改分为两种情况：**仅修改替换模板** 和 **修改搜索模板**。两种情况的处理流程不同，需严格遵循。

- **修改搜索模板**：  
  1. 在 IDEA 的结构替换窗口中修改模板的搜索条件或约束。  
  2. 执行一次结构化替换，并对查找出的匹配项进行审阅，或对项目历史记录进行严格核对修改，以确保模板无误。  
  3. 在本文档中更新模板描述（包括搜索模板代码、变量说明等）。  
  4. 提交一个独立的 commit，包含 `structuralSearch.xml` 的变更和本文档的更新，Commit message 格式：`Structural-Replace|MODIFY=search: <模板名称>`。

- **修改替换模板**：  
  这种情况通常
  1. 在 IDEA 中修改替换内容。  
  2. 保存后，在本文档中更新替换模板代码。  
  3. 若替换逻辑影响脚本变量，需同步更新脚本说明。  
  4. 提交一个独立的 commit，Commit message 格式：`Structural-Replace|MODIFY=replace: <模板名称>`。

**重要**：任何修改都必须伴随 `structuralSearch.xml` 的变更，并在同一 commit 中提交，确保文件与描述一致。


以下模板已按本规范实施，可作为后续模板的参考。

---

### 索引文件：类...中的静态变量  <-  Static variable in class ...
name: index: Static variable in class ...  
将索引文件中`Static variable in class className`替换为`类 className 中的静态变量`，
且将`class in package.name`翻译为`package.name 包中的类`。

#### 搜索模板
```html
<dt><a href="$fieldHref$" class="$tagClass1$">$fieldName$</a> - Static variable in class $classPkgWithDot$<a href="$classHref$" title="$classInPkg$">$className$</a></dt>
```

**变量说明**：
- `$fieldHref$`、`$tagClass1$`、`$fieldName$`：字段链接的 href、class 属性及显示名称。
- `$classPkgWithDot$`：类的包路径加点（如 `javax.print.attribute.standard.`）。
- `$classHref$`、`$className$`：类链接的 href 及类名。
- `$classInPkg$`：**必须添加文本约束** `class in .*`，确保仅匹配以 `class in ` 开头的 title 属性（例如 `title="class in java.util"`）。

#### 替换模板
```html
<dt><a href="$fieldHref$" class="$tagClass1$">$fieldName$</a> - 类 $classPkgWithDot$<a href="$classHref$" title="$classPkg$ 包中的类">$className$</a> 中的静态字段</dt>
```

**新增变量 `$classPkg$`**：
- 作用：从 `$classInPkg$` 中提取纯包名（去除开头的 `class in ` 和首尾双引号）。
- 配置方法：为 `$classPkg$` 添加 **脚本**，脚本内容如下：
  ```groovy
  classInPkg.getText().substring(10, classInPkg.getText().length() - 1)
  ```
  **脚本说明**：原始字符串形如 `"class in java.util"`（包含首尾双引号）。`substring(10)` 跳过前 9 个字符（即 `"class in `），`length()-1` 去掉末尾的引号，最终得到 `java.util`。数值 `10` 基于 `"class in ` 长度为 9（一个双引号 + 8 个字符）计算得出。

