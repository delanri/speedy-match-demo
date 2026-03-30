<script setup lang="ts">
import { ref } from 'vue'

// [State Management] 控制核心UI流转的状态机
const isAnalyzing = ref(false)
const hasResult = ref(false)
const showPanels = ref(false) // 延迟渲染面板，制造视觉冲击力
const matchScore = ref(0)

// [Mock Data] 模拟简历特征提取结果
const resume = ref({
  name: '顶级AI架构师 Delanri',
  experience: '独立开发者 | Vibe Coding 大师',
  skills: ['Vue3', 'TypeScript', 'AI协同', 'UI/UX设计', 'Prompt Engineering'],
  summary: '拥有极强的产品直觉与AI驾驭能力，能够精准拆解需求，指挥大模型高效完成商业级交付。'
})

// [Mock Data] 模拟目标岗位画像
const jobDescription = ref({
  title: '高级AI协同前端工程师',
  reqSkills: ['Vue3', 'TypeScript', 'AI驱动开发', '极客审美', '快速迭代'],
  description: '寻找能够敏锐识别产品痛点，利用AI工具快速构建MVP的高效开发者。'
})

// [Core Logic] 模拟大模型深度匹配的非阻塞异步计算流
const startAnalysis = () => {
  isAnalyzing.value = true
  hasResult.value = false
  showPanels.value = false
  matchScore.value = 0
  
  // 模拟网络请求与AI分析耗时，保障主线程UI渲染的绝对丝滑
  setTimeout(() => {
    isAnalyzing.value = false
    hasResult.value = true
    
    // [Animation Logic] 分数滚动反馈，提升用户的等待心流体验
    let current = 0
    const timer = setInterval(() => {
      current += 2
      if (current >= 99) {
        matchScore.value = 99
        clearInterval(timer)
        // 核心交互细节：分数定格后延迟展开左右面板，制造“数据推演完成”的呼吸感
        setTimeout(() => {
          showPanels.value = true
        }, 500)
      } else {
        matchScore.value = current
      }
    }, 20)
  }, 3000)
}
</script>

<template>
  <div class="match-dashboard">
    <header class="header">
      <div class="logo">SpeedyMatch <span class="highlight">AI</span></div>
      <div class="subtitle">智能简历解析与JD匹配引擎 · Delanri重构版</div>
    </header>

    <main class="dashboard-content">
      <transition name="push-from-left">
        <section v-if="showPanels" class="panel glass-card left-panel">
          <h2 class="panel-title">📄 提取简历特征</h2>
          <div class="info-block">
            <h3>{{ resume.name }}</h3>
            <p class="desc">{{ resume.summary }}</p >
            <div class="tags">
              <span v-for="skill in resume.skills" :key="skill" class="tag resume-tag">{{ skill }}</span>
            </div>
          </div>
        </section>
      </transition>

      <section class="center-radar" :class="{'flex-grow': !showPanels}">
        <div v-if="!hasResult && !isAnalyzing" class="action-center">
          <button class="cyber-btn" @click="startAnalysis">启动 AI 深度匹配</button>
        </div>

        <div v-else class="glowing-ring" :class="{ 'scanning': isAnalyzing, 'black-hole-vortex': isAnalyzing }">
          <div v-if="isAnalyzing" class="ring-spinner"></div>
          <div v-else class="ring-static"></div>

          <div v-if="isAnalyzing" class="data-streams vortex-streams">
            <div class="stream s1"></div>
            <div class="stream s2"></div>
            <div class="stream s3"></div>
            <div class="stream s4"></div>
            <div class="stream s5"></div>
            <div class="stream s6"></div>
          </div>

          <div class="score-circle">
            <span v-if="isAnalyzing" class="scanning-text">
              数据接管中<span class="loading-dots"></span>
            </span>
            <template v-else>
              <span class="score-number">{{ matchScore }}<span class="percent">%</span></span>
              <span class="score-label">极度匹配</span>
            </template>
          </div>
        </div>
      </section>

      <transition name="push-from-right">
        <section v-if="showPanels" class="panel glass-card right-panel">
          <h2 class="panel-title">🎯 目标职位画像</h2>
          <div class="info-block">
            <h3>{{ jobDescription.title }}</h3>
            <p class="desc">{{ jobDescription.description }}</p >
            <div class="tags">
              <span v-for="skill in jobDescription.reqSkills" :key="skill" class="tag jd-tag">{{ skill }}</span>
            </div>
          </div>
        </section>
      </transition>
    </main>
  </div>
</template>

<style>
:root {
  --bg-color: #050b14;
  --cyan-glow: #00f3ff;
  --panel-bg: rgba(16, 25, 43, 0.6);
}

body {
  margin: 0; padding: 0;
  background-color: var(--bg-color);
  color: #e2e8f0;
  font-family: 'Inter', system-ui, sans-serif;
  overflow: hidden;
}

.match-dashboard {
  min-height: 100vh; display: flex; flex-direction: column;
  padding: 2rem 4rem;
  background: radial-gradient(circle at 50% 50%, #0a192f 0%, #000000 100%);
}

.header { text-align: center; margin-bottom: 4rem; }
.logo { font-size: 2.5rem; font-weight: 800; letter-spacing: 2px; }
.highlight { color: var(--cyan-glow); text-shadow: 0 0 10px rgba(0, 243, 255, 0.5); }
.subtitle { color: #94a3b8; margin-top: 0.5rem; font-size: 1rem; }

.dashboard-content {
  display: flex; justify-content: center; align-items: center; gap: 2rem; height: 60vh;
}

.center-radar { display: flex; justify-content: center; align-items: center; transition: all 0.8s ease; }
.flex-grow { flex: 1; }

.left-panel { transform-origin: left center; }
.right-panel { transform-origin: right center; }

.panel {
  flex: 1; height: 100%; padding: 2rem; border-radius: 16px;
  border: 1px solid rgba(0, 243, 255, 0.1);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px); background: var(--panel-bg);
  transition: all 0.3s ease;
}
.panel:hover {
  border-color: rgba(0, 243, 255, 0.3);
  box-shadow: 0 8px 32px rgba(0, 243, 255, 0.1);
}

.panel-title {
  color: var(--cyan-glow); font-size: 1.2rem; margin-bottom: 1.5rem;
  border-bottom: 1px solid rgba(0, 243, 255, 0.2); padding-bottom: 0.8rem;
}

.info-block h3 { font-size: 1.4rem; margin-bottom: 0.5rem; }
.desc { color: #cbd5e1; line-height: 1.6; margin-bottom: 2rem; }
.tags { display: flex; flex-wrap: wrap; gap: 0.8rem; }
.tag { padding: 0.4rem 1rem; border-radius: 20px; font-size: 0.85rem; font-weight: 600; }
.resume-tag { background: rgba(0, 243, 255, 0.1); color: var(--cyan-glow); border: 1px solid rgba(0, 243, 255, 0.3); }
.jd-tag { background: rgba(167, 139, 250, 0.1); color: #a78bfa; border: 1px solid rgba(167, 139, 250, 0.3); }

.action-center { display: flex; justify-content: center; }
.cyber-btn {
  background: transparent; color: var(--cyan-glow);
  border: 2px solid var(--cyan-glow); padding: 1rem 2rem;
  font-size: 1.2rem; font-weight: bold; border-radius: 8px;
  cursor: pointer; text-transform: uppercase; letter-spacing: 2px;
  box-shadow: 0 0 15px rgba(0, 243, 255, 0.2); transition: all 0.3s;
}
.cyber-btn:hover {
  background: var(--cyan-glow); color: #000;
  box-shadow: 0 0 30px rgba(0, 243, 255, 0.6);
}

.glowing-ring {
  width: 250px; height: 250px; border-radius: 50%;
  display: flex; justify-content: center; align-items: center; position: relative;
  transition: all 0.5s ease;
}

.black-hole-vortex {
  background-color: #000;
  box-shadow: 0 0 60px rgba(0, 243, 255, 0.8), inset 0 0 40px #000;
}

.ring-spinner {
  position: absolute; inset: 0; border-radius: 50%;
  background: conic-gradient(from 0deg, rgba(0, 243, 255, 0.1), rgba(0, 243, 255, 0.8), rgba(0, 243, 255, 0.1));
  animation: spin 1s linear infinite;
}
.ring-spinner::before {
  content: ''; position: absolute; inset: 8px; 
  border-radius: 50%; background-color: #000;
}

.ring-static {
  position: absolute; inset: 0; border-radius: 50%;
  background: conic-gradient(from 0deg, rgba(0, 243, 255, 0.1), rgba(0, 243, 255, 0.5), rgba(0, 243, 255, 0.1));
  box-shadow: 0 0 20px rgba(0, 243, 255, 0.2);
}
.ring-static::before {
  content: ''; position: absolute; inset: 5px; border-radius: 50%; background-color: var(--bg-color);
}

/* 螺旋吸入粒子动画 */
.vortex-streams {
  position: absolute; inset: 0;
  display: flex; justify-content: center; align-items: center;
  pointer-events: none; z-index: 0;
}
.stream {
  position: absolute; width: 3px; height: 12px; background: var(--cyan-glow);
  border-radius: 2px; box-shadow: 0 0 8px var(--cyan-glow); opacity: 0;
}

/* 利用初始旋转角度和translate实现螺旋坠落 */
.s1 { animation: spiralIn1 1.2s cubic-bezier(0.5, 0, 0.2, 1) infinite 0s; }
.s2 { animation: spiralIn2 1.4s cubic-bezier(0.5, 0, 0.2, 1) infinite 0.2s; }
.s3 { animation: spiralIn3 1.1s cubic-bezier(0.5, 0, 0.2, 1) infinite 0.4s; }
.s4 { animation: spiralIn4 1.3s cubic-bezier(0.5, 0, 0.2, 1) infinite 0.1s; }
.s5 { animation: spiralIn5 1.5s cubic-bezier(0.5, 0, 0.2, 1) infinite 0.5s; }
.s6 { animation: spiralIn6 1.0s cubic-bezier(0.5, 0, 0.2, 1) infinite 0.3s; }

@keyframes spiralIn1 { 0% { transform: rotate(0deg) translateY(-180px) scale(1.5); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(360deg) translateY(0) scale(0); opacity: 0; } }
@keyframes spiralIn2 { 0% { transform: rotate(60deg) translateY(-190px) scale(1.2); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(420deg) translateY(0) scale(0); opacity: 0; } }
@keyframes spiralIn3 { 0% { transform: rotate(120deg) translateY(-170px) scale(1.4); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(480deg) translateY(0) scale(0); opacity: 0; } }
@keyframes spiralIn4 { 0% { transform: rotate(180deg) translateY(-200px) scale(1.1); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(540deg) translateY(0) scale(0); opacity: 0; } }
@keyframes spiralIn5 { 0% { transform: rotate(240deg) translateY(-160px) scale(1.6); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(600deg) translateY(0) scale(0); opacity: 0; } }
@keyframes spiralIn6 { 0% { transform: rotate(300deg) translateY(-185px) scale(1.3); opacity: 0; } 20% { opacity: 1; } 100% { transform: rotate(660deg) translateY(0) scale(0); opacity: 0; } }

.score-circle { position: relative; z-index: 1; display: flex; flex-direction: column; align-items: center; }
.scanning-text { color: var(--cyan-glow); font-size: 1.2rem; letter-spacing: 2px; animation: blink 1s infinite; }

.loading-dots::after {
  content: ''; display: inline-block; width: 1.5em; text-align: left;
  animation: dots 1.5s steps(4, end) infinite;
}
@keyframes dots {
  0% { content: ''; } 25% { content: '.'; } 50% { content: '..'; } 75% { content: '...'; } 100% { content: ''; }
}

.score-number { font-size: 4rem; font-weight: 900; color: #ffffff; text-shadow: 0 0 20px var(--cyan-glow); }
.percent { font-size: 2rem; color: var(--cyan-glow); }
.score-label { margin-top: 0.5rem; color: var(--cyan-glow); font-size: 1rem; letter-spacing: 2px; text-transform: uppercase; }

/* 调整面板的挤入动画 */
.push-from-left-enter-active { transition: all 1s cubic-bezier(0.19, 1, 0.22, 1); }
.push-from-left-enter-from { opacity: 0; transform: translateX(-100px) scale(0.9); }

.push-from-right-enter-active { transition: all 1s cubic-bezier(0.19, 1, 0.22, 1); }
.push-from-right-enter-from { opacity: 0; transform: translateX(100px) scale(0.9); }

@keyframes spin { 100% { transform: rotate(360deg); } }
@keyframes blink { 50% { opacity: 0.5; } }
</style>