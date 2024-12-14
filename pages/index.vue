<template>
  <main class="main m-3 m-md-5 mb-5">
    <section>
      <h2 style="font-size: 24px; padding-left: 20px; margin-bottom: 12px">
        ขนาด
      </h2>
      <div class="row row-cols-3">
        <div
          v-for="(data, index) in layoutOption"
          :key="data.id"
          class="col ba"
        >
          <LayoutButton
            :data="data"
            @click="handleSelectLayout"
            :data-select="layoutSelected"
          />
        </div>
      </div>
    </section>
    <section class="mt-4 accordion" id="category-accordion">
      <h2 class="accordion-header" id="category-heading">
        <button
          class="accordion-button"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#collapseOne"
          aria-expanded="true"
          aria-controls="collapseOne"
        >
          หมวดหมู่
        </button>
      </h2>
      <!-- <h2>หมวดหมู่</h2> -->
      <div
        id="collapseOne"
        class="accordion-collapse collapse show"
        aria-labelledby="headingOne"
        data-bs-parent="#accordionExample"
      >
        <div class="accordion-body">
          <div class="row row-cols-3">
            <div v-for="(data, index) in categoryOption" class="col p-2">
              <CategoryButton
                :data="data"
                :key="index"
                :data-select="categorySelected"
                @click="handleSelectCategory"
              />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="mt-4 accordion" id="style-image-accordion">
      <h2 class="accordion-header" id="style-image-handing">
        <button
          class="accordion-button"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#collapseTwo"
          aria-expanded="true"
          aria-controls="collapseTwo"
        >
          สไตล์ภาพ
        </button>
      </h2>
      <div
        id="collapseTwo"
        class="accordion-collapse collapse show"
        aria-labelledby="headingOne"
        data-bs-parent="#accordionExample"
      >
        <div class="accordion-body">
          <div class="row row-cols-3">
            <div v-for="(data, index) in stylesImageList" class="col p-2">
              <StyleImageBox :data="data" :key="index" />
            </div>
          </div>
        </div>
        <div class="d-flex justify-content-center mt-2">
          <div
            @click="nextPage"
            class="btn btn-primary"
            style="margin: auto"
            :disabled="loading"
          >
            {{ loading ? "กำลังโหลด..." : "โหลดเพิ่ม" }}
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import LayoutButton from "~/components/LayoutButton.vue";
import CategoryButton from "~/components/CategoryButton.vue";
import StyleImageBox from "~/components/StyleImageBox.vue";
import type { layoutType, styleImageType } from "~/types";
import { createClient } from "pexels";

const layoutOption = ref<layoutType[]>([
  { id: 1, name: "แนวตั้ง", icon: "crop_portrait" },
  { id: 2, name: "แนวนอน", icon: "crop_16_9" },
  { id: 3, name: "จตุรัส", icon: "crop_square" },
]);
const categoryOption = ref([
  "การงาน",
  "การเงิน",
  "ความรัก",
  "สุขภาพ",
  "ความมั่นใจ",
  "ความสำเร็จ",
  "วาสนา",
  "ศัตรู",
  "ความหวัง",
  "ความสุข",
  "การเรียน",
  "หายนะ",
  "ครอบครัว/ญาติ",
  "ความเชื่อมั่น",
  "ชื่อเสียง",
  "โชคลาภ",
  "เสริมเสน่ห์",
  "อำนาจ/บารมี",
  "ความปลอดภัย",
  "เพื่อน/มิตรสหาย",
]);

const page = ref<number>(1);
const perPage = ref<number>(12);
const loading = ref(false);

const stylesImageList = useState<styleImageType[]>("stylesImageList", () => []);

const layoutSelected = useState<layoutType>("layoutSelect", () => ({
  id: 0,
  name: "",
  icon: "",
}));
const categorySelected = useState<string[]>("categorySelect", () => []);

const fetchCuratedPhotos = async (page = 1, perPage = 15) => {
  const API_URL = `https://api.pexels.com/v1/curated?page=${page}&per_page=${perPage}`;
  const API_KEY = "j7ntHIrEuMWWRbFJhmGIy0eDC4Jbi8VkukfF2XWbRLwYXbjulRy4RLit";

  try {
    const response = await fetch(API_URL, {
      headers: {
        Authorization: API_KEY,
      },
    });

    if (!response.ok) {
      throw new Error(`Error: ${response.status} - ${response.statusText}`);
    }

    const data = await response.json();

    const newData = data?.photos?.map((photo: any) => ({
      id: photo?.id,
      name: photo?.photographer,
      imageUrl: photo?.src?.medium,
    }));

    stylesImageList.value = [...stylesImageList.value, ...newData];
    return data;
  } catch (error) {
    console.error("Error fetching curated photos:", error);
    return null;
  }
};

const nextPage = async () => {
  if (loading.value) return;
  loading.value = true;
  page.value += 1;
  await fetchCuratedPhotos(page.value, perPage.value);
  loading.value = false;
};

const handleSelectLayout = (dataSelect: layoutType) => {
  console.log("Click");
  layoutSelected.value = dataSelect;
  console.log(layoutSelected);
};

const handleSelectCategory = (dataSelect: string) => {
  console.log("Click");
  if (categorySelected.value.includes(dataSelect)) {
    categorySelected.value = categorySelected.value.filter(
      (x: string) => x !== dataSelect
    );
  } else categorySelected.value.push(dataSelect);
  console.log(layoutSelected);
};

onMounted(() => {
  fetchCuratedPhotos(page.value, perPage.value);
});
</script>

<style>
.accordion-button {
  font-size: 24px;
}
</style>
