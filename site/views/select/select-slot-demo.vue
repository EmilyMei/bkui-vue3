<template>
  <div class="demo">
    <div>
      <div>
        <h4>tag</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          multiple-mode="tag"
          collapse-tags
          filterable
          multiple
          show-select-all
        >
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
          <template #tag="{ selected }">
            {{ selected.map(item => `${item.label}( ${item.value} )`).join('；') }}
          </template>
        </bk-select>
      </div>
      <div>
        <h4>trigger</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          :input-search="false"
          filterable
          multiple
        >
          <template #trigger="{ selected }">
            <span>{{ selected }}</span>
          </template>
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
        </bk-select>
      </div>
      <div>
        <h4>extension</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          filterable
          @toggle="handleToggle"
        >
          <bk-option
            v-for="(item, index) in datasource"
            :disabled="item.disabled"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
          <template #extension>
            <div class="custom-extension">
              <div
                v-if="showEdit"
                style="display: flex; align-items: center"
              >
                <bk-input
                  ref="inputRef"
                  v-model="optionName"
                  size="small"
                  @enter="addOption"
                />
                <done
                  style="font-size: 22px; color: #2dcb56; cursor: pointer; margin-left: 6px"
                  @click="addOption"
                />
                <error
                  style="font-size: 16px; color: #c4c6cc; cursor: pointer; margin-left: 2px"
                  @click="showEdit = false"
                />
              </div>
              <div
                v-else
                style="display: flex; align-items: center; justify-content: center"
              >
                <span
                  style="display: flex; align-items: center; cursor: pointer"
                  @click="handleShowEdit"
                >
                  <plus style="font-size: 20px" />
                  新增
                </span>
                <span style="display: flex; align-items: center; position: absolute; right: 12px">
                  <bk-divider
                    direction="vertical"
                    type="solid"
                  />
                  <spinner
                    v-if="isLoading"
                    style="font-size: 14px; color: #3a84ff"
                  />
                  <right-turn-line
                    v-else
                    style="font-size: 14px; cursor: pointer"
                    @click="refresh"
                  />
                </span>
              </div>
            </div>
          </template>
        </bk-select>
      </div>
    </div>
    <div>
      <div>
        <h4>prefix</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          :input-search="false"
          filterable
          multiple
        >
          <template #prefix>
            <span style="padding: 0 12px; line-height: 32px; background: #ccc">运动选项</span>
          </template>
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
        </bk-select>
      </div>
      <div>
        <h4>prefix(属性)</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          :input-search="false"
          prefix="运动选项"
          filterable
          multiple
        >
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
        </bk-select>
      </div>
      <div>
        <h4>prefix(属性)</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          :auto-height="false"
          multiple-mode="tag"
          prefix="运动选项"
          collapse-tags
          filterable
          multiple
          show-select-all
        >
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          />
        </bk-select>
      </div>
    </div>
    <div>
      <div>
        <h4>optionRender</h4>
        只用使用list 列表才会有效。
        <bk-select
          class="bk-select"
          v-model="selectedValue"
          :input-search="false"
          :list="datasource"
          display-key="label"
          id-key="value"
          enable-virtual-render
          filterable
          multiple
        >
          <template #optionRender="{ item }">
            <span>{{ item.label }}</span>
          </template>
        </bk-select>
      </div>
      <div>
        <h4>optionRender</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue2"
          :input-search="false"
          :list="datasource"
          display-key="label"
          id-key="value"
          filterable
        >
          <template #optionRender="{ item }">
            <span>{{ item.label }}</span>
            <span>({{ item.value }})</span>
          </template>
        </bk-select>
      </div>
      <div>
        <h4>option</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue2"
          :input-search="false"
          filterable
          multiple
        >
          <bk-option
            v-for="(item, index) in datasource"
            :id="item.value"
            :key="index"
            :name="item.label"
          >
            <div
              style="width: 100%"
              @click.prevent.stop
            >
              <bk-input
                v-if="editOption === item.value"
                style="width: 100px"
                @enter="addOptions"
              />
              <div
                v-else
                @click="handleEditOption(item.value)"
              >
                {{ item.label }}
              </div>
            </div>
          </bk-option>
        </bk-select>
      </div>
      <!-- suffix -->
      <div>
        <h4>suffix icon</h4>
        <bk-select
          class="bk-select"
          v-model="selectedValue2"
          :input-search="false"
          :list="datasource"
          display-key="label"
          id-key="value"
          filterable
          :clearable="false"
        >
          <template #suffix>
            <Done/>
          </template>
        </bk-select>
      </div>
    </div>
  </div>
</template>
<script setup>
  import { ref } from 'vue';

  import { Done, Error, Plus, RightTurnLine, Spinner } from '@bkui-vue/lib/icon';

  const datasource = ref([
    {
      value: 1,
      label: '爬山',
    },
    {
      value: 2,
      label: '跑步',
    },
    {
      value: 3,
      label: '未知',
    },
    {
      value: 4,
      label: '健身',
    },
    {
      value: 5,
      label: '骑车',
    },
    {
      value: 6,
      label: '跳舞',
    },
  ]);
  const selectedValue = ref([1, 2, 3, 4]);
  const selectedValue2 = ref(1);

  const showEdit = ref(false);
  const handleToggle = value => {
    console.log(value);
  };
  const inputRef = ref();
  const handleShowEdit = () => {
    showEdit.value = true;
    setTimeout(() => {
      inputRef.value.focus();
    });
  };
  const optionName = ref('');
  const addOption = () => {
    if (optionName.value.trim()) {
      datasource.value.push({
        value: Math.random() + optionName.value,
        label: optionName.value,
      });
      optionName.value = '';
    }
    showEdit.value = false;
  };

  const isLoading = ref(false);
  const refresh = () => {
    isLoading.value = true;
    setTimeout(() => {
      isLoading.value = false;
    }, 2000);
  };

  const editOption = ref('');
  const handleEditOption = value => {
    editOption.value = value;
  };
  const addOptions = value => {
    datasource.value.push({
      value,
      label: value,
    });
    editOption.value = '';
  };
</script>
<style lang="postcss" scoped>
  .demo > div {
    display: flex;
    margin-bottom: 20px;
  }

  .bk-select {
    width: 300px;
    margin-right: 20px;
  }
  .custom-extension {
    width: 100%;
    color: #63656e;
    padding: 0 12px;
  }
</style>
