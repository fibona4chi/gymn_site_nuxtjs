<template>
  <div class="container-fluid" style="background-color: #343a40!important">
    <div class="d-flex flex-column bd-highlight mb-3">
      <div class="p-2 mx-auto bd-highlight text-white bg-dark">Адрес: г. Такой-то ул. Такая-то д. 0</div>
      <div class="p-2 mx-auto bd-highlight text-white bg-dark">Телефон: 8(900)000-00-00</div>
      <div class="p-2 mx-auto bd-highlight text-white bg-dark">Инстаграм Телеграмм Воцап Вконтакте</div>
      <div class="p-2 mx-auto bd-highlight text-white bg-dark">{{ title }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header_contact",
  props: ['title']
}
</script>

<style scoped>

</style>
