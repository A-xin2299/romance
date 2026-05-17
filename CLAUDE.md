# Romance Landing

## 项目背景

作为一名有前端和后端全栈能力的开发者，受市场环境影响，选择成人用品外卖作为副业方向。经过一周的市场考察后，注册成立了**心遇好物**公司，旗下外卖店铺名为**浪漫诱惑**。

为了提升店铺专业形象，制作了这个静态落地页，放在公司官网中。页面设计原则：**整体风格含蓄优雅，不直接展现成人用品信息**，通过"静谧生活""品质好物""隐私发货"等间接表达传递品牌调性，同时提供外卖入口和联系方式作为核心转化路径。

## 域名

### 已选域名

- **xinyu168.cn** — 公司官网主域名，已用于页脚官网链接和备案号

### 备选域名方案

| 域名 | 方案 | 说明 |
|------|------|------|
| romance-love.cn / .com | 店名直译 | 与"浪漫诱惑"呼应，但偏直白 |
| xinyuhaowu.com | 公司全拼 | 品牌化程度高，适合长期运营 |
| xinyu.shop | 短域名 | 简洁好记，但 .shop 后缀辨识度一般 |
| xy168.store / .shop | 缩写字母+吉祥数 | 简短但缺乏品牌关联 |

## 技术栈

- Vue 3 (Composition API, `<script setup lang="ts">`)
- Vite 8
- pnpm 包管理
- 单一页面组件：`src/components/RomanceLanding.vue`

## 常用命令

```bash
pnpm install   # 安装依赖
pnpm dev       # 启动开发服务器 (localhost:5173)
pnpm build     # 生产构建
pnpm preview   # 预览构建产物
```

## 项目结构

```
src/
  components/
    RomanceLanding.vue   # 唯一的业务组件，包含所有功能
  App.vue / main.js      # 入口（未定制修改）
public/                  # 静态资源
  美团.svg / 饿了么.svg / 京东.svg / 微信.svg / 邮件.svg / 话筒-copy.svg
  wechatQr.jpg           # 微信客服二维码
```

## 功能模块

### 1. 品牌展示
- 品牌名、标签、公司名称集中在配置区
- 品牌轮播（自动滚动 + 拖拽/触摸支持）
- 品牌：杜蕾斯、杰士邦、Okamoto、Le Wand、SVAKOM、春水堂、蜜曰、TENGA

### 2. 外卖平台
- 美团、饿了么、京东三个平台链接 + 二维码展示
- 平台 URL 和二维码图片占位在 `platforms` 数组中配置

### 3. 联系方式
- 电话（可点击拨打）、微信（复制 + 二维码弹窗）、邮箱（mailto）
- 点击微信"二维码"按钮弹出 `wechatQr.jpg`
- 微信号固定为 `Soren_18`

### 4. 高德地图
- 嵌入高德地图 2.0 API，显示店铺标记
- 点击标记跳转导航（自动区分 iOS/Android/微信/H5）
- 加载失败时显示 fallback 静态卡片
- 地图位置：`shopLocation` 对象配置（当前为珠江悦湖国际3栋1单元）

### 5. 页脚
- 官网：xinyu168.cn
- 备案号占位（`icpNumber`），链接到工信部备案网站

## 配置区

所有可修改内容集中在 `RomanceLanding.vue` 的 `// ==================== 配置区 ====================` 区域：

| 常量 | 说明 | 当前值 |
|------|------|--------|
| `shopName` | 店铺名称 | 浪漫诱惑 |
| `companyName` | 公司简称 | 心遇好物 旗下品质专送 |
| `companyFullName` | 公司全称 | 心遇好物 |
| `icpNumber` | 备案号 | 蜀ICP备XXXXXXXX号-1 |
| `contacts` | 联系方式数组 | 电话/微信/邮箱 |
| `shopLocation` | 地图坐标+地址 | 珠江悦湖国际 |
| `platforms` | 外卖平台配置 | 美团/饿了么/京东 |
| `brands` | 品牌展示列表 | 8个品牌 |

## 高德地图密钥

- JS API Key: `7c4f866bcda89d0324e73becdabf9bda`
- 安全密钥: `7de7d4d67f0abe3c99731fb647a0ddae`
