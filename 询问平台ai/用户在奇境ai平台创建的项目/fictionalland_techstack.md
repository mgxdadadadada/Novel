# 奇境 AI (fictionalland.com) 前端技术栈分析

- 分析日期：2026-09-04 / 09-05
- 分析方式：黑盒静态分析（下载生产 bundle + CSS 反读指纹）
- 构建版本：`dist_20260904_144300`（2026-09-04 14:43:00）
- 本地样本：`D:\IDA-PRO-MCP\strix-main\.tmp\fl_main.js`（1.6MB）、`fl_main.css`（142KB）

## 核心框架
| 技术 | 证据 | 状态 |
|---|---|---|
| React 18 | `react`×108, `scheduler`×1, `jsx-runtime`×2, `createContext`×8, `useReducer`×3, `useSyncExternalStore`×2 | 已证实 |
| Vite | `<script type=module crossorigin>`, hash 文件名 `index-DQjtFulT.js` | 已证实（构建产物特征） |
| TypeScript | React+TS+shadcn 生态标配 | 推断（运行时不可见） |

## 路由
| 技术 | 证据 | 状态 |
|---|---|---|
| react-router-dom v6 (data router) | `react-router`×2, `RouterProvider`×1 | 已证实 |

## UI / 样式
| 技术 | 证据 | 状态 |
|---|---|---|
| Tailwind CSS | className 工具类（`markdown-content break-words animate-pulse bg-primary`）；CSS 内 `--tw-` 变量 | 已证实 |
| shadcn/ui 风格组件 | CSS 变量 `--background:45 20% 96%` 等 HSL 通道格式 + 整套 `--primary/--card/--muted/--accent/--destructive`，典型 shadcn 主题配置 | 已证实 |
| Radix UI | JS 内 `radix`×5（无样式可访问性原语，shadcn 的底层） | 已证实 |
| lucide-react 图标库 | `lucide`×94（重度使用） | 已证实 |
| 动画 | Tailwind 内置（`animate-pulse`）+ CSS；未用 framer-motion（`framer-motion`@motion 均 0） | 已证实（未使用 framer-motion） |

## Markdown 渲染
| 技术 | 证据 | 状态 |
|---|---|---|
| react-markdown | `micromark`×3, `remark`×3, `rehype`×3, `hast`×2, `mdast`×1（unified/remark/rehype 生态） | 已证实 |
| remark 插件 ×4 + rehype 插件 ×2 | `remarkPlugins:[mce,hce,lle,goe]`, `rehypePlugins:[Zie,uoe]`（具体名混淆） | 已证实 |
| 代码高亮 / 公式库 | `katex`/`mathjax`/`prismjs`/`highlight.js`/`shiki` 全 0 | 未使用（已证实） |

## 状态管理
| 技术 | 证据 | 状态 |
|---|---|---|
| 第三方状态库 | zustand/redux/jotai/mobx/recoil/valtio 全 0 | 未从字符串证实 |
| React 原生 Context + useReducer + hooks | `createContext`×8, `useReducer`×3 | 倾向（推断为自建状态层） |

## 网络 / 请求
| 技术 | 证据 | 状态 |
|---|---|---|
| axios | 0 | 未使用 |
| 原生 fetch + XMLHttpRequest | `XMLHttpRequest`×4 + bundle 内 fetch 调用 | 已证实（原生方案） |
| swr / react-query | 0 | 未使用 |

## 微信 / 小程序
| 技术 | 证据 | 状态 |
|---|---|---|
| miniapp 逻辑 | `miniapp`×3，i18n key：`miniapp_auth_result`/`MINIAPP_ERROR`/`miniapp_error` | 已证实 |
| 微信 JS-SDK | `jWeixin`/`wx.`/`WeixinJSBridge`/`wx.config` 全 0 | 未从字符串证实（可能动态加载或仅小程序跳转） |

## 国际化
| 技术 | 证据 | 状态 |
|---|---|---|
| i18next / react-i18next | 0 | 未使用 |
| 自研 / 轻量 i18n | 大量语言 key 散落 bundle | 推断 |

## 其他
- `d3`×2（疑似 d3-scale 等子模块，可能用于图表/进度可视化）
- `XMLHttpRequest`×4（某 SDK 或封装层）
- 字体：系统字体栈（`ui-sans-serif` + `PingFang SC`/`Microsoft YaHei`），未自托管字体

## 明确未使用（字符串 0；注意 minified + tree-shaking 可能使库名消失，故"0 命中"仅代表"无法从字符串证实"）
antd、@mui、HeadlessUI、framer-motion、dayjs、date-fns、lodash、immer、zod、classnames、clsx、tailwind-merge、i18next、swr、react-query、qrcode、crypto-js、jsencrypt、three、echarts、react-icons、iconfont。

## 结论
标准现代 React SPA + shadcn/ui 技术栈：
**React 18 + Vite + TypeScript + Tailwind CSS + Radix UI + shadcn/ui + lucide-react + react-markdown + react-router-dom v6 + 原生 fetch**。

该栈完全开源可获取，"复刻技术栈"零门槛；但"复刻产品"仍受限于后端、设计质感、资源文件与混淆业务逻辑（见 frontend_js_analysis.md）。
