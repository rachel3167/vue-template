<script setup lang="tsx">
import { ContentWrap } from '@/components/ContentWrap'
import { useI18n } from '@/hooks/web/useI18n'
// import { useGuide } from '@/hooks/web/useGuide'
import { Table, TableColumn } from '@/components/Table'
import { getTableListApi } from '@/api/table'
import { TableData } from '@/api/table/types'
import { ref } from 'vue'
import { BaseButton } from '@/components/Button'

interface Params {
  pageIndex?: number
  pageSize?: number
}

const { t } = useI18n()

const columns: TableColumn[] = [
  {
    field: 'pageviews',
    label: 'No.'
  },
  {
    field: 'author',
    label: 'WH'
  },
  {
    field: 'title',
    label: 'Truck Plate Number'
  },
  {
    field: 'title',
    label: 'Checkbox'
  },
  {
    field: 'title',
    label: 'Vendor CO. Name'
  },
  {
    field: 'display_time',
    label: 'Vendor Short Name'
  },
  {
    field: 'title',
    label: 'Vendor / PIC Name'
  },
  {
    field: 'title',
    label: 'Vendor / PIC Contact'
  },
  {
    field: 'title',
    label: 'Tye of Truck (Ambience / Cold Chain)'
  },
  {
    field: 'title',
    label: 'Truck Size'
  },
  {
    field: 'title',
    label: 'Remarks'
  },
  {
    field: 'title',
    label: 'Capacity'
  },
  {
    field: 'title',
    label: 'Comm'
  },
  {
    field: 'action',
    label: t('tableDemo.action'),
    slots: {
      default: (data) => {
        return (
          <BaseButton type="primary" onClick={() => actionFn(data)}>
            Edit
          </BaseButton>
        )
      }
    }
  }
]

const loading = ref(true)

const tableDataList = ref<TableData[]>([])

const getTableList = async (params?: Params) => {
  const res = await getTableListApi(
    params || {
      pageIndex: 1,
      pageSize: 10
    }
  )
    .catch(() => {})
    .finally(() => {
      loading.value = false
    })
  if (res) {
    tableDataList.value = res.data.list
  }
}

getTableList()

const actionFn = (data: any) => {
  console.log(data)
}
</script>

<template>
  <ContentWrap :title="'Truck List'">
    <div class="table-function-container">
      <div class="table-function-left">
        <div class="date-picker-box">
          <label for="date-picker">Date:</label>
          <input type="date" id="date-picker" />
        </div>
        <div class="search-input-box">
          <input type="text" placeholder="Search..." />
          <span class="search-icon-box">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="search-icon"
              width="1em"
              height="1em"
              viewBox="0 0 24 24"
            >
              <path
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="1.5"
                d="m21 21l-4.343-4.343m0 0A8 8 0 1 0 5.343 5.343a8 8 0 0 0 11.314 11.314"
              />
            </svg>
          </span>
        </div>

        <div class="sort-by-box">
          <select id="sort-by-select">
            <option value="" selected disabled hidden>Sort by...</option>
            <option class="sort-by-option">Newest</option>
            <option class="sort-by-option">Oldest</option>
          </select>
          <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
            <path
              fill="none"
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="1.5"
              d="M5 20v-6m0-3V4m4 4h6M2 14h6m8-2h6M12 8V4m0 16v-9m7 1V4m0 16v-5"
              color="currentColor"
            />
          </svg>
        </div>
      </div>

      <div class="table-function-right">
        <div class="flex import-btn-box">
          <button class="custom-el-button"><span>Add New</span></button>
        </div>
      </div>
    </div>
    <Table
      class="scrollable-table"
      :columns="columns"
      :data="tableDataList"
      :loading="loading"
      :defaultSort="{ prop: 'display_time', order: 'descending' }"
    />
  </ContentWrap>
</template>

<style>
.scrollable-table {
  overflow-x: auto;
}

.table-function-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

.table-function-left {
  display: flex;
  justify-content: start;
  align-items: center;
  gap: 10px;
}

.date-picker-box {
  position: relative;
  display: flex;
  width: 100%;
  height: 35px;
  max-width: 210px;
  padding: 0 10px;
  cursor: pointer; /* This makes the entire box clickable */
  background-color: #fff;
  border: 1px solid rgb(217 217 217);
  border-radius: 6px;
  transition: all 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  align-items: center;
}

.date-picker-box label {
  margin-right: 5px;
  font-family: Nunito;
  font-size: 14px;
  font-weight: 400;
  color: grey;
  cursor: pointer; /* Ensures the label also triggers the date picker */
}

.date-picker-box input {
  z-index: 1;
  width: 100%;
  height: 100%;
  padding-left: 5px;
  font-family: Nunito;
  font-size: 14px;
  font-weight: 400;
  color: #333;
  cursor: pointer; /* Makes the input field clickable */
  background-color: #fff;
  border: none;
  border-radius: 6px;
  outline: none;
}

input[type='date']::-webkit-calendar-picker-indicator {
  cursor: pointer; /* Ensures the calendar icon, if displayed, has a pointer cursor */
}

.search-input-box {
  position: relative;
  width: 100%;
  height: 35px;
  max-width: 210px;
  background-color: #fff;
  border: 1px solid rgb(217 217 217);
  border-radius: 6px;
  transition: all 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.search-input-box input {
  position: relative;
  height: 100%;
  padding: 0 10px 0 32px;
  font-family: Nunito;
  font-size: 14px;
  font-weight: 400;
  color: #333;
  background-color: #fff;
  border: none;
  border-radius: 6px;
  outline: none;
}

.search-icon-box {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  width: 30px;
  height: 100%;
  background-color: #fff;
  border-radius: 6px;
  justify-content: center;
}

.search-icon,
.close-icon {
  position: absolute;
  top: 50%;
  width: 17px;
  height: 17px;
  font-size: 30px;
  cursor: pointer;
  transition: all 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.search-icon {
  color: black;
  transform: translateY(-50%);
}

.search-input-box.open .search-icon {
  transform: translateY(-50%) rotate(0);
}

.close-icon {
  right: -45px;
  padding: 5px;
  color: #fff;
  pointer-events: none;
  opacity: 0;
  transform: translateY(-50%);
}

.search-input-box.open .close-icon {
  pointer-events: auto;
  opacity: 1;
  transform: translateY(-50%) rotate(180deg);
}

.sort-by-box {
  position: relative;
}

select {
  z-index: 1;
  width: 100%;
  padding: 0 1em 0 0;
  margin: 0;
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
  cursor: inherit;
  background-color: transparent;
  border: none;
  outline: none;
  appearance: none;
}

select::-ms-expand {
  display: none;
}

#sort-by-select {
  width: 150px;
  height: 35px;
  padding: 3px 12px;
  font-family: Nunito;
  font-size: 14px;
  font-weight: 400;
  color: grey;
  text-indent: 1px;
  cursor: pointer;
  border: 1px solid rgb(217 217 217);
  border-radius: 5px;
}

#sort-by-select:focus {
  border: 1px solid rgb(217 217 217);
  border-radius: 5px;
}

.sort-by-box svg {
  position: absolute;
  top: 20%;
  right: 5%;
  width: 20px;
  height: 20px;
  text-align: center;
  pointer-events: none;
}

#sort-by-select .sort-by-option {
  padding: 30px;
  cursor: pointer;
  border: 1px solid rgb(217 217 217);
  outline: none;
  box-shadow: none;
  appearance: none;
}

.import-btn-box {
  display: flex;
  justify-content: center;
  align-items: center;
}

.custom-el-button {
  width: 160px;
  height: 35px;
  font-family: Nunito, sans-serif;
  font-size: 17px;
  font-weight: 600;
  color: #fff;
  cursor: pointer;
  background-color: #487bff;
  border: none;
  border-radius: 5px;
  outline: none;
  transition: all 0.5s;
}

.custom-el-button:hover {
  transform: scale(1.07);
}

@media (width <= 450px) {
  .table-function-left {
    flex-wrap: wrap;
    justify-content: start;
  }
}
</style>
