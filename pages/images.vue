<template>
  <v-layout>
    <v-flex class="text-center">
      <v-card :elevation="10">
        <v-card-text>
          <p class="text-center ma-0">Click on a row with image to 'Yes' to display the image</p>
        </v-card-text>
      </v-card>

      <v-select
        dark
        item-color="grey"
        v-model="startHeader"
        v-on:change="setHeader"
        :items="headersList"
        item-text="text"
        :item-value="['text','value']"
        :menu-props="{ maxWidth: '400' }"
        label="Select column"
        multiple
        hint="Select colomns"
      ></v-select>
      <v-card>
        <v-img
          v-if="image.length>0"
          :src="'https://albiziapp.reveries-project.fr/api/image/'+image"
          style="max-width:300px;margin:auto;"
        ></v-img>

        <v-card-title>
          Observations
          <v-spacer></v-spacer>
          <v-text-field v-model="search" label="Search" single-line hide-details></v-text-field>
        </v-card-title>
      </v-card>
      <v-data-table
        :headers="headers"
        multi-sort
        :items="data"
        :dense="true"
        :items-per-page="10"
        @click:row="setImage"
        item-key="_id"
        v-model="selected"
        :search="search"
        class="elevation-1"
      ></v-data-table>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  async asyncData({ $axios }) {
    console.log(this);
    let data = await $axios.$get("https://albiziapp.reveries-project.fr/api/observation");
    return {
      data: data.map(x => {
        if (x.hasImage) {
          x.hasImage = "Yes";
          return x;
        }
        x.hasImage = "No";
        return x;
      })
    };
  },
  data() {
    return {
      image: "",
      search: "",
      headersList: [
        {
          text: "Author",
          align: "left",
          value: "authorName"
        },
        { text: "Species", align: "left", value: "specie" },
        { text: "Genus", align: "left", value: "genus" },
        { text: "Image", align: "left", value: "hasImage" },
        { text: "Common", align: "left", value: "common" },
        { text: "Confidence", align: "left", value: "confidence" },
        { text: "Date", align: "left", value: "date" }
      ],
      startHeader: ["Author", "Species", "Genus", "Image"],
      selected: [],
      headers: [
        {
          text: "Author",
          align: "left",
          value: "authorName"
        },
        { text: "Species", align: "left", value: "specie" },
        { text: "Genus", align: "left", value: "genus" },
        { text: "Image", align: "left", value: "hasImage" }
      ]
    };
  },
  methods: {
    setImage(e) {
      if (e.hasImage == "Yes") {
        this.image = e._id;
        return;
      }
      this.image = "";
    },
    setHeader(e) {
      let newHeaders = [];
      for (let i of e) {
        let header = this.headersList.filter(v => v.text == i)[0];
        newHeaders.push(header);
      }
      this.headers = newHeaders;
    }
  }
};
</script>

