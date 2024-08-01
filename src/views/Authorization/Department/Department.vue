<script setup lang="tsx">
import { ContentWrap } from '@/components/ContentWrap'
import { useI18n } from '@/hooks/web/useI18n'
import { Table, TableColumn } from '@/components/Table'
import { getTableListApi } from '@/api/table'
import { TableData } from '@/api/table/types'
import { ref } from 'vue'
// import { ElTag } from 'element-plus'
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
    label: 'Day'
  },
  {
    field: 'title',
    label: 'Area Name',
    sortable: true
  },
  {
    field: 'title',
    label: 'Area Code'
  },
  {
    field: 'title',
    label: 'Customer Code'
  },
  {
    field: 'display_time',
    label: 'Time'
  },
  {
    field: 'title',
    label: 'Customer Name'
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
  <ContentWrap :title="'Warehouse Conditional Schedule List'">
    <Table
      :columns="columns"
      :data="tableDataList"
      :loading="loading"
      :defaultSort="{ prop: 'display_time', order: 'descending' }"
    />
  </ContentWrap>
</template>
