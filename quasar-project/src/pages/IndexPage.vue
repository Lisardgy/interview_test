<template>
  <q-page class="row items-center justify-evenly">
    <div class="flex-col container">
      <div class="flex-row">
        <q-btn
          @click="getAll"
          label="All"
          class="all-btn col"
          unelevated
          dense
        />
        <q-btn
          @click="getLaunched"
          label="Launched"
          class="other-btn col"
          unelevated
          dense
        />
        <q-btn
          @click="getUpcoming"
          label="Upcoming"
          class="other-btn col"
          unelevated
          dense
        />
      </div>
      <div>
        <q-card
          v-ripple
          square
          class="cursor-pointer"
          v-for="(item, i) in items"
          :key="i"
        >
          <div
            class="flex-row justify-between items-center launches-card"
            @click="isOpenDialog(item)"
          >
            <div class="flex items-center">
              <q-img
                :src="item.links.patch.small"
                style="border-radius: 50%"
                class="img-icon"
              />
              <span class="card-name">{{ item.name }}</span>
            </div>
            <div class="flex-row">
              <q-badge align="top" rounded v-if="item.crew.length"
                >{{ item.crew.length }} crews</q-badge
              >
              <span class="q-mx-sm">{{ formatDate(item.date_utc) }}</span>
              <span class="text-bold text-primary" v-if="isShowUpcoming">{{
                item.upcoming == true ? "Upcoming" : "Launched"
              }}</span>
            </div>
          </div>
        </q-card>
      </div>
    </div>
    <q-dialog v-model="dialog">
      <q-card>
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">Close icon</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section>
          <RocketDialog class="dialog" :dialogItem="dialogItem" />
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { Todo, Meta } from "components/models";
import RocketDialog from "components/RocketDialog.vue";
import { defineComponent, onMounted, ref } from "vue";
import dayjs from "dayjs";
import axios from "axios";

export default defineComponent({
  name: "IndexPage",
  components: { RocketDialog },
  setup() {
    const items = ref();
    const isShowUpcoming = ref(true);
    onMounted(() => {
      getAll();
    });
    const dialog = ref(false);
    const dialogItem = ref();
    const isOpenDialog = (item) => {
      dialog.value = true;
      dialogItem.value = item;
    };

    const getApiData = async () => {
      const { data } = await axios.get(
        "https://api.spacexdata.com/v4/launches"
      );
      items.value = data;

      return data;
    };

    const getAll = async () => {
      isShowUpcoming.value = true;
      getApiData();
    };

    const getLaunched = async () => {
      isShowUpcoming.value = false;
      const apiData = await getApiData();
      const launched = apiData.filter((x) => x.upcoming == true);
      items.value = launched;
      // console.log(items.value);
      // console.log(launched);
    };

    const getUpcoming = async () => {
      isShowUpcoming.value = false;
      const apiData = await getApiData();
      const upcoming = apiData.filter((x) => x.upcoming == false);
      items.value = upcoming;
      // console.log(items.value);
      // console.log(upcoming);
    };

    const formatDate = (date) => {
      const formated = dayjs(date).format("dddd, MMMM D, YYYY ");
      return formated;
    };

    return {
      getApiData,
      getLaunched,
      getUpcoming,
      items,
      formatDate,
      isShowUpcoming,
      getAll,
      isOpenDialog,
      dialog,
      dialogItem,
    };
  },
});
</script>
<style scoped lang="scss">
.dialog {
  max-width: 600px;
  width: 100%;
  max-height: 80%;
  height: 100%;
}

.img-icon {
  width: 30px;
  height: auto;
}
.card-name {
  font-size: 16px;
  font-weight: 500;
  margin-left: 5px;
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
</style>
