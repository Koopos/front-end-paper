

在使用 Vite 构建前端项目（尤其是你正在深入的 3D 视觉和现代框架项目）时，`vite.config.js` 是整个项目的“大脑”。

Vite 的配置文件采用 ES 模块风格，通常配合 `defineConfig` 工具函数来获得完整的类型提示。

---

### 1. 基础配置结构

最基本的配置文件如下所示：

JavaScript

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue' // 以 Vue 为例

export default defineConfig({
  // 选项配置...
})
```

---

### 2. 常用核心配置项

#### **A. 项目根目录与公共路径**

- **`root`**: 项目根目录（`index.html` 所在的位置），默认是当前工作目录。
    
- **`base`**: 开发或生产环境服务的公共基础路径。如果你打算部署在 `https://foo.com/my-app/`，则需设置为 `'/my-app/'`。
    
- **`publicDir`**: 静态资源目录，默认是 `public`。
    

#### **B. 开发服务器配置 (`server`)**

对于前端开发，这是最常修改的部分：

JavaScript

```
server: {
  host: '0.0.0.0', // 监听所有地址，允许局域网访问
  port: 3000,      // 指定开发服务器端口
  open: true,      // 启动时自动在浏览器打开
  proxy: {         // 配置跨域代理
    '/api': {
      target: 'http://localhost:8080',
      changeOrigin: true,
      rewrite: (path) => path.replace(/^\/api/, '')
    }
  }
}
```

#### **C. 别名与模块解析 (`resolve`)**

为了避免在代码中使用复杂的相对路径（如 `../../../`），我们可以定义别名：

JavaScript

```
import { resolve } from 'path'

resolve: {
  alias: {
    '@': resolve(__dirname, 'src'),
    'assets': resolve(__dirname, 'src/assets'),
    'components': resolve(__dirname, 'src/components')
  }
}
```

#### **D. 构建配置 (`build`)**

决定了项目打包后的产物结构：

JavaScript

```
build: {
  outDir: 'dist',         // 打包输出目录
  assetsDir: 'assets',    // 静态资源存放目录
  sourcemap: false,       // 是否生成 source map
  minify: 'terser',       // 代码压缩方式（terser 或 esbuild）
  rollupOptions: {        // 传递给 Rollup 的底层配置
    output: {
      manualChunks: {     // 分包策略，减小单个 js 文件体积
        vendor: ['vue', 'three'] 
      }
    }
  }
}
```