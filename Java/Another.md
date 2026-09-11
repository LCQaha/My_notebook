# WSL

1. 通过资源管理器访问：`\\wsl$`


# 前端框架

1. Router路由
    - 根据不同的环境给出不同的链接。在前后端分离框架下，要接触路由概念，如下所示：
        ```typescript
        export default {
          dev: {
            '/api/': {
              target: 'http://localhost:8080',
              changeOrigin: true,
            },
          },
        
          test: {
            '/api/': {
              target: 'https://test-api.example.com',
              changeOrigin: true,
            },
          },
        
          pre: {
            '/api/': {
              target: 'https://pre-api.example.com',
              changeOrigin: true,
            },
          },
        };
        ```

2. 环境

    | 环境 | 用途 | 前端形态 | API 来源 |
    |---|---|---|---|
    | Mock | 独立开发前端 | Umi 开发服务器 | 本地 `mock/` |
    | Dev | 联调开发 | Umi 开发服务器 | 开发后端 |
    | Test | 测试验收 | 测试服务器 | 测试后端 |
    | Pre | 上线前验证 | 类似生产部署 | 预发布后端 |
    | Prod | 正式用户使用 | 构建后的静态文件 | 正式后端 |



十、你日常最应该使用的流程

第一次拉取项目：

```powershell
npm ci
```

只开发页面：

```powershell
npm start
```

连接真实后端：

```powershell
npm run dev
```

开发完成后：

```powershell
npm run biome
git diff
npm run lint
npm test
npm run build
```

上线前：

```powershell
npm ci
npm run lint
npm test
npm run build
npm run preview
```

一般不要日常运行：

```text
npm run simple
npm run i18n-remove
npm run deploy
npm run record
npm run openapi
```

它们分别涉及批量删除、批量改写、远程发布、接口录制和自动生成代码，只应在明确需要时使用。


# MyBatis

## 删除标记


