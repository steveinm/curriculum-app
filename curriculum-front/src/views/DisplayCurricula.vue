<template>
  <v-row no-gutters class="display-curricula-page">
    <v-col
      md="6"
      offset-md="3"
      sm="8"
      offset-sm="2"
    >
      <div class="page-header">
        <h1>All Curricula</h1>
        <v-btn @click="$router.push('/curricula/create')">
          Create New
        </v-btn>
      </div>

      <div class="curricula-list">
        <v-card
          class="curriculum-card"
          outlined
          v-for="curriculum in curricula"
          :key="curriculum._id"
        >
          <v-card-title class="headline">
            <router-link :to="`/curricula/${curriculum._id}`">{{ curriculum.name }}</router-link>
          </v-card-title>
          
          <v-card-subtitle>
            {{ curriculum.description }}
          </v-card-subtitle>
          
          <v-card-text>
            <v-progress-linear
              :value="retrieveCompleted(curriculum._id)"
              color="blue-grey"
              height="25"
            >
              <template v-slot="{ value }">
                <strong>{{ value }}%</strong>
              </template>
            </v-progress-linear>
          </v-card-text>
        </v-card>
      </div>
    </v-col>
  </v-row>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  name: 'DisplayCurricula',
  data() {
    return {
      ratioCompleted: 35
    }
  },
  computed: {
    ...mapState(['curricula', 'completeCounts'])
  },
  methods: {
    ...mapActions(['getCurricula', 'countAllCompleted']),
    retrieveCompleted(id) {
      const totals = this.completeCounts.find((obj) => {
        return obj.id === id
      })
      return Math.floor((totals.numberCompleted / totals.totalNumber) * 100)
    }
  },
  mounted() {
    this.getCurricula()
  },
  created() {
    this.countAllCompleted()
  }
}
</script>
