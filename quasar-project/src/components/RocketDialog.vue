<template>
  <div class="flex-col justify-center items-center">
    <div class="flex-row justify-center items-center title">
      {{ dialogItem.name }}
    </div>
    <div class="">{{ formatDate(dialogItem.date_utc) }}</div>

    <q-badge
      rounded
      color="primary"
      :label="dialogItem.crew.length"
      class="q-px-md q-py-sm"
    />
    <div>{{ dialogItem.name }}</div>
    <img :src="dialogItem.links.patch.large" class="large-img" />
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";
import dayjs from "dayjs";

export default defineComponent({
  props: {
    dialogItem: {},
  },
  setup() {
    const formatDate = (date) => {
      const formated = dayjs(date).format("dddd, MMMM D, YYYY hh:mm:ss ZZ");
      return formated;
    };
    return { formatDate };
  },
});
</script>
<style scoped lang="scss">
.large-img {
  max-width: 500px;
  width: 100%;
  height: auto;
}

body {
  background: #f3f4f6;
}

.img-icon {
  width: 30px;
  height: auto;
}
.title {
  font-size: 20px;
  font-weight: 600;
}

.container {
  max-width: 1000px;
  width: 100%;
  padding: 1rem 3rem;
}

.launches-card {
  width: 100%;
  margin: 10px 0;
  padding: 15px;
  background: #fff;
}
.all-btn {
  background: #34359d;
  color: white;

  height: 2.5rem;
}

.other-btn {
  background: #f3f4f6;
  color: #99999b;
  height: 2.5rem;
}
.flex-row {
  display: flex;
  flex-direction: row;
}
.flex-col {
  display: flex;
  flex-direction: column;
}
.flex {
  display: flex;
}
</style>
