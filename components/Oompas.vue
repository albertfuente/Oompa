<template>
  <div>
    <div class="oompa-component-title-container">
      <img src="../static/icon.png" alt="icon" class="icon" />
      <h1>Oompa Loompa's crew</h1>
    </div>

    <div class="oompa-component-search-container">
      <input v-model="message" placeholder="Search" />
      <img src="../static/search.png" alt="serach" class="search" />
    </div>

    <div class="coompas-component-title-container">
      <h1>Find your Oompa Loompa</h1>
      <h3>There are more than 100k</h3>
    </div>

    <div v-if="filteredOompas.length > 0" class="oompas-component-wrapper">
      <div
        class="oompas-component-container"
        v-for="(oompa, index) in filteredOompas"
        :key="index"
      >
        <div
          class="oompa-component-container"
          @click="onClick(oompa.first_name)"
        >
          <img :src="oompa.image" class="oompa-component-image" alt="image" />
          <div class="oompa-component-name">{{ oompa.first_name }}</div>
          <div class="oompa-component-gender">
            {{ oompa.gender === "F" ? "Female" : "Man" }}
          </div>
          <div class="oompa-component-profession">{{ oompa.profession }}</div>
        </div>
      </div>
    </div>

    <div v-if="filteredOompas.length === 0" class="oompas-component-wrapper">
      <div
        class="oompas-component-container"
        v-for="(oompa, index) in oompas"
        :key="index"
      >
        <div
          class="oompa-component-container"
          @click="onClick(oompa.first_name)"
        >
          <img :src="oompa.image" class="oompa-component-image" alt="image" />
          <div class="oompa-component-name">{{ oompa.first_name }}</div>
          <div class="oompa-component-gender">
            {{ oompa.gender === "F" ? "Female" : "Man" }}
          </div>
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
      oompas: [],
      message: "",
      filteredOompas: [],
    };
  },
  methods: {
    onClick(id) {
      window.location.href = `user/${id}`;
    },
    async fetch() {
      const result = await this.$axios.get(
        `https://2q2woep105.execute-api.eu-west-1.amazonaws.com/napptilus/oompa-loompas`
      );
      this.oompas = result.data.results;
    },
  },
  mounted() {
    this.fetch();
  },
  watch: {
    message: function (value) {
      return (this.filteredOompas = this.oompas.filter(
        (result) =>
          result.first_name.startsWith(value) ||
          result.profession.startsWith(value)
      ));
    },
  },
};
</script>
<style >
.oompa-component-title-container {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
  background: grey;
  align-items: center;
}
.coompas-component-title-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.oompas-component-container {
  margin: 20px;
  width: 250px;
}
.oompa-component-image {
  width: 250px;
}
.oompa-component-name {
  font-weight: bold;
}
.oompas-component-wrapper {
  display: flex;
  flex-wrap: wrap;
}
.search {
  width: 22px;
  margin-left: 5px;
}
.icon {
  width: 50px;
  height: 44px;
  margin-right: 10px;
}
.oompa-component-search-container {
  display: flex;
  justify-content: end;
  padding-right: 150px;
}
</style>