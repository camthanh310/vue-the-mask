<script setup>
import { reactive } from "vue";
import { vMaska } from "maska";

const cached = reactive({
  data: [],
  masked: [],
  unmasked: [],
  position: -1,
  pattern: "###-##-####",
});

const options = reactive({
  mask: (value) => {
    // console.log("hook mask");
    // console.log(cached.pattern);
    // return cached.pattern;
    cached.pattern = "###-##-####";
    if (value.length > 3 && value.length <= 5) {
      cached.pattern = "***-##-####";
    } else if (value.length >= 6) {
      cached.pattern = "***-**-####";
    }
    return cached.pattern;
  },
  preProcess: (value) => {
    if (value.length > 1 && cached.data.length === 0) {
      console.log("copy");
      console.log(value);
      const data = value.split("-");
      const data1 = data[0];

      console.log(cached.data);
    } else {
      cached.data = [...cached.data, value];
    }
    return value;
  },
  postProcess: (value) => {
    console.log("cached data length");
    console.log(cached.data.length);
    let goodValue = value;
    if (cached.data.length > 3 && cached.data.length <= 5) {
      const data = cached.data[cached.data.length - 1].split("-")[1];
      goodValue = `***-${data}`;
    } else if (cached.data.length === 6) {
      const data = value.split("")[2];
      console.log(data);
      goodValue = `***-**-${data}`;
    } else if (cached.data.length >= 7) {
      const data = cached.data[cached.data.length - 1];
      console.log(data);
      goodValue = data;
    }
    console.log(goodValue);
    return goodValue;
  },
  eager: true,
});
</script>

<template>
  123-45-6789
  <input v-maska:[options] maxlength="11" />
  <strong>
    {{ cached }}
  </strong>
</template>
