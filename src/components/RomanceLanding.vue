<template>
  <div class="page-wrapper">
    <!-- 全屏 Banner -->
    <div class="banner">
      <img src="/banner.svg" alt="浪漫诱惑">
    </div>

    <div class="container">

        <!-- 品牌标签 -->
        <div class="brand-tags">
          <span v-for="tag in brandTags" :key="tag">{{ tag }}</span>
        </div>

        <!-- 品类展示 -->
        <div class="category-section">
          <div class="section-heading">精选品类 · 品质生活</div>
          <div class="category-grid">
            <div v-for="cat in categories" :key="cat.name" class="category-chip">
              <div class="cat-icon-circle" :style="{ background: cat.bg }">
                <span>{{ cat.icon }}</span>
              </div>
              <div class="cat-name">{{ cat.name }}</div>
            </div>
          </div>
        </div>

        <!-- 品牌展示区 -->
        <div class="brand-showcase">
          <div class="showcase-header">
            <div class="showcase-title">
              <span>&#9825;</span> 甄选品牌 · 静谧之选
            </div>
            <div class="showcase-desc">品质承诺 · 安心相伴</div>
          </div>
          <div class="logo-carousel-wrap" ref="carouselWrap">
            <div class="logo-carousel-row" ref="carouselRow1">
              <div v-for="logo in row1" :key="logo.name + '-r1'" class="logo-item">
                <div class="logo-circle">
                  <img :src="logo.src" :alt="logo.name">
                </div>
              </div>
            </div>
            <div class="logo-carousel-row" ref="carouselRow2">
              <div v-for="logo in row2" :key="logo.name + '-r2'" class="logo-item">
                <div class="logo-circle">
                  <img :src="logo.src" :alt="logo.name">
                </div>
              </div>
            </div>
          </div>
          <div class="showcase-footer">&#10023; 官方授权正品保障 &#10023; 隐私包装</div>
        </div>

        <!-- 外卖平台入口 -->
        <div class="section-card platform-section">
          <div class="section-heading">便捷下单 · 随心所选</div>
          <div class="platform-links">
            <a
              v-for="platform in platforms"
              :key="platform.key"
              :href="platform.url"
              class="platform-btn"
              target="_blank"
              rel="noopener"
            >
              <div class="p-icon" :class="platform.class">
                <img :src="platform.icon" :alt="platform.name">
              </div>
              <div class="p-text">
                <div class="p-name">{{ platform.name }}</div>
                <div class="p-desc">{{ platform.desc }}</div>
              </div>
              <div class="p-arrow">&rsaquo;</div>
            </a>
          </div>
          <div class="section-tip">点击按钮跳转店铺 · 扫码下方二维码即可下单</div>

          <!-- 店铺二维码 -->
          <div class="qr-row">
            <div v-for="qr in platforms" :key="qr.key + '-qr'" class="qr-card">
              <div class="qr-box" @click="previewQr(qr.qrcode)">
                <img v-if="qr.qrcode" :src="qr.qrcode" :alt="qr.name + '二维码'">
              </div>
              <div class="qr-label">
                <img :src="qr.icon" alt="">
                {{ qr.name }}
              </div>
            </div>
          </div>
        </div>

        <!-- 联系我们 & 配送信息 -->
        <div class="contact-section">
          <div class="section-heading">联系我们 &amp; 配送信息</div>

          <!-- 高德地图 + 联系方式 -->
          <div class="contact-grid">
            <div id="mapContainer" class="map-container" :class="{ 'map-loading': mapLoading }"></div>
            <div class="contact-list">
              <div v-for="item in contacts" :key="item.label" class="contact-chip">
                <img class="contact-icon" :src="item.icon" :alt="item.label">
                <span class="contact-content">
                  <template v-if="item.type === 'tel'">
                    <template v-for="(phone, i) in item.value" :key="i">
                      <span v-if="i > 0" class="tel-divider">/</span>
                      <a :href="'tel:' + phone">{{ phone }}</a>
                    </template>
                  </template>
                  <template v-else-if="item.type === 'email'">
                    <a :href="'mailto:' + item.value">{{ item.value }}</a>
                  </template>
                  <template v-else-if="item.type === 'wechat'">
                    <span>{{ item.value }}</span>
                    <button class="copy-btn" @click="copyWechat">{{ wechatCopied ? '已复制 ✓' : '复制' }}</button>
                    <button class="copy-btn" @click="wechatQrOpen = true">二维码</button>
                  </template>
                </span>
              </div>
            </div>
          </div>

          <div class="trust-badges">
            <span v-for="badge in trustBadges" :key="badge">{{ badge }}</span>
          </div>
        </div>

        <!-- 页脚 -->
        <div class="footer">
          &copy; {{ companyFullName }} | {{ shopName }} &middot; 专注品质生活<br>
          本店承诺合法合规经营，注重隐私与专业服务
          <div class="icp">
            <span>官方网站：<a href="https://xinyu168.cn" target="_blank" rel="noopener">xinyu168.cn</a></span>
            <span style="margin-left:12px;">|</span>
            <span style="margin-left:12px;">
              <a class="beian-link" href="https://beian.miit.gov.cn/" target="_blank" rel="noopener">
                {{ icpNumber }}
                <svg width="12" height="12" viewBox="0 0 16 16" fill="currentColor" style="opacity:0.5;">
                  <path d="M11.2 13.2a1 1 0 0 1-.7.3H3.3a1 1 0 0 1-1-1V5.3a1 1 0 0 1 .3-.7l6.5-6.5a1 1 0 0 1 .7-.3h6.2a1 1 0 0 1 1 1v6.2a1 1 0 0 1-.3.7l-5.4 5.4zM10 2.4l-5.6 5.6H3V6.4l5.6-5.6h1.4v1.6zm-7 6.8V13h3.8l4.2-4.2-3.6-3.6-4.4 4z"/>
                </svg>
              </a>
            </span>
          </div>
        </div>
    </div>
  </div>

  <!-- 微信二维码弹窗 -->
  <Transition name="fade">
    <div v-if="wechatQrOpen" class="qr-overlay" @click.self="wechatQrOpen = false">
      <div class="qr-modal">
        <button class="qr-close" @click="wechatQrOpen = false">&times;</button>
        <p class="qr-title">微信客服二维码</p>
        <img src="/wechatQr.jpg" alt="微信二维码">
      </div>
    </div>
  </Transition>

  <!-- 二维码预览弹窗（店铺二维码） -->
  <Transition name="fade">
    <div v-if="qrPreviewUrl" class="qr-overlay" @click.self="qrPreviewUrl = ''">
      <div class="qr-modal">
        <button class="qr-close" @click="qrPreviewUrl = ''">&times;</button>
        <p class="qr-title">店铺二维码</p>
        <img :src="qrPreviewUrl" alt="二维码">
        <p class="qr-hint">长按图片可保存到手机相册</p>
      </div>
    </div>
  </Transition>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

// ==================== 配置区 ====================
const shopName = '浪漫诱惑'
const companyFullName = '心遇好物'
const icpNumber = '蜀ICP备XXXXXXXX号-1'

const brandTags = ['静谧 · 品味', '心遇好物', '夜间极速达']
const trustBadges = ['🌙 24h 在线客服', '❤️ 隐私发货 · 安心之选', '🏷️ 心遇好物 · 浪漫不将就']

const categories = [
  { name: '安全套', icon: '🛡️', bg: 'linear-gradient(135deg,#f8f0fc,#ede0f5)' },
  { name: '润滑液', icon: '💧', bg: 'linear-gradient(135deg,#f0f5fc,#dde8f5)' },
  { name: '情趣玩具', icon: '✨', bg: 'linear-gradient(135deg,#fcf0f8,#f5dde8)' },
  { name: '情趣内衣', icon: '👗', bg: 'linear-gradient(135deg,#f8f0fc,#ede0f5)' },
  { name: '延时喷剂', icon: '⏱️', bg: 'linear-gradient(135deg,#f5fcf0,#e8f5dd)' },
  { name: '按摩油', icon: '🫧', bg: 'linear-gradient(135deg,#fcf5f0,#f5e8dd)' },
]

const brandLogos = [
  { name: '杜蕾斯', src: '/logo/杜蕾斯.png' },
  { name: '杰士邦', src: '/logo/杰士邦.jpeg' },
  { name: '冈本', src: '/logo/冈本.jpg' },
  { name: '名流', src: '/logo/名流.png' },
  { name: 'olo', src: '/logo/olo.png' },
  { name: '第6感', src: '/logo/第6感.png' },
  { name: '大象', src: '/logo/大象.png' },
  { name: '赤尾', src: '/logo/赤尾.jpeg' },
  { name: '来乐', src: '/logo/来乐.jpeg' },
  { name: '涩井', src: '/logo/涩井.jpeg' },
  { name: '谜姬', src: '/logo/谜姬.jpeg' },
  { name: '司沃康', src: '/logo/司沃康.jpeg' },
  { name: '霏慕', src: '/logo/霏慕.jpeg' },
  { name: 'galaku', src: '/logo/galaku.png' },
  { name: 'TENGA', src: '/logo/TENGA.jpeg' },
  { name: '潮汐派对', src: '/logo/潮汐派对.png' },
  { name: '倍力乐', src: '/logo/倍力乐.png' },
  { name: '安太医', src: '/logo/安太医.png' },
  { name: '独爱', src: '/logo/独爱.png' },
  { name: '姬欲', src: '/logo/姬欲.webp' },
]

// 两行均分，每行 10 个，复制一份实现无缝轮播
const halfCount = Math.ceil(brandLogos.length / 2)
const row1 = brandLogos.slice(0, halfCount)
const row2 = brandLogos.slice(halfCount)

const platforms = [
  { key: 'meituan', name: '美团外卖', desc: '浪漫诱惑 · 美团旗舰店', class: 'mt', icon: '/美团.svg', url: 'https://i.meituan.com/comingsoon?shopid=romance_love', qrcode: '/店铺QR.jpg' },
  { key: 'eleme', name: '饿了么', desc: '浪漫诱惑 · 饿了么官方店', class: 'elm', icon: '/饿了么.svg', url: 'https://h5.ele.me/shop/?id=romance_love', qrcode: '/店铺QR.jpg' },
  { key: 'jd', name: '京东外卖', desc: '浪漫诱惑 · 京东专送店', class: 'jd', icon: '/京东.svg', url: 'https://paipai.jd.com/shop?sid=romance_charm', qrcode: '/店铺QR.jpg' },
]

const contacts = [
  { label: '店主专线', type: 'tel', value: ['17341787045', '15760551571'], icon: '/话筒-copy.svg' },
  { label: '客服微信', type: 'wechat', value: 'Soren_18', icon: '/微信.svg' },
  { label: '合作邮箱', type: 'email', value: 'service@xinyu168.cn', icon: '/邮件.svg' },
]

const shopLocation = { lng: 103.8588, lat: 30.6873, name: '珠江悦湖国际3栋1单元' }

// ==================== 状态 ====================
const mapLoading = ref(true)
const wechatCopied = ref(false)
const carouselWrap = ref<HTMLElement | null>(null)
const carouselRow1 = ref<HTMLElement | null>(null)
const carouselRow2 = ref<HTMLElement | null>(null)

const wechatQrOpen = ref(false)
const qrPreviewUrl = ref('')

// ==================== 二维码预览 ====================
function previewQr(url: string) {
  qrPreviewUrl.value = url
}

// ==================== 复制微信 ====================
async function copyWechat() {
  try {
    await navigator.clipboard.writeText('Soren_18')
    wechatCopied.value = true
    setTimeout(() => (wechatCopied.value = false), 1500)
  } catch {
    prompt('请手动复制微信号：', 'Soren_18')
  }
}

// ==================== 设备检测 & 导航 URL ====================
function getNavUrl() {
  const ua = navigator.userAgent
  const { lng, lat, name } = shopLocation
  const n = encodeURIComponent(name)

  if (/iPhone|iPad|iPod/i.test(ua)) {
    return `iosamap://path?sourceApplication=浪漫诱惑&dlat=${lat}&dlon=${lng}&dname=${n}&dev=0&t=0`
  }
  if (/Android/i.test(ua)) {
    return `androidamap://route?sourceApplication=浪漫诱惑&dlat=${lat}&dlon=${lng}&dname=${n}&dev=0&t=0`
  }
  if (/MicroMessenger/i.test(ua)) {
    return `https://apis.map.qq.com/tools/route?marker=coord:${lat},${lng};title:${n}&referer=浪漫诱惑`
  }
  return `https://uri.amap.com/marker?position=${lng},${lat}&name=${n}&callnative=0`
}

// ==================== 高德地图 2.0 ====================
function initMap() {
  // 安全密钥
  ;(window as any)._AMapSecurityConfig = {
    securityJsCode: '7de7d4d67f0abe3c99731fb647a0ddae'
  }

  // 动态加载 loader
  const script = document.createElement('script')
  script.src = 'https://webapi.amap.com/loader.js'
  script.onload = () => {
    const loader = (window as any).AMapLoader
    if (!loader) return fallbackMap()
    loader
      .load({ key: '7c4f866bcda89d0324e73becdabf9bda', version: '2.0', plugins: [] })
      .then((AMap: any) => {
        mapLoading.value = false
        const map = new AMap.Map('mapContainer', {
          zoom: 15,
          center: [shopLocation.lng, shopLocation.lat],
          resizeEnable: false,
          zoomEnable: false,
          dragEnable: false,
          doubleClickZoom: false,
          scrollWheel: false,
          touchZoom: false,
          mapStyle: 'amap://styles/light',
        })

        const marker = new AMap.Marker({
          position: [shopLocation.lng, shopLocation.lat],
          title: shopLocation.name,
          cursor: 'pointer',
          map,
        })

        marker.on('click', () => { window.location.href = getNavUrl() })
        document.getElementById('mapContainer')?.addEventListener('click', () => { window.location.href = getNavUrl() })
      })
      .catch(fallbackMap)
  }
  script.onerror = fallbackMap
  document.head.appendChild(script)
}

function fallbackMap() {
  mapLoading.value = false
  const el = document.getElementById('mapContainer')
  if (!el) return
  el.innerHTML = `<div style="display:flex;align-items:center;justify-content:center;height:100%;color:#8e5ab6;font-size:0.85rem;flex-direction:column;gap:8px;">
    <span>&#128205; 成都市温江区 · 珠江悦湖国际</span>
    <span style="font-size:0.72rem;color:#a070c0;">点击打开地图导航</span>
  </div>`
  el.addEventListener('click', () => {
    const { lng, lat, name } = shopLocation
    window.open(`https://uri.amap.com/marker?position=${lng},${lat}&name=${encodeURIComponent(name)}&callnative=1`, '_blank')
  })
}

// ==================== Logo 轮播 ====================
function initLogoCarousel() {
  const wrap = carouselWrap.value
  const r1El = carouselRow1.value
  const r2El = carouselRow2.value
  if (!wrap || !r1El || !r2El) return

  // 等 DOM 渲染完成后计算
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      // 用 offsetLeft + offsetWidth 精确计算最大滚动距离
      function getMaxScroll(el: HTMLElement) {
        const last = el.lastElementChild as HTMLElement
        if (!last) return 0
        return Math.max(0, last.offsetLeft + last.offsetWidth - wrap.clientWidth)
      }

      const row1Max = getMaxScroll(r1El)
      const row2Max = getMaxScroll(r2El)
      if (row1Max <= 0 || row2Max <= 0) return

      let pos1 = 0
      let pos2 = 0
      let dir1 = 1
      let dir2 = 1
      let timer1: ReturnType<typeof setInterval> | null = null
      let timer2: ReturnType<typeof setInterval> | null = null

      function step1() {
        pos1 += dir1
        if (pos1 >= row1Max) { pos1 = row1Max; dir1 = -1 }
        if (pos1 <= 0) { pos1 = 0; dir1 = 1 }
        r1El.style.transform = `translateX(${-pos1}px)`
      }
      function step2() {
        pos2 += dir2
        if (pos2 >= row2Max) { pos2 = row2Max; dir2 = -1 }
        if (pos2 <= 0) { pos2 = 0; dir2 = 1 }
        r2El.style.transform = `translateX(${-pos2}px)`
      }

      timer1 = setInterval(step1, 50)
      timer2 = setInterval(step2, 50)

      let isDown = false
      let startX = 0
      let start1 = 0
      let start2 = 0

      function down(e: MouseEvent | TouchEvent) {
        isDown = true
        startX = 'touches' in e ? e.touches[0].pageX : e.pageX
        start1 = pos1
        start2 = pos2
        if (timer1) clearInterval(timer1)
        if (timer2) clearInterval(timer2)
      }

      function move(e: MouseEvent | TouchEvent) {
        if (!isDown) return
        e.preventDefault()
        const x = 'touches' in e ? e.touches[0].pageX : e.pageX
        const delta = startX - x
        pos1 = Math.max(0, Math.min(row1Max, start1 + delta))
        pos2 = Math.max(0, Math.min(row2Max, start2 + delta))
        r1El.style.transform = `translateX(${-pos1}px)`
        r2El.style.transform = `translateX(${-pos2}px)`
      }

      function up() {
        isDown = false
        if (pos1 >= row1Max) dir1 = -1; else if (pos1 <= 0) dir1 = 1
        if (pos2 >= row2Max) dir2 = -1; else if (pos2 <= 0) dir2 = 1
        timer1 = setInterval(step1, 50)
        timer2 = setInterval(step2, 50)
      }

      function pause() {
        if (timer1) { clearInterval(timer1); timer1 = null }
        if (timer2) { clearInterval(timer2); timer2 = null }
      }
      function resume() {
        if (timer1 || timer2) return
        timer1 = setInterval(step1, 50)
        timer2 = setInterval(step2, 50)
      }

      wrap.addEventListener('mousedown', down)
      wrap.addEventListener('mousemove', move)
      wrap.addEventListener('mouseup', up)
      wrap.addEventListener('mouseleave', up)
      wrap.addEventListener('mouseenter', () => { if (!isDown) pause() })
      wrap.addEventListener('mouseleave', () => { if (!isDown) resume() })
      wrap.addEventListener('touchstart', down, { passive: true })
      wrap.addEventListener('touchmove', move, { passive: false })
      wrap.addEventListener('touchend', up)
    })
  })
}

// ==================== 生命周期 ====================
onMounted(() => {
  initMap()
  initLogoCarousel()
})
</script>

<style scoped>
* { margin: 0; padding: 0; box-sizing: border-box; }

.page-wrapper {
  background: linear-gradient(145deg, #f8f0fc 0%, #f5eefc 50%, #f6f0fc 100%);
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
  color: #3a2438;
  line-height: 1.6;
  min-height: 100vh;
}

.banner {
  width: 100%;
}
.banner img {
  width: 100%;
  height: auto;
  display: block;
}

html { scroll-behavior: smooth; }

.container {
  max-width: 1080px;
  margin: 0 auto;
  padding: 2rem 1.5rem 3rem;
}

.brand-tags {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  flex-wrap: wrap;
}

.brand-tags span {
  background: #ede0f5;
  padding: 0.25rem 0.9rem;
  border-radius: 50px;
  font-size: 0.78rem;
  color: #8e5ab6;
  letter-spacing: 1px;
}

/* 品类展示 */
.category-section {
  margin: 1.5rem 0;
  padding: 0;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 1rem;
  margin-top: 1rem;
  padding: 0 1rem;
}

.category-chip {
  text-align: center;
  background: #f8f5fc;
  border-radius: 1.2rem;
  padding: 1rem 0.5rem;
  border: 1px solid #ede0f5;
  transition: transform 0.2s, box-shadow 0.2s;
}

.category-chip:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(150, 90, 180, 0.12);
}

.cat-icon-circle {
  width: 48px;
  height: 48px;
  margin: 0 auto 0.5rem;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
}

.cat-name {
  font-size: 0.75rem;
  color: #8e5ab6;
  font-weight: 500;
}

/* 品牌展示区 */
.brand-showcase {
  margin: 1.5rem 0 2rem;
  background: transparent;
  border-radius: 1.5rem;
  padding: 0;
  border: 1px solid transparent;
}

.showcase-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding: 1.2rem 1rem 0.8rem;
  margin-bottom: 0.8rem;
}

.showcase-title {
  font-weight: 550;
  color: #8e5ab6;
  font-size: 0.95rem;
  display: flex;
  align-items: center;
  gap: 6px;
}

.showcase-desc { font-size: 0.72rem; color: #c790d8; }

.logo-carousel-wrap {
  position: relative;
  overflow-x: hidden;
  overflow-y: visible;
  padding: 0.5rem 0;
}

.logo-carousel-row {
  display: flex;
  flex-wrap: nowrap;
  gap: 10px;
  will-change: transform;
  padding: 0 6px;
}

.logo-carousel-row + .logo-carousel-row {
  margin-top: 0.8rem;
}

.logo-item {
  flex: 0 0 auto;
  width: 110px;
  min-width: 0;
  text-align: center;
  background: #f8f5fc;
  border-radius: 1rem;
  padding: 0.8rem 0.5rem;
  border: 1px solid #e8d5f5;
  transition: transform 0.2s, box-shadow 0.2s;
}

.logo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(150, 90, 180, 0.12);
}

.logo-circle {
  width: 80px;
  height: 80px;
  margin: 0 auto;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: #fff;
}

.logo-circle img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.showcase-footer {
  text-align: center;
  font-size: 0.68rem;
  color: #a070c0;
  margin-top: 0.8rem;
  padding-bottom: 1.2rem;
  letter-spacing: 2px;
}

/* 平台区域 */
.platform-section { margin: 1.5rem 0; }

.section-card {
  background: transparent;
  border-radius: 1.5rem;
  padding: 0;
  border: 1px solid transparent;
}

.section-heading {
  font-size: 1.15rem;
  font-weight: 550;
  color: #5a2d68;
  margin-bottom: 1.2rem;
  padding: 0 1rem;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  text-align: left;
  gap: 8px;
}

/* 平台按钮 */
.platform-links { display: flex; flex-direction: column; gap: 0.8rem; padding: 0 1.5rem; }

.platform-btn {
  display: flex;
  align-items: center;
  gap: 12px;
  background: #f8f0fc;
  border-radius: 1rem;
  padding: 0.9rem 1.2rem;
  text-decoration: none;
  font-weight: 500;
  color: #6a3d8e;
  border: 1px solid #e0c8f0;
  transition: all 0.2s;
  cursor: pointer;
}

.platform-btn:hover {
  background: #e8d0f5;
  border-color: #c590d8;
  transform: translateX(4px);
}

.platform-btn .p-icon {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.platform-btn .p-icon img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.platform-btn .p-text { flex: 1; }
.platform-btn .p-name { font-size: 0.92rem; font-weight: 600; }
.platform-btn .p-desc { font-size: 0.72rem; color: #a070c0; margin-top: 2px; }
.platform-btn .p-arrow { color: #c590d8; font-size: 1.1rem; }

.section-tip {
  font-size: 0.68rem;
  color: #c18ad0;
  margin-top: 1rem;
  padding: 0 1.5rem;
  text-align: center;
}

/* 二维码一行展示 */
.qr-row {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  align-items: flex-start;
  margin-top: 1rem;
  padding: 1.2rem 1.5rem 0;
  border-top: 1px solid transparent;
}

.qr-card {
  text-align: center;
  flex: 1;
  max-width: 160px;
}

.qr-box {
  width: 130px;
  height: 130px;
  margin: 0 auto 0.5rem;
  border-radius: 0.8rem;
  background: #f5f0fa;
  border: 2px dashed #d8c8e8;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.qr-box img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.qr-box img[src=""] { display: none; }

.qr-box:has(img[src=""])::before {
  content: '扫码下单';
  font-size: 0.72rem;
  color: #c790d8;
}

.qr-label {
  font-size: 0.78rem;
  font-weight: 500;
  color: #8e5ab6;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
}

.qr-label img { width: 18px; height: 18px; object-fit: contain; }

/* 地图 + 联系方式 */
.contact-grid {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 1.2rem;
  margin-top: 1rem;
  padding: 0 1.5rem 1.5rem;
  align-items: center;
}

.contact-list {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  align-items: flex-start;
  justify-content: center;
}

.contact-chip {
  background: #f5eefc;
  padding: 0.5rem 0.8rem;
  border-radius: 1rem;
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.82rem;
  border: 1px solid #e8d5f5;
  width: 260px;
}

.contact-icon { flex-shrink: 0; }

.contact-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
}

.contact-chip img { width: 16px; height: 16px; object-fit: contain; flex-shrink: 0; }
.contact-chip a { color: #b890d0; text-decoration: none; white-space: nowrap; }
.contact-chip a:hover { text-decoration: underline; }
.contact-chip .tel-divider { color: #a070c0; padding: 0 4px; }
.contact-chip .contact-content span { color: #b890d0; white-space: nowrap; }
.contact-chip a:hover { text-decoration: underline; }

.copy-btn {
  background: #e0d0f0;
  border: none;
  border-radius: 50px;
  padding: 2px 10px;
  font-size: 0.7rem;
  cursor: pointer;
  color: #8e5ab6;
  margin-left: 4px;
  transition: background 0.2s;
}

.copy-btn:hover { background: #d0c0e8; }

/* 高德地图 */
.map-container {
  position: relative;
  border-radius: 1rem;
  overflow: hidden;
  border: 1px solid #e8d5f5;
  height: 220px;
  cursor: pointer;
  display: block;
  text-decoration: none;
  transition: box-shadow 0.2s;
}

.map-container:hover { box-shadow: 0 8px 24px rgba(142, 90, 182, 0.15); }

.map-container.map-loading {
  background: #f0e5f5;
  display: flex;
  align-items: center;
  justify-content: center;
}

.map-container.map-loading::before {
  content: '地图加载中...';
  font-size: 0.8rem;
  color: #a070c0;
}

/* 联系方式区域容器 */
.contact-section {
  border-radius: 1.5rem;
  padding: 0;
  margin-top: 1rem;
  border: 1px solid transparent;
}

.trust-badges {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  flex-wrap: wrap;
  padding: 0 1.5rem 1.5rem;
  font-size: 0.75rem;
  color: #b68bca;
}

/* 页脚 */
.footer {
  text-align: center;
  margin-top: 2rem;
  font-size: 0.72rem;
  color: #c590d8;
  padding: 1.5rem 1rem;
  border-top: 1px solid #ede0f5;
}

.footer .icp { margin-top: 0.5rem; }
.footer .icp a { color: #c590d8; text-decoration: none; }
.footer .icp a:hover { color: #a070c0; text-decoration: underline; }
.footer .beian-link { display: inline-flex; align-items: center; gap: 3px; }

/* 响应式 */
@media (max-width: 640px) {
  .page-wrapper { padding: 0; }
  .container { padding: 0.8rem 0.6rem 2rem; }
  .banner img { width: 100%; }
  .brand h1 { font-size: 1.8rem; letter-spacing: 2px; }
  .section-card { padding: 0; }
  .contact-section { padding: 0; }
  .category-section { padding: 0; }
  .brand-showcase { padding: 0; }
  .section-heading { padding: 0 0.6rem; }
  .platform-links { padding: 0 0.6rem; }
  .section-tip { padding: 0 0.6rem; }
  .qr-row { padding: 1.2rem 0.6rem 0; }
  .contact-grid { padding: 0 0.6rem 1.5rem; }
  .trust-badges { padding: 0 0.6rem 1.5rem; }
  .showcase-header { padding: 1.2rem 0.6rem 0.8rem; }
  .logo-carousel-wrap { padding: 0.5rem 0.6rem 0; overflow-x: hidden; overflow-y: visible; }
  .logo-carousel-row { gap: 8px; padding: 0 4px; }
  .logo-carousel-row + .logo-carousel-row { margin-top: 0.6rem; }
  .logo-item { flex: 0 0 calc(25% - 6px); padding: 0.6rem 0.3rem; }
  .logo-circle { width: 56px; height: 56px; }
  .category-grid { padding: 0 0.6rem; }
  .footer { padding: 1.5rem 0.6rem; }
  .category-grid { grid-template-columns: repeat(3, 1fr); gap: 0.6rem; }
  .category-chip { padding: 0.8rem 0.3rem; }
  .cat-icon-circle { width: 40px; height: 40px; font-size: 1.2rem; }
  .cat-name { font-size: 0.68rem; }
  .contact-grid { grid-template-columns: 1fr; padding: 0 0.6rem 1.5rem; gap: 0.8rem; align-items: center; justify-items: center; }
  .contact-list { flex-direction: row; flex-wrap: wrap; }
  .contact-chip { font-size: 0.75rem; padding: 0.45rem 0.6rem; }
  .contact-chip img { width: 14px; height: 14px; }
  .map-container { height: 220px !important; width: 100%; }
  .trust-badges { gap: 0.8rem; }
  .qr-row { gap: 0.8rem; }
  .qr-box { width: 90px !important; height: 90px !important; }
  .qr-card { max-width: 110px; }
  .qr-label { font-size: 0.7rem; }
}

/* 微信二维码弹窗 */
.qr-overlay {
  position: fixed; inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex; align-items: center; justify-content: center;
  z-index: 1000;
}
.qr-modal {
  background: #fff;
  border-radius: 1rem;
  padding: 1.5rem 2rem 2rem;
  position: relative;
  text-align: center;
}
.qr-modal img { max-width: 280px; width: 100%; border-radius: 0.5rem; }
.qr-close {
  position: absolute; top: 0.5rem; right: 0.7rem;
  background: none; border: none; font-size: 1.5rem; cursor: pointer; color: #666;
}
.qr-title { font-weight: 600; color: #b890d0; margin-bottom: 1rem; }
.qr-hint { font-size: 0.75rem; color: #a070c0; margin-top: 0.8rem; }

.qr-modal img {
  max-width: 280px; width: 100%; border-radius: 0.5rem;
  -webkit-touch-callout: default;
  -webkit-user-select: none;
}

.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

/* 动画 */
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.container { animation: fadeInUp 0.6s ease-out; }
.brand-showcase { animation: fadeInUp 0.6s ease-out 0.1s both; }
.platform-section { animation: fadeInUp 0.6s ease-out 0.2s both; }
.contact-section { animation: fadeInUp 0.6s ease-out 0.3s both; }
</style>
