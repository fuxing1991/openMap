# fx-amountInput组件

### 使用方式

#### 在 scripts 中引用组件:

```
import fxAmountInput from '@/components/fx-amountInput/fx-amountInput.vue';
export default {
    components: {
        fxAmountInput
    }
}
</script>

```

#### 在 template  中引用组件:
```
<fx-amountInput></fx-amountInput>
<fx-amountInput :currency="currency" :fontSize="fontSize" :confirmText="confirmText" :btnColor="btnColor" :placeholder="placeholder" :maxNumber="maxNumber" :isBold="isBold" :isFilter="isFilter" @change="change" @confirm="confirm"></fx-amountInput>
```

### 属性说明:


| 属性            | 类型                     | 默认值 | 描述                                       |
| --------------- | ------------------------ | ---- | ------------------------------------------ |
| currency        | String                   | ￥   | 金额类型（默认为人民币）                                   |
| fontSize  | Number，String                   | 30   | 字体大小                                   |
| confirmText      |  String                | 充值   | 按钮文字                                   |
| btnColor          | String                 | #2B76EF   | 按钮背景色 |
| placeholder        | String                   | 请输入充值金额   | 占位符                                   |
| maxNumber  | Number                   | 100000000000  | 最大输入金额                                   |
| isBold      |  Boolean                | true   | 输入框字体加粗                                   |
| isFilter          | Boolean                 | true   | 金额格式化（格式化后：9,999,999.12） |

### 事件说明


| 事件名称           | 说明                     | 返回值 |
| --------------- | ------------------------ | ---- |
| change        | 输入框值改变时触发的事件，实时返回输入的金额                   | value  |
| confirm        | 点击提交按钮时触发的提交事件                  | value  |