<template>
  <div class="card">
    <div class="header">
      <slot name="header"></slot>
    </div>
    <div class="container">
      <slot name="tasks"></slot>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Card',
  data: () => ({}),
}
</script>
<style lang="css" scoped>
.card {
  background: #ffffff;
  box-shadow: 0px 3.19px 31.92px rgba(212, 210, 210, 0.25);
  border-radius: 5px;
  padding: 5px;
  transition: 0.3s;
  height: fit-content;
  max-width: 100%;
}
.header {
  font-family: Roboto;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 16px;
  color: #0b2958;
  border-bottom: 0.2px solid #bcc5e6;
  max-width: 100%;
}
.container {
  font-family: Rubik;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 17px;
  color: #2c2828;
  max-width: 100%;
}
</style>
