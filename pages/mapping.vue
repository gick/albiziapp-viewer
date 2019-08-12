<template>
  <v-container>
    <v-flex class="text-center">
      <v-card :elevation="10">
        <v-card-text>
          <p class="text-center ma-0">Select rows to display records on the map</p>
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
        persistent-hint
      ></v-select>
      <v-card>
        <v-img
          v-if="image.length>0"
          :src="'/api/image/'+image"
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
        show-select
        :items="data"
        :dense="true"
        :items-per-page="10"
        item-key="_id"
        v-model="selected"
        :search="search"
        class="elevation-1"
      ></v-data-table>
    </v-flex>
    <v-flex>
      <div id="map-wrap" style="height: 80vh">
        <no-ssr>
          <l-map :zoom="13" :center="[47.413220, -1.219482]">
            <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
            <v-marker-cluster>
              <l-marker
                v-for="i in selected"
                :lat-lng="[i.location.coordinates[1], i.location.coordinates[0]]"
              ></l-marker>
            </v-marker-cluster>
          </l-map>
        </no-ssr>
      </div>
    </v-flex>
  </v-container>
</template>
<style >
.v-menu__content.theme--dark.menuable__content__active {
  z-index: 1000 !important;
}
</style>
<script>
import Vue2LeafletMarkerCluster from "vue2-leaflet-markercluster";

export default {
  components: {
    "v-marker-cluster": Vue2LeafletMarkerCluster
  },

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
      startHeader: ["Author", "Species", "Genus"],
      selected: [],
      headers: [
        {
          text: "Author",
          align: "left",
          value: "authorName"
        },
        { text: "Species", align: "left", value: "specie" },
        { text: "Genus", align: "left", value: "genus" },
      ]
    };
  },
  methods: {
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

