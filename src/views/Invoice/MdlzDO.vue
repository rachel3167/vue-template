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
    field: 'pageviews',
    label: 'No.'
  },
  {
    field: 'display_time',
    label: 'Date'
  },
  {
    field: 'title',
    label: 'Sales Invoice'
  },
  {
    field: 'title',
    label: 'Delivery Term'
  },
  {
    field: 'pageviews',
    label: 'Our DO No'
  },
  {
    field: 'title',
    label: 'Ref. No'
  },
  {
    field: 'title',
    label: 'Customer'
  },
  {
    field: 'title',
    label: 'Customer Name'
  },
  {
    field: 'title',
    label: 'Agent#'
  },
  {
    field: 'title',
    label: 'Group'
  },
  {
    field: 'title',
    label: 'Stock#'
  },
  {
    field: 'title',
    label: 'Barcode'
  },
  {
    field: 'title',
    label: 'Stock Control'
  },
  {
    field: 'title',
    label: 'Description'
  },
  {
    field: 'title',
    label: 'Stock Name2'
  },
  {
    field: 'title',
    label: 'Stock Location'
  },
  {
    field: 'title',
    label: 'Stock Control'
  },
  {
    field: 'title',
    label: 'Qty'
  },
  {
    field: 'title',
    label: 'UOM'
  },
  {
    field: 'title',
    label: 'Unit Price'
  },
  {
    field: 'title',
    label: 'Amount'
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
  <ContentWrap :title="'Invoice List - QnE'">
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

.scrollable-table th {
  width: 100px;
}
</style>
