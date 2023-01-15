<template>
  <div
    ref="parent"
    class="widget-otp-parent"
    :class="{ [gap]: true }"
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
        :placeholder="placeholder"
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
        :placeholder="placeholder"
        @paste="codePasted"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
export interface Props {
  childs?: number;
  type?: "number" | "text";
  placeholder?: string;
  gap?: "otp-gap-1" | "otp-gap-2" | "otp-gap-3" | "otp-gap-4" | "otp-gap-5" | "otp-gap-6" | "otp-gap-7" | "otp-gap-8" | "otp-gap-9" | "otp-gap-10" | "otp-gap-11" | "otp-gap-12" | "otp-gap-13" | "otp-gap-14" | "otp-gap-15" | "otp-gap-16" | "otp-gap-17" | "otp-gap-18" | "otp-gap-19" | "otp-gap-20" | "otp-gap-21" | "otp-gap-22" | "otp-gap-23" | "otp-gap-24" | "otp-gap-25" | "otp-gap-26" | "otp-gap-27" | "otp-gap-28" | "otp-gap-29" | "otp-gap-30" | "otp-gap-31" | "otp-gap-32" | "otp-gap-33" | "otp-gap-34" | "otp-gap-35" | "otp-gap-36" | "otp-gap-37" | "otp-gap-38" | "otp-gap-39" | "otp-gap-40" | "otp-gap-41" | "otp-gap-42" | "otp-gap-43" | "otp-gap-44" | "otp-gap-45" | "otp-gap-46" | "otp-gap-47" | "otp-gap-48";
}

import {
  ref,
  watch,
  // getCurrentInstance,
  toRef
} from "vue";
import type { Ref } from "vue";

const props = withDefaults(defineProps<Props>(), {
  childs: 4,
  type: "number",
  placeholder: "",
  gap: "otp-gap-16"
});

const emit = defineEmits(["code", "filled"]);

//const otpWidget = getCurrentInstance();
const parent: Ref<HTMLElement | null> = ref(null);
const code: Ref<string[]> = ref([]);
const countInput: Ref<number> = toRef(props, 'childs')
const type: Ref<string> = toRef(props, 'type')

const targetIndex = (elm: HTMLElement): number => {
  const parentChilds: HTMLCollection = (elm.parentNode as HTMLElement).children;
  return [...Array.from(parentChilds)].indexOf(elm);
};
const clearedToFocus = (event: Event): void => {
  const target = (event.target as HTMLInputElement);
  (target.value = String(target.value)[0] || '');
};

const checkEvent = (event: KeyboardEvent): void => {
  event.preventDefault();
  event.stopPropagation();
  //
  const that: HTMLInputElement = event.target as HTMLInputElement;
  const index = targetIndex(that);
  const isNumber = /^\d$/i.test(event.key) && type.value === 'number';
  const isText = /(^\D)|(^\d)$/i.test(event.key) && type.value === 'text';
  const nextSibling: HTMLInputElement = (parent.value as HTMLElement).children[index + 1] as HTMLInputElement;
  const prevSibling: HTMLInputElement = (parent.value as HTMLElement).children[index - 1] as HTMLInputElement;

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
  if (codeLength === countInput.value) {
    emit("code", sendCode);
    emit("filled");
  }
  else if (codeLength === 0) emit("code", sendCode);
}, {deep: true});

const codePasted = (e: ClipboardEvent): void => {
  const codep = String((e.clipboardData as DataTransfer).getData('Text')).trim();
  const ins = (parent.value as HTMLElement).querySelectorAll("input");
  if(codep && codep.length === countInput.value) {
    if( (type.value === "text" && /^[a-zA-Z0-9]*$/.test(codep)) || (type.value === "number" && /^[0-9]*$/.test(codep))) {
      ins.forEach((node, index)=>{
        node.value = codep[index];
        code.value.push(String(codep[index]));
        node.focus();
      })
    }
  }
};

defineExpose({
  parent
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
    max-width: 24px;
    padding: 12px 0;
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
      max-width: 36px;
      padding: 12px 0;
    }
    border: 0;
    outline: none;
    background: transparent;
    appearance: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    padding: 12px;
    border: 1px solid gray;
    border-radius: 3px;
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
