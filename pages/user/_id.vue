<template>
  <div>
    <div class="oompa-component-title-container">
      <img
        src="../../static/icon.png"
        class="oompa-component-back"
        alt="back"
        @click="onBack()"
      />
      <h1>Oompa Loompa's crew</h1>
    </div>
    <div
      class="oompa-component-container"
      v-for="(oompa, index) in oompaDetails"
      :key="index"
    >
      <div class="oompa-component-container" @click="onClick(oompa.first_name)">
        <img :src="oompa.image" class="oompa-component-image" alt="image" />
        <div class="oompa-details-container">
          <div class="oompa-component-name">{{ oompa.first_name }}</div>
          <div class="oompa-component-profession">{{ oompa.profession }}</div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      oompaDetails: [],
      idOompa: this.$route.params.id,
    };
  },
  methods: {
    onClick(id) {
      window.location.href = `user/${id}`;
    },
    onBack() {
      window.location.href = "/";
    },
    async fetch() {
      const result = await this.$axios.get(
        `https://2q2woep105.execute-api.eu-west-1.amazonaws.com/napptilus/oompa-loompas`
      );
      this.oompaDetails = result.data.results.filter(
        (result) => result.first_name === this.idOompa
      );
    },
  },
  mounted() {
    this.fetch();
  },
};
</script>
<style scoped>
.oompa-component-title-container {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
  background: grey;
  align-items: center;
}
.oompa-component-container {
  display: flex;
  justify-content: center;
}
.oompa-component-image {
  width: 400px;
}
.oompa-component-back {
  cursor: pointer;
  margin-right: 10px;
  width: 50px;
  height: 44px;
}
.oompa-details-container {
  margin-left: 10px;
}
</style>