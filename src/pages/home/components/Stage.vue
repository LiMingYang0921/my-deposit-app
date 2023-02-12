<script lang="ts">
import { ref, reactive, defineComponent } from 'vue'


export default defineComponent({
  setup() {

    const rules = reactive({
      startDate: [{ required: true, message: '请选择第一阶段开始日期' }],
      endDate: [{ required: true, message: '请选择第一阶段结束日期' }],
      depositNow: [{ required: true, message: '请输入当前存款金额' }],
    })

    const form = reactive({
      startDate: null,
      endDate: null,
      depositNow: null,
    })

    let showPicker = ref(false);
    const datePickerOnConfirm = ({ selectedValues }) => {
      form.endDate = selectedValues.join('/')
      showPicker.value = false
    };

    let depositNowKeyboardShow = ref(false)
    const depositNowTouchstart = () => {
      depositNowKeyboardShow.value = true
    }


    return {
      form,
      rules,
      datePickerOnConfirm,
      showPicker,
      minDate: new Date(),
      maxDate: new Date(2055, 5, 1),
      depositNowKeyboardShow,
      depositNowTouchstart,
    }
  }
})
</script>

<template>
  <h6 class="stage_title">第一阶段</h6>
  <van-cell-group inset>

    <van-field v-model="form.startDate" is-link readonly name="datePicker" label="开始日期" placeholder="选择第一阶段开始日期"
      @click="showPicker = true" :rules="rules.startDate" />
    <van-popup v-model:show="showPicker" position="bottom">
      <van-date-picker :min-date="minDate" :max-date="maxDate" @confirm="datePickerOnConfirm"
        @cancel="showPicker = false" :columns-type="['year', 'month']" />
    </van-popup>

    <van-field v-model="form.endDate" is-link readonly name="datePicker" label="结束日期" placeholder="选择第一阶段结束日期"
      @click="showPicker = true" :rules="rules.endDate" />
    <van-popup v-model:show="showPicker" position="bottom">
      <van-date-picker :min-date="minDate" :max-date="maxDate" @confirm="datePickerOnConfirm"
        @cancel="showPicker = false" :columns-type="['year', 'month']" />
    </van-popup>

    <van-field label="月存款" v-model="form.depositNow" type="number" placeholder="预计月存款" name="depositNow"
      :rules="rules.depositNow" @touchstart.stop="depositNowTouchstart" right-icon="gold-coin-o" readonly clickable />

  </van-cell-group>

  <van-number-keyboard v-model="form.depositNow" :show="depositNowKeyboardShow" theme="custom" extra-key="-"
    close-button-text="完成" @blur="depositNowKeyboardShow = false" />

</template>

<style scoped lang="scss">
.header {
  margin-bottom: 16px;
}

.stage_title {
  margin: 0;
  padding: 16px 16px 4px;
  color: #969799;
  font-weight: 400;
  font-size: 14px;
  line-height: 16px;
}
</style>
