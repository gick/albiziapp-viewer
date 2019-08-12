<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <div class="text-center">
        <vuetify-logo />
      </div>
      <v-card>
        <v-card-title class="headline">
          Welcome to the Albiziapp Viewer
        </v-card-title>
        <v-card-text>
          <p>Albiziapp is a collaborative tree mapping application. This explorer allows to visualize data from Albiziapp</p> 
          <p>
            For more information on Albiziapp, check out <a
              href="https://info.albiziapp.reveries-project.fr"
              target="_blank"
            > this site
            </a>.
          </p>
          <p>Thank you for using Albiziapp.</p>
          <div class="text-xs-right">
            <em><small>&mdash; Pierre-Yves Gicquel</small></em>
          </div>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  }
}
</script>
