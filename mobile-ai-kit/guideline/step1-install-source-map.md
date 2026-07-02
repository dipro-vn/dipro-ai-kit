# STEP1: Mục đích chính là để tạo soure map, giúp AI hiểu được source code của dự án.

Chọn 1 trong 2 tool:

- Codegraph: đơn giản, nhanh gọn, phù hợp với project base
- Understand-Anything: phức tạp, output đầy đủ hơn, phù hợp với các dự án labo, maintain không có tài liệu đầy đủ.

# Codegraph: https://github.com/colbymchenry/codegraph

1. Install the CLI

```shell
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh
codegraph install
```

2. Initialize a CodeGraph project

```shell
cd your-project
codegraph init
```

3. Initialize each project

```shell
cd your_project
codegraph init
```

# Understand-Anything: https://github.com/Egonex-AI/Understand-Anything

1. Install the plugin

```
/plugin marketplace add Egonex-AI/Understand-Anything
/plugin install understand-anything
```

2. Analyze your codebase

```
/understand
```

Localized output: Use --language to generate content in your preferred language:
Supported languages: en (default), zh, zh-TW, ja, ko, ru

```
/understand --language ja
```

Read More: https://github.com/Egonex-AI/Understand-Anything#4-keep-learning
