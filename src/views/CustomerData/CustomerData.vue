<script setup lang="tsx">
import { ContentWrap } from '@/components/ContentWrap'
// import { useI18n } from '@/hooks/web/useI18n'
// import { ElTag } from 'element-plus'
import { Table, TableColumn } from '@/components/Table'
import { getTableListApi } from '@/api/table'
import { TableData } from '@/api/table/types'
import { ref } from 'vue'

// const { t } = useI18n()

interface Params {
  pageIndex?: number
  pageSize?: number
}

const columns: TableColumn[] = [
  {
    field: 'index',
    label: 'No.',
    type: 'index'
  },
  {
    field: 'title',
    label: 'Acc'
  },
  {
    field: 'author',
    label: 'Customer Name'
  },
  {
    field: 'pageviews',
    label: 'Area Code'
  },
  {
    field: 'title',
    label: 'Delivery Remarks'
  },
  {
    field: 'content',
    label: 'Billing Address',
    children: [
      {
        field: 'title',
        label: 'Address 1'
      },
      {
        field: 'title',
        label: 'Address 2'
      },
      {
        field: 'title',
        label: 'Address 3'
      },
      {
        field: 'title',
        label: 'Address 4'
      }
    ]
  },
  {
    field: 'title',
    label: 'Area'
  },
  {
    field: 'title',
    label: 'Area Name'
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
  <ContentWrap :title="'Customer Data'" :subtitle="'Total: 80 customers'">
    <Table
      :columns="columns"
      :data="tableDataList"
      :loading="loading"
      :defaultSort="{ prop: 'display_time', order: 'descending' }"
    />
  </ContentWrap>
</template>
<style>
th[colspan='4'] .cell {
  text-align: center;
}
</style>
