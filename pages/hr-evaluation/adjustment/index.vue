<template>
  <div class="page-container">
    <PageHeader />
    <div class="page-content">
      <!-- 조정단계 평가 대상자 통계 -->
      <div class="w-full">
        <div class="evaluation-stats mb-5">
          <h3 class="text-lg font-medium mb-10">조정단계 평가대상자</h3>
          <div class="stats-grid grid grid-cols-7 gap-4 text-center">
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">수</div>
              <div class="stat-value text-xl font-bold">9</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">S</div>
              <div class="stat-value text-xl font-bold">1</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">A</div>
              <div class="stat-value text-xl font-bold">8</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">B</div>
              <div class="stat-value text-xl font-bold">0</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">C</div>
              <div class="stat-value text-xl font-bold">0</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">D</div>
              <div class="stat-value text-xl font-bold">0</div>
            </div>
            <div class="stat-item p-3 border rounded-md">
              <div class="stat-label">1차조정</div>
              <div class="stat-value text-xl font-bold">0</div>
            </div>
          </div>
        </div>
        <UiTable
          v-model="gradeDistributionData"
          bordered
          :scrollable="true"
          max-height="calc(100vh - 300px)"
        >
          <template #colgroup>
            <col style="width: 200px" />
            <col style="width: auto" />
            <col style="width: auto" />
            <col style="width: auto" />
            <col style="width: auto" />
          </template>
          <template #header>
            <th v-for="header in gradeDistributionHeaders" :key="header.key">
              {{ header.title }}
            </th>
          </template>
          <template #body="{ rows }">
            <tr v-for="row in rows" :key="row.id">
              <td class="txt-l">{{ row.department }}</td>
              <td>{{ row.target }}</td>
              <td class="txt-r">{{ row.orgEval }}</td>
              <td class="txt-r">
                <a href="#" class="underline">{{ row.perfEval }}</a>
              </td>
              <td class="txt-r">
                <a href="#" class="underline">{{ row.compEval }}</a>
              </td>
              <td class="txt-r">{{ row.attendance }}</td>
              <td class="txt-r">{{ row.certificate }}</td>
              <td class="txt-r">{{ row.example }}</td>
              <td class="txt-r">{{ row.service }}</td>
              <td class="txt-r">{{ row.calculatedScore }}</td>
              <td>{{ row.calculatedGrade }}</td>
              <td class="">
                <UiSelect
                  :id="`${row.id}coordGradeId`"
                  data-col="coordGradeId"
                  class="saveTarget"
                  :options="[
                    { value: '', label: '선택' },
                    { value: 'RG00001', label: 'S' },
                    { value: 'RG00002', label: 'A' },
                    { value: 'RG00003', label: 'B' },
                    { value: 'RG00004', label: 'C' },
                    { value: 'RG00005', label: 'D' }
                  ]"
                />
              </td>
              <td class="txt-l">
                <UiTextarea
                  :id="`coordReason${row.id}`"
                  name="coordReason"
                  data-col="coordReason"
                  maxlength="1500"
                  size="small"
                  >{{ row.adjustmentReason }}</UiTextarea
                >
              </td>
            </tr>
          </template>
        </UiTable>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, inject } from 'vue'
  import PageHeader from './comp/PageHeader.vue'

  // 로고 텍스트 설정
  const logoText = inject('logoText')
  logoText.value = '조정평가'
  const gradeDistributionHeaders = ref([
    { key: 'department', title: '부서', width: '80px' },
    { key: 'target', title: '평가대상', width: '50px' },
    { key: 'orgEval', title: '조직평가', width: '40px' },
    { key: 'perfEval', title: '성과평가', width: '40px' },
    { key: 'compEval', title: '역량평가', width: '40px' },
    { key: 'attendance', title: '근태', width: '40px' },
    { key: 'certificate', title: '자격증', width: '40px' },
    { key: 'example', title: '예시', width: '40px' },
    { key: 'service', title: '복무사항', width: '40px' },
    { key: 'calculatedScore', title: '산출점수', width: '30px' },
    { key: 'calculatedGrade', title: '산출등급', width: '30px' },
    { key: 'firstAdjustment', title: '1차조정', width: '30px' },
    { key: 'adjustmentReason', title: '조정사유', width: '110px' }
  ])

  const gradeDistributionData = ref([
    {
      id: 1,
      department: 'ESM 고객관리팀>공공클라우드 영업팀',
      target: '강영호 주임',
      orgEval: '70',
      perfEval: '',
      compEval: '60',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '13',
      calculatedGrade: 'S',
      firstAdjustment: 'S',
      adjustmentReason: ''
    },
    {
      id: 2,
      department: 'ESM 고객관리팀>공공클라우드 영업팀',
      target: '김민준 사원',
      orgEval: '65',
      perfEval: '',
      compEval: '58',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '12',
      calculatedGrade: 'A',
      firstAdjustment: 'A',
      adjustmentReason: ''
    },
    {
      id: 3,
      department: 'ESM 고객관리팀>민간클라우드 영업팀',
      target: '박서연 과장',
      orgEval: '72',
      perfEval: '',
      compEval: '63',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '13.5',
      calculatedGrade: 'S',
      firstAdjustment: 'S',
      adjustmentReason: ''
    },
    {
      id: 4,
      department: 'ESM 고객관리팀>민간클라우드 영업팀',
      target: '이지훈 대리',
      orgEval: '68',
      perfEval: '',
      compEval: '59',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '12.7',
      calculatedGrade: 'A',
      firstAdjustment: 'A',
      adjustmentReason: ''
    },
    {
      id: 5,
      department: 'ESM 개발팀>백엔드 개발팀',
      target: '최수아 책임',
      orgEval: '74',
      perfEval: '',
      compEval: '67',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '14.1',
      calculatedGrade: 'S',
      firstAdjustment: 'A',
      adjustmentReason: '팀 내 S등급 비율 초과'
    },
    {
      id: 6,
      department: 'ESM 개발팀>백엔드 개발팀',
      target: '정도윤 선임',
      orgEval: '67',
      perfEval: '',
      compEval: '61',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '12.8',
      calculatedGrade: 'A',
      firstAdjustment: 'A',
      adjustmentReason: ''
    },
    {
      id: 7,
      department: 'ESM 개발팀>프론트엔드 개발팀',
      target: '김하은 주임',
      orgEval: '63',
      perfEval: '',
      compEval: '56',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '11.9',
      calculatedGrade: 'B',
      firstAdjustment: 'B',
      adjustmentReason: ''
    },
    {
      id: 8,
      department: 'ESM 개발팀>프론트엔드 개발팀',
      target: '이준호 사원',
      orgEval: '61',
      perfEval: '',
      compEval: '54',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '11.5',
      calculatedGrade: 'B',
      firstAdjustment: 'B',
      adjustmentReason: ''
    },
    {
      id: 9,
      department: 'ESM 기획팀>서비스 기획팀',
      target: '박지민 과장',
      orgEval: '70',
      perfEval: '',
      compEval: '62',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '13.2',
      calculatedGrade: 'A',
      firstAdjustment: 'A',
      adjustmentReason: ''
    },
    {
      id: 10,
      department: 'ESM 기획팀>서비스 기획팀',
      target: '최민서 대리',
      orgEval: '64',
      perfEval: '',
      compEval: '57',
      attendance: '',
      certificate: '',
      example: '',
      service: '',
      calculatedScore: '12.1',
      calculatedGrade: 'B',
      firstAdjustment: 'A',
      adjustmentReason: '업무 성과 및 기여도 고려하여 상향 조정'
    }
  ])

  const handleEdit = row => {
    console.log('수정:', row)
    // 수정 로직 구현
  }
</script>

<style scoped>
  /* 조정단계 평가 대상자 통계 */
  .evaluation-stats {
    margin-bottom: 1.25rem;
  }

  .stats-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 1rem;
    text-align: center;
  }

  .stat-item {
    padding: 0.75rem;
    border: 1px solid #e5e7eb;
    border-radius: 0.375rem;
  }

  .stat-label {
    font-size: 0.875rem;
    color: #6b7280;
    margin-bottom: 0.25rem;
  }

  .stat-value {
    font-size: 1.25rem;
    font-weight: 700;
  }
</style>
