<template>
  <div
    ref="parent"
    class="widget-otp-parent otp-gap-16"
    data-widget="widget-otp-parent"
  >
    <template v-if="type === 'number'">
      <input
        v-for="(count, index) in childs"
        :key="index"
        :ref="`widget-otp-input-${index}`"
        type="number"
        min="0"
        max="9"
        step="1"
        maxlength="1"
        autocomplete="off"
        spellcheck="false"
        inputmode="decimal"
        @keyup="checkEvent"
        @input="clearedToFocus"
        @paste="codePasted"
      />
    </template>

    <template v-if="type === 'text'">
      <input
        v-for="(count, index) in childs"
        :key="index"
        :ref="`widget-otp-input-${index}`"
        type="text"
        maxlength="1"
        autocomplete="off"
        spellcheck="false"
        @keyup="checkEvent"
        @input="clearedToFocus"
        @paste="codePasted"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
export interface Props {
  childs?: number;
  type?: string;
}

import {
  ref,
  watch,
  onMounted,
  // getCurrentInstance,
  toRef
} from "vue";
import type { Ref } from "vue";

const props = withDefault(defineProps<Props>(), {
  childs: 4,
  type: "number"
});

const emit = defineEmits(["code"]);

//const otpWidget = getCurrentInstance();
const parent: Ref<HTMLElement | null> = ref(null);
const code: Ref<string[]> = ref([]);
const countInput: Ref<number> = toRef(props, 'childs')
const type: Ref<string> = toRef(props, 'type')

const targetIndex = (elm) => [...elm.parentNode.children].indexOf(elm);
const clearedToFocus = ({ target }) => (target.value = String(target.value)[0] || '');

const checkEvent = (event) => {
  event.preventDefault();
  event.stopPropagation();
  //
  const that = event.target;
  const index = targetIndex(that);
  const isNumber = /^\d$/i.test(event.key) && type.value === 'number';
  const isText = /(^\D)|(^\d)$/i.test(event.key) && type.value === 'text';
  const nextSibling = parent.value.children[index + 1];
  const prevSibling = parent.value.children[index - 1];

  if (event.key !== "Backspace" && (isNumber || isText)) {
    code.value.push(String(that.value));
    if(that.value) that.classList.add('hasvalue');
    if (nextSibling) nextSibling.focus();
  } else if (event.key === "Backspace") {
    that.value = "";
    code.value.splice(code.value.indexOf(String(that.value), 1));
    that.classList.remove('hasvalue');
    if (prevSibling) prevSibling.focus();
  }
};

//emit code
watch(code, () => {
  const codeLength = code.value.length;
  const sendCode = code.value.join("");
  if (codeLength === countInput.value) context.emit("code", sendCode);
  else if (codeLength === 0) context.emit("code", sendCode);
}, {deep: true});

const codePasted = (e) => {
  const codep = String(e.clipboardData.getData('Text')).trim();
  const ins = parent.value.querySelectorAll("input");
  if(codep && /^[0-9]*$/.test(codep) && codep.length === countInput.value) {
    ins.forEach((node, index)=>{
      node.value = codep[index];
      code.value.push(String(codep[index]));
      node.focus();
    })
  }
};

defineExpose({
  code
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
::placeholder {
  font-weight: 400;
  color: gray;
}

@for $i from 1 to 48 {
  .otp-gap-#{$i} {
    margin: -#{$i}px 0 0 -#{$i}px;
    width: calc(100% + #{$i}px);

    & > * {
      margin: #{$i}px 0 0 #{$i}px;
    }
  }
}

@-moz-document url-prefix()/*firefox*/
{
  input[type="number"] {
    max-width: 52px;
    padding: 16px 0;
  }
}

div[data-widget="widget-otp-parent"] {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  border: 0;

  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  input {
    &[type="number"] {
      -moz-appearance: textfield; //Firefox
    }
    &[type="text"] {
      max-width: 52px;
      padding: 16px 0;
    }
    border: 0;
    outline: none;
    background: transparent;
    appearance: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    padding: 16px;
    border: 2px solid gray;
    border-radius: 6px;
    text-align: center;
    cursor: pointer;

    &:hover {
      opacity: 0.9;
    }
    &:focus {
      border-color: #0075a3;
    }
    &.hasvalue {
      border-color: #41cd52;
    }
  }
}
</style>