<script lang="ts">
import Stage from './components/Stage.vue'
import { ref, reactive, defineComponent } from 'vue'

export default defineComponent({
  components: { Stage },
  setup() {

    const rules = reactive({
      birthday: [{ required: true, message: '请选择生日' }],
      depositNow: [{ required: true, message: '请输入当前存款金额' }],
      interestRate: [{ required: true, message: '请输入平均年利率' }]
    })

    const form = reactive({
      birthday: null,
      depositNow: null,
      interestRate: null
    })

    let showPicker = ref(false);
    const datePickerOnConfirm = ({ selectedValues }) => {
      form.birthday = selectedValues.join('/')
      showPicker.value = false
    };

    let depositNowKeyboardShow = ref(false)
    const depositNowTouchstart = () => {
      interestRateKeyboardShow.value = false
      depositNowKeyboardShow.value = true
    }

    let interestRateKeyboardShow = ref(false)
    const interestRateTouchstart = () => {
      depositNowKeyboardShow.value = false
      interestRateKeyboardShow.value = true
    }

    return {
      form,
      rules,
      datePickerOnConfirm,
      showPicker,
      minDate: new Date(1963, 0, 1),
      maxDate: new Date(2013, 5, 1),
      depositNowKeyboardShow,
      depositNowTouchstart,
      interestRateKeyboardShow,
      interestRateTouchstart
    }
  }
})
</script>

<template>
  <van-nav-bar class="header" title="未来存款估算" />
  <van-form :scroll-to-error="true">
    <van-cell-group inset>

      <van-field v-model="form.birthday" is-link readonly name="datePicker" label="生日" placeholder="选择出生年月"
        @click="showPicker = true" :rules="rules.birthday" />
      <van-popup v-model:show="showPicker" position="bottom">
        <van-date-picker :min-date="minDate" :max-date="maxDate" @confirm="datePickerOnConfirm"
          @cancel="showPicker = false" :columns-type="['year', 'month']" />
      </van-popup>

      <van-field v-model="form.depositNow" type="number" placeholder="当前存款" name="depositNow" label="存款"
        :rules="rules.depositNow" @touchstart.stop="depositNowTouchstart" right-icon="gold-coin-o" readonly clickable />

      <van-field label="年利率" v-model="form.interestRate" type="number" placeholder="预计投资/理财平均年利率(单利)"
        name="interestRate" :rules="rules.interestRate" @touchstart.stop="interestRateTouchstart" right-icon="discount"
        readonly clickable />

    </van-cell-group>

    <Stage />

    <div style="margin: 16px">
      <van-button round block type="primary" native-type="submit">
        开始计算
      </van-button>
    </div>
  </van-form>

  <van-number-keyboard v-model="form.depositNow" :show="depositNowKeyboardShow" theme="custom" extra-key="-"
    close-button-text="完成" @blur="depositNowKeyboardShow = false" />

  <van-number-keyboard v-model="form.interestRate" :show="interestRateKeyboardShow" theme="custom" extra-key="."
    close-button-text="完成" @blur="interestRateKeyboardShow = false" />

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
