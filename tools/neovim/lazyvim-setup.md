# LazyVim 配置指南

> 开箱即用的 Neovim 配置

---

## 1. 安装

### 前置要求

```bash
# 1. 安装 Neovim 0.9+
brew install neovim

# 2. 安装 Git
brew install git

# 3. 安装 Node.js (用于 LSP)
brew install node

# 4. 安装 Python (用于部分插件)
brew install python3

# 5. 安装 Ripgrep, fd, bat
brew install ripgrep fd bat
```

### 安装 LazyVim

```bash
# 备份现有配置
mv ~/.config/nvim ~/.config/nvim.bak
mv ~/.local/share/nvim ~/.local/share/nvim.bak

# 克隆 LazyVim
git clone https://github.com/LazyVim/starter ~/.config/nvim
rm -rf ~/.config/nvim/.git
```

---

## 2. 目录结构

```
~/.config/nvim/
├── lua/
│   ├── config/
│   │   ├── autocmds.lua    # 自动命令
│   │   ├── keymaps.lua     # 快捷键
│   │   ├── options.lua     # 选项
│   │   └── lazy.lua        # Lazy.nvim 配置
│   └── plugins/
│       ├── lsp.lua         # LSP 配置
│       ├── editor.lua      # 编辑器插件
│       └── custom.lua      # 自定义配置
└── init.lua                # 入口文件
```

---

## 3. 基础配置

```lua
-- lua/config/options.lua
local opt = vim.opt

-- 行号
opt.relativenumber = true
opt.number = true

-- 缩进
opt.tabstop = 2
opt.shiftwidth = 2
opt.expandtab = true
opt.autoindent = true

-- 搜索
opt.ignorecase = true
opt.smartcase = true

-- 外观
opt.termguicolors = true
opt.background = "dark"
opt.signcolumn = "yes"

-- 性能
opt.updatetime = 250
opt.timeoutlen = 300

-- 拆分窗口
opt.splitright = true
opt.splitbelow = true
```

---

## 4. 快捷键速查

### 空格键Leader

| 快捷键 | 功能 |
|--------|------|
| `<Space>` | Leader 键 |
| `<Space>e` | 文件浏览器 |
| `<Space>fb` | 缓冲区列表 |
| `<Space>ff` | 查找文件 |
| `<Space>fg` | Live grep |
| `<Space>fh` | 查找帮助 |
| `<Space>fm` | Marks |
| `<Space>fo` | Old files |
| `<Space>fw` | 查找单词 |

### 窗口操作

| 快捷键 | 功能 |
|--------|------|
| `<Ctrl-h>` | 切换到左侧窗口 |
| `<Ctrl-j>` | 切换到下方窗口 |
| `<Ctrl-k>` | 切换到上方窗口 |
| `<Ctrl-l>` | 切换到右侧窗口 |
| `<Ctrl-↑>` | 增大窗口高度 |
| `<Ctrl-↓>` | 减小窗口高度 |
| `<Ctrl-←>` | 增大窗口宽度 |
| `<Ctrl-→>` | 减小窗口宽度 |

### Tab 操作

| 快捷键 | 功能 |
|--------|------|
| `<Tab>` | 下一个 Tab |
| `<S-Tab>` | 上一个 Tab |
| `<Space>tn` | 新建 Tab |
| `<Space>tq>` | 关闭 Tab |

### LSP

| 快捷键 | 功能 |
|--------|------|
| `gd` | 跳转到定义 |
| `gr` | 查找引用 |
| `K` | 悬停文档 |
| `gD` | 跳转到声明 |
| `gi` | 跳转到实现 |
| `<Space>ca` | 代码动作 |
| `<Space>cr` | 重命名 |
| `<Space>cf` | 格式化 |
| `<Space>cd` | 显示诊断 |

---

## 5. LSP 配置

```lua
-- lua/plugins/lsp.lua

-- 配置语言服务器
return {
  {
    "neovim/nvim-lspconfig",
    opts = {
      servers = {
        -- TypeScript
        tsserver = {
          settings = {
            typescript = {
              format = {
                enable = false,
              },
            },
          },
        },

        -- Lua
        lua_ls = {
          settings = {
            Lua = {
              workspace = {
                checkThirdParty = false,
              },
            },
          },
        },

        -- Go
        gopls = {},

        -- Rust
        rust_analyzer = {},

        -- Python
        pyright = {},

        -- YAML
        yamlls = {},

        -- JSON
        jsonls = {},
      },
    },
  },
}
```

---

## 6. 常用插件

### 文件浏览

```lua
-- Neo-tree
return {
  "nvim-neo-tree/neo-tree.nvim",
  opts = {
    filesystem = {
      filtered_items = {
        visible = false,
        hide_dotfiles = false,
        hide_gitignored = false,
      },
    },
  },
}
```

### Git 集成

```lua
-- Gitsigns
return {
  "lewis6991/gitsigns.nvim",
  opts = {
    signs = {
      add          = { text = "│" },
      change       = { text = "│" },
      delete       = { text = "_" },
      topdelete    = { text = "‾" },
      changedelete = { text = "~" },
      untracked    = { text = "┆" },
    },
  },
}
```

### 终端

```lua
-- Toggleterm
return {
  "akinsho/toggleterm.nvim",
  opts = {
    open_mapping = [[<c-\>]],
    direction = "float",
  },
}
```

---

## 7. 自定义配置

### 自定义插件

```lua
-- lua/plugins/custom.lua

-- 自定义快捷键
vim.keymap.set("n", "<Leader>w", "<cmd>w<cr>", { desc = "Save" })
vim.keymap.set("n", "<Leader>q", "<cmd>q<cr>", { desc = "Quit" })

-- 自定义选项
vim.opt.scrolloff = 10
vim.opt.colorcolumn = "100"

-- 自定义命令
vim.api.nvim_create_user_command("Format", function(args)
  vim.lsp.buf.format({
    timeout_ms = 2000,
  })
end, { desc = "Format file" })
```

---

## 8. 常用操作

### 文件操作

```vim
" 新建文件
:enew

" 保存
:w

" 另存为
:saveas filename

" 重命名
:saveas newname | :bd! oldname
```

### 搜索替换

```vim
" 搜索
/pattern

" 搜索当前单词
*

" 全局替换
:%s/old/new/g

" 当前行替换
:s/old/new/g

" 确认替换
:%s/old/new/gc
```

### 分屏操作

```vim
" 水平分屏
:split

" 垂直分屏
:vsplit

" 关闭当前窗口
:close

" 关闭其他窗口
:only
```

---

## 9. 故障排查

**Q: LSP 不工作？**
```vim
:checkhealth lsp
:LspInfo
```

**Q: 插件不加载？**
```vim
:Lazy health
:Lazy sync
```

**Q: 快捷键不生效？**
```vim
:Telescope keymaps
```

---

## 10. 最佳实践

1. **使用 LazyVim 自带配置**：不要过度自定义
2. **按需加载插件**：大型项目考虑延迟加载
3. **保持配置简洁**：复杂逻辑放到单独文件
4. **定期更新**：`:Lazy update`
5. **备份配置**：使用 Git 管理配置

---

## 速查表

```vim
" 文件操作
<Space>ff     " 查找文件
<Space>fr     " 最近文件
<Space>e      " 文件浏览器

" 搜索
<Space>fg     " Live grep
<Space>fw     " 查找单词
n/N           " 下一个/上一个搜索结果

" 窗口
<Ctrl-h/j/k/l> " 切换窗口
<Space>sh     " 水平分屏
<Space>sv     " 垂直分屏

" LSP
gd           " 跳转定义
gr           " 查找引用
<Space>ca     " 代码动作
<Space>cr     " 重命名

" Git
<Space>gg     " Lazygit
<Space>gb     " Git blame
<Space>gs     " Gitsigns

" 调试
<Space>db     " 断点
<Space>dc     " 继续
<Space>do     " 步过
<Space>di     " 步入
```

---

## 相关文档

- [LazyVim 官网](https://www.lazyvim.org/)
- [LazyVim GitHub](https://github.com/LazyVim/LazyVim)
- [Neovim 文档](https://neovim.io/doc/)
