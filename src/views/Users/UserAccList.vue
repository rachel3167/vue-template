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
</style>
