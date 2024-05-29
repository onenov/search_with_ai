<script setup lang="ts">
import { RiArrowRightLine } from '@remixicon/vue';
import { useI18n } from 'vue-i18n';
type Emits = {
  (e: 'search', val: string): void,
}

type Props = {
  loading: boolean
  autofocus: boolean
}
const { t } = useI18n();

const props = defineProps<Props>();

const emits = defineEmits<Emits>();

const query = defineModel<string>();

const onSearch = () => {
  if (!query.value?.trim()) return;  
  emits('search', query.value.trim());
};
</script>

<template>
  <div id="searchbar">
    <div class="grow overflow-hidden rounded-3xl border-2 border-zinc-100 dark:border-white p-1 bg-white dark:bg-transparent flex items-center justify-center gap-2">
      <t-input id="search" v-model="query" :disabled="props.loading" clearable :autofocus="autofocus" :maxlength="100" size="large" :placeholder="t('tips.search')" @enter="onSearch">
      </t-input>
      <div class="w-10">
        <t-button class="right-buttom" :disabled="loading" shape="square" variant="base" @click="onSearch">
            <template #icon><RiArrowRightLine /></template>
          </t-button>
      </div>
    </div>
    <div class="grow-0">
      <slot />
    </div>
  </div>
</template>


<style scoped>
#searchbar {
  --td-radius-default: 25px;
  --td-size-5:20px
}
.right-buttom{
  margin-right: -8px;
}
</style>
