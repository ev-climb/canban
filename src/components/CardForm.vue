<template>
  <v-dialog width="500px">
    <v-card class="d-flex align-center pa-5">
      <v-card-title>{{ title }}</v-card-title>
      <v-text-field
        v-model="form.image"
        type="text"
        label="Введите ссылку на изображение"
        variant="underlined"
        style="width: 400px; margin-top: 20px">
      </v-text-field>
      <v-text-field
        v-model="form.id"
        type="text"
        label="Введите ID товара"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-text-field
        v-model="form.title"
        type="text"
        label="Введите заголовок товара"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-text-field
        v-model="form.description"
        type="text"
        label="Введите описание товара"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-autocomplete
        :value="form.category"
        :items="categories"
        label="Выберите категорию товара"
        variant="underlined"
        style="width: 400px">
      </v-autocomplete>
      <v-text-field
        v-model="form.price"
        type="number"
        label="Введите цену"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-text-field
        v-model="form.rating.rate"
        type="number"
        label="Введите рейтинг"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-text-field
        v-model="form.rating.count"
        type="number"
        label="Введите колличество баллов"
        variant="underlined"
        style="width: 400px">
      </v-text-field>
      <v-card-actions class="justify-space-between" style="width: 400px">
        <v-btn variant="outlined" color="green" @click="$emit('saveCard')">Сохранить</v-btn>
        <v-btn variant="outlined" @click="$emit('closeForm')">Отменить</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
  import { ref } from 'vue';
  defineProps({
    title: '',
    form: {},
  });

  const categories = ref([`men's clothing`, 'jewelery', 'electronics', `women's clothing`]);
</script>

<style lang="scss" scoped></style>
