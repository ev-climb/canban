<template>
  <div class="card" :style="`box-shadow: 0px 0px 27px 0px ${borderColor}`">
    <v-tooltip text="Редактировать товар">
      <template v-slot:activator="{ props }">
        <v-btn
          v-bind="props"
          icon="mdi-pencil"
          density="compact"
          variant="tonal"
          class="edit-btn"
          color="green"
          @click="isEditCardDialogOpen = true" />
      </template>
    </v-tooltip>
    <v-tooltip text="Переместить товар">
      <template v-slot:activator="{ props }">
        <v-btn
          v-bind="props"
          icon="mdi-arrow-right"
          density="compact"
          variant="tonal"
          class="to-right-btn"
          color="grey"
          @click="toNextList" />
      </template>
    </v-tooltip>
    <v-tooltip text="Переместить товар">
      <template v-slot:activator="{ props }">
        <v-btn
          v-bind="props"
          icon="mdi-arrow-left"
          density="compact"
          variant="tonal"
          class="to-left-btn"
          color="grey"
          @click="toPreviousList" />
      </template>
    </v-tooltip>
    <v-tooltip text="Удалить товар">
      <template v-slot:activator="{ props }">
        <v-btn
          v-bind="props"
          icon="mdi-delete"
          density="compact"
          variant="tonal"
          class="delete-btn"
          color="red"
          @click="isDeleteCardDialogOpen = true" />
      </template>
    </v-tooltip>
    <v-tooltip text="Сортировка по рейтингу">
      <template v-slot:activator="{ props }">
        <v-btn
          v-bind="props"
          icon="mdi-sort"
          density="compact"
          variant="tonal"
          class="sort-btn"
          color="blue"
          @click="sortList" />
      </template>
    </v-tooltip>

    <img :src="props.card.image" alt="изображение товара" />
    <div class="info">
      <p><b>ID</b>: {{ props.card.id }}</p>
      <p><b>Название</b>: {{ props.card.title }}</p>
      <p>
        <b>Описание</b>:
        {{ props.card.description }}
      </p>
      <p><b>Категория</b>: {{ props.card.category }}</p>
      <p><b>Цена</b>: {{ props.card.price }}</p>
      <p><b>Рейтинг</b>: {{ props.card.rating ? props.card.rating.rate : 0 }}</p>
    </div>
    <CardForm
      title="Редактирование карточки"
      v-model="isEditCardDialogOpen"
      :form="props.card"
      @save-card="saveCard"
      @close-form="isEditCardDialogOpen = false" />
    <v-dialog width="500px" v-model="isDeleteCardDialogOpen">
      <v-card class="d-flex align-center pa-5">
        <v-card-title>Удалить карточку?</v-card-title>
        <v-card-actions class="justify-space-between" style="width: 400px">
          <v-btn
            variant="outlined"
            color="red"
            @click="
              () => {
                $emit('deleteCard');
                isDeleteCardDialogOpen = false;
              }
            "
            >Да</v-btn
          >
          <v-btn variant="outlined" @click="isDeleteCardDialogOpen = false">Нет</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup>
  import { ref, inject, computed } from 'vue';
  import CardForm from './CardForm.vue';

  const firstList = inject('firstList');
  const secondList = inject('secondList');
  const lastList = inject('lastList');

  const props = defineProps({
    card: {},
    options: {},
  });

  const borderColor = computed(() => {
    return props.options.id === 1 ? 'blue' : props.options.id === 2 ? 'yellow' : 'pink';
  });

  let cards = ref([]);

  function getLocalCards() {
    switch (props.options.id) {
      case 1:
        cards = firstList;
        break;
      case 2:
        cards = secondList;
        break;
      case 3:
        cards = lastList;
        break;
    }
  }

  const isDeleteCardDialogOpen = ref(false);
  const isEditCardDialogOpen = ref(false);

  function saveCard() {
    isEditCardDialogOpen.value = false;
    //По готовности бекенда добавить сохранение на сервер
  }
  function toNextList() {
    getLocalCards();
    if (cards.value === firstList.value) {
      secondList.value.unshift(props.card);
      firstList.value = firstList.value.filter((card) => card.id !== props.card.id);
    } else if (cards.value === secondList.value) {
      lastList.value.unshift(props.card);
      secondList.value = secondList.value.filter((card) => card.id !== props.card.id);
    }
  }

  function toPreviousList() {
    getLocalCards();
    if (cards.value === lastList.value) {
      secondList.value.unshift(props.card);
      lastList.value = lastList.value.filter((card) => card.id !== props.card.id);
    } else if (cards.value === secondList.value) {
      firstList.value.unshift(props.card);
      secondList.value = secondList.value.filter((card) => card.id !== props.card.id);
    }
  }

  function sortList() {
    getLocalCards();
    cards = cards.value.sort((a, b) => b.rating.rate - a.rating.rate);
  }
</script>

<style lang="scss" scoped>
  .card {
    margin-top: 20px;
    padding: 10%;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 50px;
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
    position: relative;
    cursor: pointer;
    .edit-btn {
      position: absolute;
      right: 20px;
      top: 20px;
    }

    .to-right-btn {
      position: absolute;
      right: 20px;
      top: 60px;
    }
    .to-left-btn {
      position: absolute;
      right: 20px;
      top: 100px;
    }
    .sort-btn {
      position: absolute;
      right: 20px;
      top: 140px;
    }
    .delete-btn {
      position: absolute;
      right: 20px;
      bottom: 20px;
    }
    img {
      width: 50%;
      border-radius: 10px;
    }
    .info {
      margin-top: 10px;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: start;
      p {
        margin-top: 10px;
      }
    }
  }
</style>
