<template>
  <div class="image">
    <input
      :id="id"
      :value="value"
      class="custemp"
      type="text"
      style="width: 100%"
    />
  </div>
</template>

<script>
export default{
  data(){
    return{
      data:{},
      computed: {
        id: function () {
          return this.data.OrderID;
        },
        value: function () {
          return this.data.Freight;
        },
      },
    }
  }
}
</script>
