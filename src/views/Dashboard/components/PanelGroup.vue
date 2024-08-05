<script setup lang="ts">
import { ElRow, ElCol, ElCard, ElSkeleton } from 'element-plus'
import { CountTo } from '@/components/CountTo'
import { useDesign } from '@/hooks/web/useDesign'
import { useI18n } from '@/hooks/web/useI18n'
import { ref, reactive } from 'vue'
import { getCountApi } from '@/api/dashboard/analysis'
import type { AnalysisTotalTypes } from '@/api/dashboard/analysis/types'

const { t } = useI18n()

const { getPrefixCls } = useDesign()

const prefixCls = getPrefixCls('panel')

const loading = ref(true)

let totalState = reactive<AnalysisTotalTypes>({
  users: 0,
  messages: 0,
  moneys: 0,
  shoppings: 0
})

const getCount = async () => {
  const res = await getCountApi()
    .catch(() => {})
    .finally(() => {
      loading.value = false
    })
  totalState = Object.assign(totalState, res?.data || {})
}

getCount()
</script>

<template>
  <ElRow :gutter="20" justify="start" :class="prefixCls">
    <ElCol :xl="5" :lg="4" :md="6" :sm="8" :xs="24" class="custom-col">
      <ElCard shadow="hover" class="mb-20px dashboard-card">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex dashboard-card-body`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--peoples p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:peoples" :size="40" />
                </div> -->
              </div>
              <div class="flex flex-col justify-between">
                <div :class="`${prefixCls}__item--text text-16px text-gray-500`">{{
                  t('analysis.newUser')
                }}</div>
                <CountTo
                  class="text-20px font-700 text-right custom-count-to"
                  :start-val="0"
                  :end-val="100"
                  :duration="1500"
                />
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>

    <ElCol :xl="5" :lg="4" :md="6" :sm="8" :xs="24" class="custom-col">
      <ElCard shadow="hover" class="mb-20px dashboard-card">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex dashboard-card-body`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--message p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:message" :size="40" />
                </div> -->
              </div>
              <div class="flex flex-col justify-between">
                <div :class="`${prefixCls}__item--text text-16px text-gray-500`">{{
                  t('analysis.unreadInformation')
                }}</div>
                <CountTo
                  class="text-20px font-700 text-right custom-count-to"
                  :start-val="0"
                  :end-val="30"
                  :duration="1500"
                />
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>

    <ElCol :xl="5" :lg="4" :md="6" :sm="8" :xs="24" class="custom-col">
      <ElCard shadow="hover" class="mb-20px dashboard-card">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex dashboard-card-body`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--money p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:money" :size="40" />
                </div> -->
              </div>
              <div class="flex flex-col justify-between">
                <div :class="`${prefixCls}__item--text text-16px text-gray-500`">{{
                  t('analysis.transactionAmount')
                }}</div>
                <CountTo
                  class="text-20px font-700 text-right custom-count-to"
                  :start-val="0"
                  :end-val="50"
                  :duration="1500"
                />
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>

    <ElCol :xl="5" :lg="4" :md="6" :sm="8" :xs="24" class="custom-col">
      <ElCard shadow="hover" class="mb-20px dashboard-card">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex dashboard-card-body`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--shopping p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:shopping" :size="40" />
                </div> -->
              </div>
              <div class="flex flex-col justify-between">
                <div :class="`${prefixCls}__item--text text-16px text-gray-500`">{{
                  t('analysis.totalShopping')
                }}</div>
                <CountTo
                  class="text-20px font-700 text-right custom-count-to"
                  :start-val="0"
                  :end-val="100"
                  :duration="1500"
                />
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>

    <ElCol :xl="5" :lg="4" :md="6" :sm="8" :xs="24" class="custom-col">
      <ElCard shadow="hover" class="mb-20px dashboard-card">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex dashboard-card-body`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--shopping p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:shopping" :size="40" />
                </div> -->
              </div>
              <div class="flex flex-col justify-between">
                <div :class="`${prefixCls}__item--text text-16px text-gray-500`">{{
                  t('analysis.failedDelivery')
                }}</div>
                <CountTo
                  class="text-20px font-700 text-right custom-count-to"
                  :start-val="0"
                  :end-val="10"
                  :duration="1500"
                />
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>
  </ElRow>

  <ElRow :gutter="20" justify="start" :class="prefixCls">
    <ElCol :xl="24" :lg="24" :md="24" :sm="24" :xs="24">
      <ElCard shadow="hover" class="mb-20px">
        <ElSkeleton :loading="loading" animated :rows="2">
          <template #default>
            <div :class="`${prefixCls}__item flex`">
              <div>
                <!-- <div
                  :class="`${prefixCls}__item--icon ${prefixCls}__item--peoples p-16px inline-block rounded-6px`"
                >
                  <Icon icon="svg-icon:peoples" :size="40" />
                </div> -->
              </div>

              <div class="dashboard-import-section flex">
                <div class="flex flex-col justify-between">
                  <div :class="`${prefixCls}__item--text text-36px font-700 import-title`">{{
                    t('analysis.importSpreadsheet')
                  }}</div>
                  <div
                    :class="`${prefixCls}__item--text text-16px text-gray-500 mt-5 import-description`"
                    >{{ t('analysis.importLimit') }}</div
                  >
                </div>

                <div class="flex import-btn-box">
                  <button class="el-button custom-el-button"
                    ><span>{{ t('analysis.importBtn') }}</span></button
                  >
                </div>
              </div>
            </div>
          </template>
        </ElSkeleton>
      </ElCard>
    </ElCol>
  </ElRow>
</template>

<style lang="less" scoped>
@prefix-cls: ~'@{adminNamespace}-panel';

.@{prefix-cls} {
  &__item {
    &--peoples {
      color: #40c9c6;
    }

    &--message {
      color: #36a3f7;
    }

    &--money {
      color: #f4516c;
    }

    &--shopping {
      color: #34bfa3;
    }

    // &:hover {
    //   :deep(.@{adminNamespace}-icon) {
    //     color: #fff !important;
    //   }
    //   .@{prefix-cls}__item--icon {
    //     transition: all 0.38s ease-out;
    //   }
    //   .@{prefix-cls}__item--peoples {
    //     background: #40c9c6;
    //   }
    //   .@{prefix-cls}__item--message {
    //     background: #36a3f7;
    //   }
    //   .@{prefix-cls}__item--money {
    //     background: #f4516c;
    //   }
    //   .@{prefix-cls}__item--shopping {
    //     background: #34bfa3;
    //   }
    // }
  }
}
</style>

<style>
.dashboard-card,
.dashboard-card .el-card__body {
  height: 80%;
}

.dashboard-card-body {
  height: 80%;
}

.dashboard-card-body .flex-col {
  width: 100%;
}

.custom-count-to {
  font-size: 25px;
}

.dashboard-import-section {
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.custom-el-button {
  width: 90%;
  padding: 30px;
  font-family: Nunito, sans-serif;
  font-size: 30px;
  border-radius: 12px;
}

.custom-el-button,
.custom-el-button:hover {
  color: #fff;
  background-color: #487bff;
}

.import-btn-box {
  width: 40%;
  justify-content: center;
  align-items: center;
}

.import-description {
  width: 80%;
}

@media only screen and (width >= 1200px) {
  .el-col-lg-4.custom-col {
    flex: 0 0 20%;
    max-width: 20%;
  }
}

@media only screen and (width <= 1130px) {
  .import-title {
    font-size: 30px;
  }

  .import-description {
    font-size: 14px;
  }

  .import-btn-box {
    width: 60%;
  }

  .custom-el-button {
    padding: 23px;
    font-size: 23px;
    border-radius: 12px;
  }
}

@media only screen and (width <= 1000px) {
  .import-title {
    font-size: 25px;
  }

  .import-description {
    font-size: 13px;
  }

  .import-btn-box {
    width: 70%;
    justify-content: end;
  }

  .custom-el-button {
    padding: 20px;
    font-size: 20px;
    border-radius: 12px;
  }
}

@media only screen and (width <= 850px) {
  .import-title {
    font-size: 20px;
  }

  .import-description {
    font-size: 12px;
  }

  .import-btn-box {
    width: 70%;
    justify-content: end;
  }

  .custom-el-button {
    padding: 20px;
    font-size: 16px;
    border-radius: 12px;
  }
}

@media only screen and (width <= 767px) {
  .import-title {
    font-size: 23px;
  }

  .import-description {
    font-size: 14px;
  }

  .import-btn-box {
    width: 80%;
    justify-content: end;
  }

  .custom-el-button {
    padding: 20px;
    font-size: 16px;
    border-radius: 12px;
  }
}

@media only screen and (width <= 576px) {
  .import-title {
    font-size: 20px;
  }

  .import-description {
    font-size: 13px;
  }
}

@media only screen and (width <= 500px) {
  .import-title {
    font-size: 20px;
  }

  .import-description {
    font-size: 13px;
  }

  .import-btn-box {
    width: 100%;
  }

  .custom-el-button {
    padding: 20px;
    font-size: 16px;
    border-radius: 10px;
  }
}
</style>
