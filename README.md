# otp-widget-vue
OTP code input widget for Vue 3

## install
```sh
npm i @lbgm/otp-widget-vue
```

## Props
  Interface:
  ```ts
  interface Props {
    childs?: number;
    type?: "number" | "text";
    placeholder?: string;
    flexGap?: "otp-gap-1" | "otp-gap-2" | "otp-gap-3" | "otp-gap-4" | "otp-gap-5" | "otp-gap-6" | "otp-gap-7" | "otp-gap-8" | "otp-gap-9" | "otp-gap-10" | "otp-gap-11" | "otp-gap-12" | "otp-gap-13" | "otp-gap-14" | "otp-gap-15" | "otp-gap-16" | "otp-gap-17" | "otp-gap-18" | "otp-gap-19" | "otp-gap-20" | "otp-gap-21" | "otp-gap-22" | "otp-gap-23" | "otp-gap-24" | "otp-gap-25" | "otp-gap-26" | "otp-gap-27" | "otp-gap-28" | "otp-gap-29" | "otp-gap-30" | "otp-gap-31" | "otp-gap-32" | "otp-gap-33" | "otp-gap-34" | "otp-gap-35" | "otp-gap-36" | "otp-gap-37" | "otp-gap-38" | "otp-gap-39" | "otp-gap-40" | "otp-gap-41" | "otp-gap-42" | "otp-gap-43" | "otp-gap-44" | "otp-gap-45" | "otp-gap-46" | "otp-gap-47" | "otp-gap-48";
 }
  ```

 Default values:
 ```js
 {
  childs: 4,
  type: "number",
  placeholder: "",
  flexGap: "otp-gap-16"
 }
 ```

 ## Events
 `code`: sends value filled as `string`;

 `filled`: when all inputs are filled;

## Use
 main.ts :
 ```js
  import { OtpWidget } from '@lbgm/otp-widget-vue';

  // register as global component
  app.component('OtpWidget', OtpWidget);
 ```
 App.vue :
 ```js
 // import component style
 import '@lbgm/otp-widget-vue/style';

 import { ref } from "vue";
 import type { Ref } from "vue";

 const otp: Ref<string> = ref("");
 ```

 use component:
 ```html
    <otp-widget @filled="void 0" @code="otp = $event" />
 ```

 ```js
  console.log(otp) : 8842
 ```