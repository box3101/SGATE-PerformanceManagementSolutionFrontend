<template>
  <div class="page-container container-large">
    <div class="page-header com-header">
      <div class="header-left">
        <div class="select-group">
          <div class="select-item flex items-center">
            <span class="select-label flex items-center"> 기준년도 </span>
            <UiSelect
              v-model="selectedYear"
              :options="yearOptions"
              size="medium"
              placeholder="선택"
              width="150px"
            />
          </div>
        </div>
      </div>
      <div class="header-right">
        <div class="button-group">
          <UiButton type="button" variant="secondary-line" size="medium" @click="handleAttachment">
            <i class="icon-md icon-paper-clip"></i>
            파일첨부
          </UiButton>
          <UiButton type="button" variant="primary-line" size="medium" @click="handleSave">
            저장
          </UiButton>
        </div>
      </div>
    </div>

    <!-- 컨텐츠 영역 -->
    <div class="page-content">
      <!-- TinyMCE 에디터 영역 -->
      <div class="editor-container">
        <div class="editor-wrapper">
          <TinyEditor v-model="content" :height="700" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'

  // 로고 텍스트 설정
  const logoText = inject('logoText')
  logoText.value = '업무기술서 템플릿'

  const selectedYear = ref('2025')
  const yearOptions = ref([
    { value: '2025', label: '2025년' },
    { value: '2024', label: '2024년' },
    { value: '2023', label: '2023년' }
  ])

  const handleExcelDownload = () => {
    // 엑셀 다운로드 로직 구현
  }

  const handleSave = () => {
    // 저장 로직 구현
  }
</script>

<style scoped>
  .editor-container {
    margin: 0 auto;
    width: 100%;
  }
</style>
