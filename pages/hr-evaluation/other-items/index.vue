<template>
  <div class="page-container">
    <PageHeader />
    <div class="flex-container">
      <div class="w-40p">
        <!-- 배분표 관리 테이블 -->
        <UiTable
          v-model="gradeDistributionData"
          title="배분표 관리"
          editable
          :canAddSortableButton="false"
          scrollable
          bordered
          max-height="calc(100vh - 200px)"
        >
          <template #colgroup>
            <col style="width: 40px" />
            <col style="width: auto" />
            <col style="width: 150px" />
            <col style="width: 40px" />
            <col style="width: 40px" />
          </template>
          <template #header="{ isAllSelected, selectAll }">
            <th>
              <UiCheckbox
                size="large"
                @click.stop
                :modelValue="isAllSelected"
                @update:modelValue="selectAll"
              />
            </th>
            <th v-for="header in gradeDistributionHeaders" :key="header.key">
              {{ header.title }}
            </th>
          </template>
          <template #body="{ rows, isRowSelected, toggleRowSelection }">
            <tr v-for="row in rows" :key="row.id">
              <td class="text-center">
                <UiCheckbox
                  size="large"
                  @click.stop
                  :modelValue="isRowSelected(row)"
                  @update:modelValue="() => toggleRowSelection(row)"
                />
              </td>
              <td class="text-center">
                {{ row.name }}
              </td>
              <td class="text-center">
                {{ row.type }}
              </td>
              <td class="text-center">
                {{ row.viewable ? 'Y' : 'N' }}
              </td>
              <td class="text-center">
                <UiButton variant="ghost" size="small" iconOnly>
                  <i class="icon-md icon-search"></i>
                </UiButton>
              </td>
            </tr>
          </template>
        </UiTable>
      </div>
      <div class="w-60p">
        <!-- 필터 섹션 - 확장형 -->
        <div class="filter-section">
          <div class="filter-header">
            <h3 class="filter-title">검색 조건</h3>
            <div class="filter-toggle">
              <UiButton variant="ghost" size="small" @click="toggleExpanded">
                <i
                  class="icon-sm"
                  :class="isExpanded ? 'icon-chevron-up' : 'icon-chevron-down'"
                ></i>
                {{ isExpanded ? '접기' : '더보기' }}
              </UiButton>
            </div>
          </div>

          <div class="filter-grid" v-show="isExpanded">
            <div class="filter-item">
              <label>직위</label>
              <UiSelect
                v-model="filters.positions"
                :options="positionOptions"
                multiple
                placeholder="조회 및 선택 하세요"
                size="small"
              />
            </div>
            <div class="filter-item">
              <label>직급</label>
              <UiSelect
                v-model="filters.grades"
                :options="gradeOptions"
                multiple
                placeholder="조회 및 선택 하세요"
                size="small"
              />
            </div>
            <div class="filter-item">
              <label>직무</label>
              <UiSelect
                v-model="filters.jobs"
                :options="jobOptions"
                multiple
                placeholder="조회 및 선택 하세요"
                size="small"
              />
            </div>
            <div class="filter-item">
              <label>부서</label>
              <UiSelect
                v-model="filters.departments"
                :options="departmentOptions"
                multiple
                placeholder="조회 및 선택 하세요"
                size="small"
              />
            </div>
            <div class="filter-item">
              <label>직원</label>
              <UiSelect
                v-model="filters.employees"
                :options="employeeOptions"
                multiple
                placeholder="조회 및 선택 하세요"
                size="small"
              />
            </div>
          </div>

          <div class="filter-actions">
            <div class="filter-result-info">
              <span class="result-count">총 {{ totalCount }}건</span>
              <span class="active-filters" v-if="activeFilterCount > 0">
                ({{ activeFilterCount }}개 조건 적용)
              </span>
            </div>
            <div class="button-group">
              <UiButton variant="secondary-line" @click="handleReset">
                <i class="icon-sm icon-refresh"></i>
                초기화
              </UiButton>
              <UiButton variant="primary" @click="handleSearch">
                <i class="icon-sm icon-search icon-white"></i>
                검색
              </UiButton>
            </div>
          </div>
        </div>

        <!-- 평가대상, 부서 , 직위, 직급,점수/가감점 테이블 -->
        <UiTable
          v-model="evaluationData"
          title="평가 항목 관리"
          scrollable
          bordered
          max-height="calc(100vh - 200px)"
          excelControls
          excelDownloadLabel="엑셀 다운로드"
          excelUploadLabel="엑셀 업로드"
        >
          <template #colgroup>
            <col style="width: 150px" />
            <col style="width: 150px" />
            <col style="width: 120px" />
            <col style="width: 120px" />
            <col style="width: 150px" />
          </template>
          <template #header>
            <th v-for="header in evaluationHeaders" :key="header.key">
              {{ header.title }}
            </th>
          </template>
          <template #header-right-end>
            <UiButton variant="primary" size="medium"> 저장 </UiButton>
          </template>
          <template #body="{ rows }">
            <tr v-for="row in rows" :key="row.id">
              <td class="text-center">{{ row.target }}</td>
              <td>{{ row.department }}</td>
              <td class="text-center">{{ row.position }}</td>
              <td class="text-center">{{ row.rank }}</td>
              <td class="text-center">
                <UiInput v-model="row.score" type="number" size="small" />
              </td>
            </tr>
          </template>
        </UiTable>
      </div>
    </div>
  </div>
</template>

<script setup>
  import PageHeader from './comp/PageHeader.vue'

  const gradeDistributionHeaders = [
    { key: 'name', title: '항목명' },
    { key: 'type', title: '유형' },
    { key: 'viewable', title: '가시화여부' },
    { key: 'view', title: '조회' }
  ]

  const gradeDistributionData = ref([
    { id: 1, name: '기타항목1', type: '텍스트', viewable: true },
    { id: 2, name: '기타항목2', type: '숫자', viewable: false },
    { id: 3, name: '기타항목3', type: '선택', viewable: true }
  ])

  const isExpanded = ref(true)
  const totalCount = ref(0)
  const activeFilterCount = ref(0)

  const filters = ref({
    positions: [],
    grades: [],
    jobs: [],
    departments: [],
    employees: []
  })

  const positionOptions = ref([
    { value: 'p1', label: '사원' },
    { value: 'p2', label: '대리' },
    { value: 'p3', label: '과장' },
    { value: 'p4', label: '차장' },
    { value: 'p5', label: '부장' }
  ])

  const gradeOptions = ref([
    { value: 'g1', label: '1급' },
    { value: 'g2', label: '2급' },
    { value: 'g3', label: '3급' },
    { value: 'g4', label: '4급' },
    { value: 'g5', label: '5급' }
  ])

  const jobOptions = ref([
    { value: 'j1', label: '영업' },
    { value: 'j2', label: '마케팅' },
    { value: 'j3', label: '인사' },
    { value: 'j4', label: '개발' },
    { value: 'j5', label: '디자인' }
  ])

  const departmentOptions = ref([
    { value: 'd1', label: '영업부' },
    { value: 'd2', label: '마케팅부' },
    { value: 'd3', label: '인사부' },
    { value: 'd4', label: 'IT부서' }
  ])

  const employeeOptions = ref([
    { value: 'e1', label: '홍길동' },
    { value: 'e2', label: '김철수' },
    { value: 'e3', label: '이영희' }
  ])

  const toggleExpanded = () => {
    isExpanded.value = !isExpanded.value
  }

  const handleSearch = () => {
    // 검색 로직 구현
    totalCount.value = 15
    activeFilterCount.value = Object.values(filters.value).filter(val =>
      Array.isArray(val) ? val.length > 0 : !!val
    ).length
  }

  const handleReset = () => {
    Object.keys(filters.value).forEach(key => {
      filters.value[key] = []
    })
    activeFilterCount.value = 0
  }

  const evaluationHeaders = [
    { key: 'target', title: '평가대상' },
    { key: 'department', title: '부서' },
    { key: 'position', title: '직위' },
    { key: 'rank', title: '직급' },
    { key: 'score', title: '점수/가감점' }
  ]

  const evaluationData = ref([
    { id: 1, target: '홍길동', department: '영업부', position: '사원', rank: '1급', score: '5' },
    { id: 2, target: '김철수', department: '마케팅부', position: '대리', rank: '2급', score: '4' },
    { id: 3, target: '이영희', department: '인사부', position: '과장', rank: '3급', score: '3' },
    { id: 4, target: '최민수', department: 'IT부서', position: '차장', rank: '4급', score: '2' },
    { id: 5, target: '박영희', department: '영업부', position: '부장', rank: '5급', score: '1' }
  ])

  const handleEdit = row => {
    console.log('수정:', row)
    // 수정 로직 구현
  }
</script>
