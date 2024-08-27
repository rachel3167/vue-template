<script setup lang="tsx">
import { ContentWrap } from '@/components/ContentWrap'
// import { useI18n } from '@/hooks/web/useI18n'
import { Table, TableColumn } from '@/components/Table'
import { getTableListApi } from '@/api/table'
import { TableData } from '@/api/table/types'
import { ref } from 'vue'

interface Params {
  pageIndex?: number
  pageSize?: number
}

const columns: TableColumn[] = [
  {
    field: 'index',
    type: 'index',
    label: 'No.'
  },
  {
    field: 'author',
    label: 'Customer Name'
  },
  {
    field: 'title',
    label: 'Area Code'
  },
  {
    field: 'title',
    label: 'Billing Address1'
  },
  {
    field: 'title',
    label: 'Billing Address2'
  },
  {
    field: 'title',
    label: 'Billing Address3'
  },
  {
    field: 'title',
    label: 'Billing Address4'
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
</script>

<template>
  <ContentWrap :title="'Cash Account'" :subtitle="'Total: 80 customers'">
    <Table
      :columns="columns"
      :data="tableDataList"
      :loading="loading"
      :defaultSort="{ prop: 'display_time', order: 'descending' }"
    />
  </ContentWrap>
</template>

<style>
/* table {
  table-layout: auto !important;
}

colgroup col:nth-child(1) {
  width: 70px;
}

.el-table__row td:nth-child(1) .cell {
  width: 70px !important;
}

.el-table__header th:nth-child(1) {
  width: 70px;
}

.el-table__header th:nth-child(1) .cell {
  width: 70px;
} */
</style>
